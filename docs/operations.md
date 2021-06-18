# Operational considerations

- Observability
	- Logging
	- Monitoring
	- Tracing
- Advanced deployment strategies
- Scaling
- Resilience
	- HA architecture
	- Persistent workloads


## Additional objects 

We will also meet:

- DaemonSet
- StatefulSet

## Pod sandbox

- What is shared?
- What is per container?

## Logging

Challenges:

- Ephemeral workloads
- Ephemeral nodes
- Serverless nodes

Strategies:

- Sidecar per pod
- DaemonSet per node
- Embedded within application
