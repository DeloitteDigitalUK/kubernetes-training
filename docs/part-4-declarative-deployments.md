# Kubernetes 101 - Part 4: Declarative deployments

In the previous section we 

## A return to declarative configuration

You may have noticed that Helm is operated using a CLI, in an _imperative_ style. We started this course espousing the benefits of declarative systems; always reconciling towards a desired state.

Modern deployment approaches, like GitOps, rely upon being able to work from infrastructure configuration as code. If we want Helm’s benefits, like templating, reusability, encapsulation and deployment history but also declarative configuration, what do we do?

Luckily the community comes to our rescue. Here are two projects that provide a way to achieve this:

- **Helmfile**: YAML formatted; supports templating and external values files, as well as the concept of different ‘environments’
- **Helmsman**: TOML formatted - like INI files
- **Terraform**: HCL formatted; commonly used infrastructure management tool

Each centers around the idea of using a configuration file in your source control system containing your desired state. This includes:

- Helm chart and version
- Helm repository
- Release values, inline and within values files

In the next section we’re going to focus on Helmfile.

## Hello Helmfile

A Helmfile is intended to be committed to your source control system. It combines a reusble chart with a given set of values.

Here is a basic example:

```
releases:
- name: team-a-jenkins
  chart: stable/jenkins
  version: 0.29.3
  values:
  - apps/team-a/jenkins.yaml
  
- name: team-b-jenkins
  chart: stable/jenkins
  version: 0.30.1
  values:
  - apps/team-b/jenkins.yaml
```

This file defines two different releases. Each uses the ‘stable/jenkins’ chart, but here teams might need different versions. In addition, any team-specific settings are separated into their own values files.

Under the covers, Helmfile delegates to Helm. When we use the Helmfile CLI to perform an action it orchestrates the required commands to install or upgrade our application without us having to provide a long string of arguments to a terminal command.

Great! We can now control Helm with this configuration file, without having to make sure we pass the same chart versions, values etc. manually whenever we release. This avoids the risk of manual error, and improves the predictability of our deployment process. This is an improvement, but we’re still not quite where we want to be.

## Are we there yet?

By itself, Helmfile is a useful way of ensuring the same values and charts can be viewed by the whole team, reviewed, audited and so on. It is, however, not *yet* declarative.

What we want is to tell our tool to just ‘apply’ our configuration from file (just as we did at the start with our static Kubernetes YAML objects).

To do this, Helmfile needs to be able to perform a diff between the current state and the desired state.

### The final step

Helmfile delegates to the `helm` command under the covers. This means that its diff functionality makes use of the `helm-diff` plugin.

# steps to install helm diff plugin

Now we’re ready to go. Let’s test it:

helmfile apply

#### What does this do?

Helmfile performs the following steps:

- synchronises your local chart cache from the remote repositories
- locates all your values files
- resolves any placeholders in your values
- invokes the `helm diff` command, passing your chart and values
- if the diff indicates the release is not installed, or is otherwise different from your desired configuration, triggers a `helm upgrade` command to update your application state

The important thing to note about the apply process is that it is idempotent. Why is this important? It means you can run `helmfile apply` any number of times, and only if changes are required will do anything.

#### What can I do with this?

This is super powerful for things like CI/CD pipelines and drift detection. You can imagine having a deployment pipeline that performs the following steps:

- check out a given tag or branch from source control
- run a Helmfile apply and your applications are rolled out smoothly

If something goes wrong, you can just revert the commit from your deployment branch, triggering the pipeline again. Now the previous configuration is applied, and your application is rolled back to the last working state.

> Aside: it’s probably worth noting that in an emergency situation you still have access to the full power of the Helm CLI if you need it. So, in our rollback example, you could instead have used `helm rollback <release> <version>` to return to a previously deployed configuration. In this scenario we would have to take care to rapidly reconcile the configuration in our version control system with the state of the system to avoid unexpectedly reapplying the same changes in the next deployment.
