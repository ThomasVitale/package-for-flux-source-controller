# Configuring High Availability

The Flux Source Controller supports high availability following an active/passive model based on the leader election strategy. Since only one instance performs work at any given time, one replica for each Pod is enough.

The leader election strategy is enabled by default and can be customized.

```yaml
leader_election:
  lease_duration: "35s"
  release_on_cancel: "true"
  renew_deadline: "30s"
  retry_period: "5s"
```
