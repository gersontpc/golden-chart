# Helm Chart App Test
Chart Helm que entrega um Deployment NGINX com Service e HPA opcional.

## Pré‑requisitos
- Kubernetes e `kubectl` configurados
- Helm 3 instalado

## Uso rápido
```bash
# validar templates
helm lint .
helm template . --debug

# instalar/atualizar
helm upgrade --install my-app . -n demo --create-namespace

# rodar hook de teste (faz curl no service)
helm test my-app -n demo

# remover
helm uninstall my-app -n demo
```

## Configuração (values.yaml)
Principais valores que você pode ajustar:

| Valor | Descrição | Padrão |
| --- | --- | --- |
| `replicaCount` | Réplicas quando HPA desabilitado | `2` |
| `image.repository` | Imagem do container | `nginx` |
| `image.tag` | Tag da imagem | `"1.27"` |
| `image.pullPolicy` | Política de pull | `IfNotPresent` |
| `imagePullSecrets` | Lista de secrets de pull | `[]` |
| `nameOverride` / `fullnameOverride` | Sobrescreve nomes gerados | `""` |
| `serviceAccount.create` | Cria ServiceAccount dedicado | `true` |
| `serviceAccount.annotations` | Anotações no ServiceAccount | `{}` |
| `serviceAccount.name` | Nome do SA (deixa vazio para gerar) | `""` |
| `podAnnotations` | Anotações extras no Pod | `{}` |
| `podLabels` | Labels extras no Pod | `{}` |
| `podSecurityContext` | Contexto de segurança do pod | `{}` |
| `securityContext` | Contexto de segurança do container | `{}` |
| `service.type` | Tipo do Service | `ClusterIP` |
| `service.port` | Porta exposta pelo Service | `80` |
| `service.name` | Nome da porta/targetPort | `"http"` |
| `service.protocol` | Protocolo da porta | `"TCP"` |
| `resources` | Requests/limits de CPU/Mem | `{}` |
| `nodeSelector` | Node selector | `{}` |
| `tolerations` | Tolerations | `[]` |
| `affinity` | Affinity | `{}` |
| `autoscaling.enabled` | Ativa HPA | `false` |
| `autoscaling.minReplicas` | Réplicas mínimas do HPA | `2` |
| `autoscaling.maxReplicas` | Réplicas máximas do HPA | `10` |
| `autoscaling.targetCPUUtilizationPercentage` | Target de CPU (%) | `80` |
| `autoscaling.targetMemoryUtilizationPercentage` | Target de memória (%) | `null` |

## Arquitetura gerada
- Deployment com container NGINX exposto na porta 80
- Service apontando para o selector do Deployment
- HPA (se `autoscaling.enabled=true`) usando CPU e opcionalmente memória
- Hook de teste (`helm test`) que faz um curl no Service
