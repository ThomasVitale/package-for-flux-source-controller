# Configuring Observability

Monitor and observe the operation of Flux Source Controller using logs and metrics.

## Logs

The log verbosity and encoding for the Flux Source Controller container can be configured.

```yaml
logging:
  level: info
  encoding: json
```

## Metrics

The Flux Source Controller produces Prometheus metrics by default. This package comes pre-configured with the necessary annotations to let Prometheus scrape metrics automatically from the Flux Source Controller.
