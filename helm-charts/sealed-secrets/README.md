# Sealed Secrets

This chart contains the resources to use [sealed-secrets](https://github.com/bitnami-labs/sealed-secrets).

## Prerequisites

* Kubernetes >= 1.9

## Configuration

| Parameter                     | Description                                                                | Default                                     |
|------------------------------:|:---------------------------------------------------------------------------|:--------------------------------------------|
| **controller.create**         | `true` if Sealed Secrets controller resources should be created            | `true`                                      |
| **rbac.create**               | `true` if rbac resources should be created                                 | `true`                                      |
| **rbac.pspEnabled**           | `true` if psp resources should be created                                  | `false`                                     |
| **serviceAccount.create**     | Whether to create a service account or not                                 | `true`                                      |
| **serviceAccount.name**       | The name of the service account to create or use                           | `"sealed-secrets-controller"`               |
| **secretName**                | The name of the TLS secret containing the key used to encrypt secrets      | `"sealed-secrets-key"`                      |
| **image.pullPolicy**          | The image pull policy for the deployment                                   | `IfNotPresent`                              |
| **image.repository**          | The repository to get the controller image from                            | `quay.io/bitnami/sealed-secrets-controller@sha256:8e9a37bb2e1a6f3a8bee949e3af0e9dab0d7dca618f1a63048dc541b5d554985` |
| **resources**                 | CPU/Memory resource requests/limits                                        | `{}`                                        |
| **crd.create**                | `true` if crd resources should be created                                  | `true`                                      |
| **crd.keep**                  | `true` if the sealed secret CRD should be kept when the chart is deleted   | `true`                                      |
| **networkPolicy**             | Whether to create a network policy that allows access to the service       | `false`                                     |
| **securityContext.runAsUser** | Defines under which user the operator Pod and its containers/processes run | `1001`                                      |
| **securityContext.fsGroup**   | Defines fsGroup for the operator Pod and its containers/processes run      | `65534`                                     |
| **commandArgs**               | Set optional command line arguments passed to the controller process       | `[]`                                        |
| **ingress.enabled**           | Enables Ingress                                                            | `false`                                     |
| **ingress.annotations**       | Ingress annotations                                                        | `{}`                                        |
| **ingress.path**              | Ingress path                                                               | `/v1/cert.pem`                              |
| **ingress.hosts**             | Ingress accepted hostnames                                                 | `["chart-example.local"]`                   |
| **ingress.tls**               | Ingress TLS configuration                                                  | `[]`                                        |
| **podAnnotations**            | Annotations to annotate pods with.                                         | `{}`                                        |
| **podLabels**                 | Labels to be added to pods                                                 | `{}`                                        |
| **priorityClassName**         | Optional class to specify priority for pods                                | `""`                                        |


- In the case that **serviceAccount.create** is `false` and **rbac.create** is `true` it is expected for a service account with the name **serviceAccount.name** to exist _in the same namespace as this chart_ before installation.
- If **serviceAccount.create** is `true` there cannot be an existing service account with the name **serviceAccount.name**.
- If a secret with name **secretName** does not exist _in the same namespace as this chart_, then on install one will be created. If a secret already exists with this name the keys inside will be used.
- OpenShift: unset the runAsUser and fsGroup like this:
```
  securityContext:
    runAsUser:
    fsGroup:
```
