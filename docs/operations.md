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

## Observability

See [CNCF trailmap](https://raw.githubusercontent.com/cncf/trailmap/master/CNCF_TrailMap_latest.png)

### Logging

Challenges:

- Ephemeral workloads
- Ephemeral nodes
- Serverless nodes

Strategies:

- Sidecar per pod
- DaemonSet per node
- Embedded within application

### Monitoring

- Prometheus
- APM

### 

