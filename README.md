# Custom loki-stack Helm Chart
대규모 시스템 환경이 아닌 중/소규모의 시스템 환경에선 `loki-stack` 으로 로깅 모니터링을 구축할 수 있습니다.
사용법 또한 비교적 간편하며, 기존에 사용하고 있는 그라파나와도 쉽게 연동이 가능하므로 리소스 모니터링과 로그 모니터링 채널을 한개로 운영 할 수 있습니다.

## 설치 및 테스트
해당 레포지트를 클론합니다.
```
helm upgrade -i loki-stack . -n loki
helm ls
```
로그 메세지 출력 확인용 Pod
```
kubectl apply -f fake-logging.yaml
```
그라파나와 연동이되어있다면 로그 메시지를 바로 확인 가능합니다.

공식 헬름차트 저정소: https://github.com/grafana/helm-charts?tab=readme-ov-file


# Loki-Stack Helm Chart

This `loki-stack` Helm chart is a community maintained chart.

## Prerequisites

Make sure you have Helm [installed](https://helm.sh/docs/using_helm/#installing-helm).

## Get Repo Info

```console
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

_See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation._

## Deploy Loki and Promtail to your cluster

### Deploy with default config

```bash
helm upgrade --install loki grafana/loki-stack
```

### Deploy in a custom namespace

```bash
helm upgrade --install loki --namespace=loki-stack grafana/loki-stack
```

### Deploy with custom config

```bash
helm upgrade --install loki grafana/loki-stack --set "key1=val1,key2=val2,..."
```

## Deploy Loki and Fluent Bit to your cluster

```bash
helm upgrade --install loki grafana/loki-stack \
    --set fluent-bit.enabled=true,promtail.enabled=false
```

## Deploy Grafana to your cluster

The chart loki-stack contains a pre-configured Grafana, simply use `--set grafana.enabled=true`

To get the admin password for the Grafana pod, run the following command:

```bash
kubectl get secret --namespace <YOUR-NAMESPACE> loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

To access the Grafana UI, run the following command:

```bash
kubectl port-forward --namespace <YOUR-NAMESPACE> service/loki-grafana 3000:80
```

Navigate to <http://localhost:3000> and login with `admin` and the password output above.
Then follow the [instructions for adding the loki datasource](https://grafana.com/docs/grafana/latest/datasources/loki/), using the URL `http://loki:3100/`.

## Upgrade
### Version >= 2.8.0
Provide support configurable datasource urls [#1374](https://github.com/grafana/helm-charts/pull/1374)

### Version >= 2.7.0
Update promtail dependency to ^6.2.3 [#1692](https://github.com/grafana/helm-charts/pull/1692)

### Version >=2.6.0
Bumped grafana 8.1.6->8.3.4 [#1013](https://github.com/grafana/helm-charts/pull/1013)