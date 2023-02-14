# Health Checks with Liveness and Readiness Probes

To check container health on kubernetes have to option

1. Readiness is to check container healthy is ready or not to receive traffic (before started)

```yaml
readinessProbe:
 tcpSocket:
   port: 80
 initialDelaySeconds: 10
 periodSeconds: 5
```

2. Liveness is to restart pod if container crash or have issue, only check after container started

Type to check health status on container pod

- exec probes

```yaml
livenessProbe:
  exec:
    command:
      - /bin/my-script.sh
  initialDelaySeconds: 60
  periodSeconds: 5
```

- tcp probes

```yaml
livenessProbe:
 tcpSocket:
  port: 80
 initialDelaySeconds: 5
 periodSeconds: 15
```

- http probes

```yaml
livenessProbe:
 httpGet:
  path: /healthz
  port: 8080
  httpHeaders:
  - name: Custom-Header
    value: Awesome
  initialDelaySeconds: 3
  periodSeconds: 3
```
