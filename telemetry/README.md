# Telemetry

## Installing:
![its free](https://www.tibiabr.com/wp-content/uploads/2017/09/its-free-meme-600px.jpg)

No, really, all telemetry(but proper distributed tracing) will be available as soon as istio is installed and the proxies are set-up.


## Distributed tracing

In order to properly access distributed tracing, all applications should be able to propagate the headers. The headers that should be proppagated are described [here](https://istio.io/latest/docs/tasks/observability/distributed-tracing/overview/)