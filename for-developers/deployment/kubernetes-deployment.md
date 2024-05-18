# ⭐ Kubernetes Deployment

We've released a Helm chart to deploy UCHAIN.INFO stack ([backend](https://github.com/UdotCASH/uchain-info/), [frontend](https://github.com/UdotCASH/uchain-info-frontend) and [stats](https://github.com/UdotCASH/uchain-info/-rs/tree/main/stats)) to a kubernetes cluster.&#x20;

Baseline instructions are available at [https://github.com/UdotCASH/uchain-info/helm-charts/tree/main/charts/uchaininfo-stack](https://github.com/UdotCASH/uchain-info/helm-charts/tree/main/charts/uchaininfo-stack).

### Troubleshooting

1. Check your values are correct, for example if you don't have Prometheus installed [disable it here](https://github.com/UdotCASH/uchain-info/helm-charts/blob/7fe62850a9d0ab220041598722569ab13fa54540/charts/uchaininfo-stack/values.yaml#L33)
2. Ingress is disabled by default as is common practice, you will likely want to enable ingress and configure hostnames accordingly.
3. Check your .yaml files for any unneeded whitespaces, this can impact functionality.
4. Make sure env variables are declared correctly ie DATABASE\_URL: “X”
5. Frontend values should exist in their own root section, at the same level as uchaininfo values (and not nested within the uchaininfo root).

For example:

````yaml
```yaml
uchaininfo:
  env:
    DATABASE_URL: "X"
    ETHEREUM_JSONRPC_VARIANT: geth
    ETHEREUM_JSONRPC_HTTP_URL: "X"
    ETHEREUM_JSONRPC_TRACE_URL: "X"
  ingress:
    enabled: true
    hostname: host-name.com
frontend:
  image:
    tag: latest
  ingress:
    enabled: true
```
````
