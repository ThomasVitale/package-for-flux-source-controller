# Using a Corporate Proxy

When running Flux Source Controller behind a corporate proxy, you can configure the controller to proxy communications with external services.

```yaml
proxy:
  http_proxy: "proxy.kadras.io"
  https_proxy: "proxy.kadras.io"
  no_proxy: ".svc, .cluster.local"
```
