<!-- Warning: Do not manually edit this file. See notes on gluon + helm-docs at the end of this file for more information. -->
# anchore-enterprise

![Version: 3.21.1-bb.2](https://img.shields.io/badge/Version-3.21.1--bb.2-informational?style=flat-square) ![AppVersion: 5.25.0](https://img.shields.io/badge/AppVersion-5.25.0-informational?style=flat-square) ![Maintenance Track: bb_integrated](https://img.shields.io/badge/Maintenance_Track-bb_integrated-green?style=flat-square)

Anchore Enterprise is a complete container security workflow solution for professional teams. Easily integrating with CI/CD systems,
it allows developers to bolster security without compromising velocity and enables security teams to audit and verify compliance in real-time.
It is based on Anchore Engine, an open-source image inspection and scanning tool.

## Upstream References

- <https://anchore.com>
- <https://github.com/anchore/anchore-charts/tree/main/stable/enterprise>

## Upstream Release Notes

- [Find our upstream chart's CHANGELOG here](https://github.com/anchore/anchore-charts/tree/main)
- [and our upstream application release notes here](https://docs.anchore.com/current/docs/releasenotes/)

## Learn More

- [Application Overview](docs/overview.md)
- [Other Documentation](docs/)

## Pre-Requisites

- Kubernetes Cluster deployed
- Kubernetes config installed in `~/.kube/config`
- Helm installed

Kubernetes: `>= 1.23.x-x`

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

- Clone down the repository
- cd into directory

```bash
helm install anchore-enterprise chart/
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| domain | string | `"dev.bigbang.mil"` |  |
| routes.inbound.anchore-api.enabled | bool | `true` |  |
| routes.inbound.anchore-api.selector."app.kubernetes.io/component" | string | `"api"` |  |
| routes.inbound.anchore-api.gateways[0] | string | `"istio-gateway/public-ingressgateway"` |  |
| routes.inbound.anchore-api.hosts[0] | string | `"anchore-api.{{ .Values.domain }}"` |  |
| routes.inbound.anchore-api.http[0].match[0].uri.prefix | string | `"/metrics"` |  |
| routes.inbound.anchore-api.http[0].route[0].destination.host | string | `"anchore-enterprise-anchore-enterprise-api.anchore.svc.cluster.local"` |  |
| routes.inbound.anchore-api.http[0].route[0].destination.port.number | int | `8228` |  |
| routes.inbound.anchore-api.http[0].fault.abort.percentage.value | int | `100` |  |
| routes.inbound.anchore-api.http[0].fault.abort.httpStatus | int | `403` |  |
| routes.inbound.anchore-api.http[1].match[0].uri.prefix | string | `"/"` |  |
| routes.inbound.anchore-api.http[1].route[0].destination.host | string | `"anchore-enterprise-anchore-enterprise-api.anchore.svc.cluster.local"` |  |
| routes.inbound.anchore-api.http[1].route[0].destination.port.number | int | `8228` |  |
| routes.inbound.anchore-ui.enabled | bool | `true` |  |
| routes.inbound.anchore-ui.selector."app.kubernetes.io/component" | string | `"ui"` |  |
| routes.inbound.anchore-ui.gateways[0] | string | `"istio-gateway/public-ingressgateway"` |  |
| routes.inbound.anchore-ui.hosts[0] | string | `"anchore.{{ .Values.domain }}"` |  |
| routes.inbound.anchore-ui.service | string | `"anchore-enterprise-anchore-enterprise-ui.anchore.svc.cluster.local"` |  |
| routes.inbound.anchore-ui.port | int | `3000` |  |
| routes.outbound.anchore-data-service.enabled | bool | `true` |  |
| routes.outbound.anchore-data-service.hosts[0] | string | `"data.anchore-enterprise.com"` |  |
| istio.enabled | bool | `false` |  |
| istio.sidecar.enabled | bool | `false` |  |
| istio.sidecar.outboundTrafficPolicyMode | string | `"REGISTRY_ONLY"` |  |
| istio.serviceEntries.custom | list | `[]` |  |
| istio.authorizationPolicies.enabled | bool | `false` |  |
| istio.authorizationPolicies.custom | list | `[]` |  |
| istio.mtls.mode | string | `"STRICT"` |  |
| networkPolicies.enabled | bool | `false` |  |
| networkPolicies.ingress.to.catalog:8082.podSelector.matchLabels."app.kubernetes.io/component" | string | `"catalog"` |  |
| networkPolicies.ingress.to.catalog:8082.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.simplequeue:8083.podSelector.matchLabels."app.kubernetes.io/component" | string | `"simplequeue"` |  |
| networkPolicies.ingress.to.simplequeue:8083.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.analyzer:8084.podSelector.matchLabels."app.kubernetes.io/component" | string | `"analyzer"` |  |
| networkPolicies.ingress.to.analyzer:8084.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.policy:8087.podSelector.matchLabels."app.kubernetes.io/component" | string | `"policyengine"` |  |
| networkPolicies.ingress.to.policy:8087.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.api:8228.podSelector.matchLabels."app.kubernetes.io/component" | string | `"api"` |  |
| networkPolicies.ingress.to.api:8228.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.reports:8558.podSelector.matchLabels."app.kubernetes.io/component" | string | `"reports"` |  |
| networkPolicies.ingress.to.reports:8558.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.notifications:8668.podSelector.matchLabels."app.kubernetes.io/component" | string | `"notifications"` |  |
| networkPolicies.ingress.to.notifications:8668.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.ingress.to.ui-redis:9121.from.k8s.monitoring-monitoring-kube-prometheus@monitoring/prometheus | bool | `false` |  |
| networkPolicies.egress.definitions.anchore-data-service.to[0].ipBlock.cidr | string | `"0.0.0.0/0"` |  |
| networkPolicies.egress.definitions.anchore-data-service.ports[0].port | int | `443` |  |
| networkPolicies.egress.definitions.anchore-data-service.ports[0].protocol | string | `"TCP"` |  |
| networkPolicies.egress.definitions.ldap-subnets.to[0].ipBlock.cidr | string | `"192.168.0.0/16"` |  |
| networkPolicies.egress.definitions.ldap-subnets.to[1].ipBlock.cidr | string | `"172.16.0.0/12"` |  |
| networkPolicies.egress.definitions.ldap-subnets.to[2].ipBlock.cidr | string | `"10.0.0.0/8"` |  |
| networkPolicies.egress.definitions.ldap-subnets.ports[0].port | int | `636` |  |
| networkPolicies.egress.definitions.ldap-subnets.ports[0].protocol | string | `"TCP"` |  |
| networkPolicies.egress.definitions.notification-services.to[0].ipBlock.cidr | string | `"0.0.0.0/0"` |  |
| networkPolicies.egress.definitions.redis-subnets.to[0].ipBlock.cidr | string | `"192.168.0.0/16"` |  |
| networkPolicies.egress.definitions.redis-subnets.to[1].ipBlock.cidr | string | `"172.16.0.0/12"` |  |
| networkPolicies.egress.definitions.redis-subnets.to[2].ipBlock.cidr | string | `"10.0.0.0/8"` |  |
| networkPolicies.egress.definitions.redis-subnets.ports[0].port | int | `6379` |  |
| networkPolicies.egress.definitions.redis-subnets.ports[0].protocol | string | `"TCP"` |  |
| networkPolicies.egress.definitions.registry-subnets.to[0].ipBlock.cidr | string | `"0.0.0.0/0"` |  |
| networkPolicies.egress.from.*.to.k8s.tempo/tempo:9411 | bool | `false` |  |
| networkPolicies.egress.from.analyzer.podSelector.matchLabels."app.kubernetes.io/component" | string | `"analyzer"` |  |
| networkPolicies.egress.from.analyzer.to.definition.registry-subnets | bool | `true` |  |
| networkPolicies.egress.from.api.podSelector.matchLabels."app.kubernetes.io/component" | string | `"api"` |  |
| networkPolicies.egress.from.api.to.definition.redis-subnets | bool | `false` |  |
| networkPolicies.egress.from.api.to.definition.notification-services | bool | `true` |  |
| networkPolicies.egress.from.catalog.podSelector.matchLabels."app.kubernetes.io/component" | string | `"catalog"` |  |
| networkPolicies.egress.from.catalog.to.definition.registry-subnets | bool | `true` |  |
| networkPolicies.egress.from.datasyncer.podSelector.matchLabels."app.kubernetes.io/component" | string | `"datasyncer"` |  |
| networkPolicies.egress.from.datasyncer.to.definition.anchore-data-service | bool | `true` |  |
| networkPolicies.egress.from.notifications.podSelector.matchLabels."app.kubernetes.io/component" | string | `"notifications"` |  |
| networkPolicies.egress.from.notifications.to.definition.notification-services | bool | `true` |  |
| networkPolicies.egress.from.ui.podSelector.matchLabels."app.kubernetes.io/component" | string | `"ui"` |  |
| networkPolicies.egress.from.ui.to.definition.ldap-subnets | bool | `true` |  |
| networkPolicies.egress.from.ui.to.definition.redis-subnets | bool | `false` |  |
| networkPolicies.additionalPolicies | list | `[]` |  |
| sso.enabled | bool | `false` |  |
| sso.name | string | `"keycloak"` |  |
| sso.acsHttpsPort | int | `-1` |  |
| sso.spEntityId | string | `"platform1_a8604cc9-f5e9-4656-802d-d05624370245_bb8-anchore"` |  |
| sso.acsUrl | string | `"https://anchore.bigbang.dev/service/sso/auth/keycloak"` |  |
| sso.defaultAccount | string | `"user"` |  |
| sso.defaultRole | string | `"read-write"` |  |
| sso.roleAttribute | string | `""` |  |
| sso.requireSignedAssertions | bool | `false` |  |
| sso.requireSignedResponse | bool | `true` |  |
| sso.idpMetadataUrl | string | `"https://login.dso.mil/auth/realms/baby-yoda/protocol/saml/descriptor"` |  |
| sso.host | string | `"login.dso.mil"` |  |
| sso.realm | string | `"baby-yoda"` |  |
| sso.resources.limits.cpu | string | `"100m"` |  |
| sso.resources.limits.memory | string | `"256Mi"` |  |
| sso.resources.requests.cpu | string | `"100m"` |  |
| sso.resources.requests.memory | string | `"256Mi"` |  |
| sso.containerSecurityContext.runAsUser | int | `1001` |  |
| sso.containerSecurityContext.runAsGroup | int | `1001` |  |
| sso.containerSecurityContext.capabilities.drop[0] | string | `"ALL"` |  |
| monitoring.enabled | bool | `false` |  |
| monitoring.namespace | string | `"monitoring"` |  |
| monitoring.serviceMonitor.scheme | string | `""` |  |
| monitoring.serviceMonitor.tlsConfig | object | `{}` |  |
| bbtests.enabled | bool | `false` |  |
| bbtests.scripts.image | string | `"registry1.dso.mil/ironbank/anchore/cli/cli:0.9.4"` |  |
| bbtests.scripts.envs.ANCHORE_CLI_URL | string | `"http://{{ include \"enterprise.api.fullname\" . }}:{{ .Values.upstream.api.service.port }}/v2"` |  |
| bbtests.scripts.envs.ANCHORE_CLI_USER | string | `"admin"` |  |
| bbtests.scripts.envs.ANCHORE_SCAN_IMAGE | string | `"quay.io/prometheus/node-exporter:latest"` |  |
| bbtests.scripts.secretEnvs[0].name | string | `"ANCHORE_CLI_PASS"` |  |
| bbtests.scripts.secretEnvs[0].valueFrom.secretKeyRef.name | string | `"{{ include \"enterprise.fullname\" . }}"` |  |
| bbtests.scripts.secretEnvs[0].valueFrom.secretKeyRef.key | string | `"ANCHORE_ADMIN_PASSWORD"` |  |
| bbtests.cypress.resources.requests.cpu | string | `"2"` |  |
| bbtests.cypress.resources.requests.memory | string | `"4Gi"` |  |
| bbtests.cypress.resources.limits.cpu | string | `"2"` |  |
| bbtests.cypress.resources.limits.memory | string | `"4Gi"` |  |
| bbtests.cypress.artifacts | bool | `true` |  |
| bbtests.cypress.envs.cypress_url | string | `"http://{{ include \"enterprise.ui.fullname\" . }}:{{ .Values.upstream.ui.service.port }}"` |  |
| bbtests.cypress.envs.cypress_user | string | `"admin"` |  |
| bbtests.cypress.envs.cypress_registry | string | `"docker.io"` |  |
| bbtests.cypress.envs.cypress_repository | string | `"anchore/grype"` |  |
| bbtests.cypress.envs.cypress_tag | string | `"latest"` |  |
| bbtests.cypress.secretEnvs[0].name | string | `"cypress_password"` |  |
| bbtests.cypress.secretEnvs[0].valueFrom.secretKeyRef.name | string | `"{{ include \"enterprise.fullname\" . }}"` |  |
| bbtests.cypress.secretEnvs[0].valueFrom.secretKeyRef.key | string | `"ANCHORE_ADMIN_PASSWORD"` |  |
| global.fullnameOverride | string | `""` |  |
| global.nameOverride | string | `"anchore-enterprise"` |  |
| upstream | object | Upstream chart values | Values to pass to [the upstream Anchore Enterprise chart](https://github.com/anchore/anchore-charts/blob/main/stable/enterprise/values.yaml) |
| ui-redis.enabled | bool | `true` |  |
| ui-redis.istio.enabled | string | `"{{ .Values.istio.enabled }}"` |  |
| ui-redis.externalEndpoint | string | `""` |  |
| ui-redis.upstream.nameOverride | string | `"ui-redis"` |  |
| ui-redis.upstream.fullnameOverride | string | `"anchore-enterprise-ui-redis"` |  |
| ui-redis.upstream.auth.password | string | `"anchore-redis,123"` |  |
| ui-redis.upstream.architecture | string | `"standalone"` |  |
| ui-redis.upstream.master.persistence.enabled | bool | `false` |  |
| ui-redis.upstream.commonConfiguration | string | `"maxmemory 200mb\nsave \"\""` |  |
| ui-redis.cleanUpgrade.enabled | bool | `false` |  |
| ui-redis.cleanUpgrade.redisLabel | string | `"app.kubernetes.io/name: ui-redis"` |  |
| postgresql.enabled | bool | `true` |  |
| postgresql.image.registry | string | `"registry1.dso.mil"` |  |
| postgresql.image.repository | string | `"ironbank/opensource/postgres/postgresql"` |  |
| postgresql.image.tag | string | `"18.3"` |  |
| postgresql.global.security.allowInsecureImages | bool | `true` |  |
| postgresql.global.postgresql.auth.username | string | `"anchore"` | PostgreSQL User to create |
| postgresql.global.postgresql.auth.password | string | `"anchore-postgres,123"` | PostgreSQL Password for the new user |
| postgresql.global.postgresql.auth.database | string | `"anchore"` | PostgreSQL Database to create |
| postgresql.primary.networkPolicy.enabled | bool | `false` |  |
| postgresql.primary.persistence.mountPath | string | `"/var/lib/postgresql"` |  |
| postgresql.primary.extraVolumes[0].name | string | `"run-postgresql"` |  |
| postgresql.primary.extraVolumes[0].emptyDir | object | `{}` |  |
| postgresql.primary.extraVolumeMounts[0].name | string | `"run-postgresql"` |  |
| postgresql.primary.extraVolumeMounts[0].mountPath | string | `"/run/postgresql"` |  |
| postgresql.primary.resources.limits.cpu | string | `"1000m"` |  |
| postgresql.primary.resources.limits.memory | string | `"4096Mi"` |  |
| postgresql.primary.resources.requests.cpu | string | `"1000m"` |  |
| postgresql.primary.resources.requests.memory | string | `"4096Mi"` |  |
| postgresql.metrics.resources.limits.cpu | string | `"200m"` |  |
| postgresql.metrics.resources.limits.memory | string | `"256Mi"` |  |
| postgresql.metrics.resources.requests.cpu | string | `"200m"` |  |
| postgresql.metrics.resources.requests.memory | string | `"256Mi"` |  |
| postgresql.postgresqlDataDir | string | `"/var/lib/postgresql/pgdata/data"` |  |
| postgresql.volumePermissions.enabled | bool | `false` |  |
| postgresqlSuperUser.postgresUsername | string | `""` |  |
| postgresqlSuperUser.postgresPassword | string | `""` |  |
| postgresqlSuperUser.existingSecret | string | `nil` |  |
| ensureDbJobs.resources.limits.cpu | int | `2` |  |
| ensureDbJobs.resources.limits.memory | string | `"2G"` |  |
| ensureDbJobs.resources.requests.cpu | int | `2` |  |
| ensureDbJobs.resources.requests.memory | string | `"2G"` |  |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.

---

_This file is programatically generated using `helm-docs` and some BigBang-specific templates. The `gluon` repository has [instructions for regenerating package READMEs](https://repo1.dso.mil/big-bang/product/packages/gluon/-/blob/master/docs/bb-package-readme.md)._

