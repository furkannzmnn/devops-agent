# AI Destekli DevOps İçin YAML Dosyası Kılavuzu

Bu kılavuz, AI destekli DevOps sürecinizi n8n ile otomatize ederken kullanıcıların nasıl bir YAML dosyası oluşturması gerektiğini açıklamaktadır.  
YAML dosyası, hangi deployment yöntemi kullanılacağını ve servisin temel gereksinimlerini belirler.

---

## 🛠 YAML Dosyası Genel Yapısı

Aşağıda, kullanıcıların oluşturması gereken YAML dosyasının temel yapısı bulunmaktadır:

```yaml
type: aws-ecs  # Desteklenen seçenekler: aws-ecs, kubernetes, docker-compose
infrastructure:
  provider: aws  # Desteklenen seçenekler: aws, gcp, azure, on-prem
  deploymentTool: terraform  # Varsayılan: terraform, alternatif: cloudformation (AWS), helm (K8s)
  region: us-east-1  # Deployment yapılacak bölge
  credentials:
    accessKey: "<AWS_ACCESS_KEY>"
    secretKey: "<AWS_SECRET_KEY>"

services:
  spring-app:
    name: "spring-container"
    image: "your-image-name"
    port: 8080
    envVars:
      - key: "SPRING_PROFILES_ACTIVE"
        value: "prod"
    replicas: 2  # Servis için çoğaltma sayısı
    resources:
      cpu: "500m"
      memory: "512Mi"
  redis:
    image: "redis:latest"
    port: 6379
  kafka:
    image: "confluentinc/cp-kafka:latest"
    port: 9092

network:
  vpc: "vpc-123456"
  subnets:
    - "subnet-1234"
    - "subnet-5678"
  securityGroup: "sg-123456"

monitoring:
  enabled: true
  tool: "prometheus"  # Varsayılan: prometheus, alternatif: cloudwatch (AWS), stackdriver (GCP)


### 4. Tablo Oluşturma
Markdown'da tablo oluşturmak için aşağıdaki formatı kullanabilirsiniz:

```markdown
## 📌 YAML Dosyasında Kullanıcıdan Beklenen Bilgiler

| Alan | Açıklama | Varsayılan Değer |
|------|----------|------------------|
| `type` | Deployment türü | `aws-ecs` |
| `infrastructure.provider` | Cloud sağlayıcısı | `aws` |
| `infrastructure.deploymentTool` | Deployment için kullanılacak araç | `terraform` |
| `infrastructure.region` | Bölge | `us-east-1` |
| `services` | Deployment yapılacak servisler | Zorunlu alan |
| `services.[servis_adı].image` | Docker imaj adı | Zorunlu alan |
| `services.[servis_adı].port` | Servisin dışarıya açtığı port | Zorunlu alan |
| `services.[servis_adı].envVars` | Ortam değişkenleri | Opsiyonel |
| `services.[servis_adı].replicas` | Çoğaltma sayısı | `1` |
| `services.[servis_adı].resources.cpu` | CPU limiti | `500m` |
| `services.[servis_adı].resources.memory` | Bellek limiti | `512Mi` |
| `network` | Ağ yapılandırması | Opsiyonel |
| `monitoring.enabled` | İzleme açık mı? | `true` |
| `monitoring.tool` | İzleme aracı | `prometheus` |


## 📌 Deployment Seçenekleri ve Desteklenen Araçlar

| Deployment Türü | Desteklenen Araçlar |
|-----------------|---------------------|
| AWS ECS | Terraform, CloudFormation |
| Kubernetes | Terraform, Helm |
| Docker Compose | Native Docker Compose |

**Not:** Kullanıcı AWS seçerse CloudFormation da opsiyonel olarak desteklenmelidir.

## 🛠 Kullanıcıdan Beklenen Girdi Örnekleri

### 1️⃣ AWS ECS için YAML Örneği (Terraform ile)

```yaml
type: aws-ecs
infrastructure:
  provider: aws
  deploymentTool: terraform
  region: eu-west-1
  credentials:
    accessKey: "<AWS_ACCESS_KEY>"
    secretKey: "<AWS_SECRET_KEY>"

services:
  my-app:
    name: "spring-app"
    image: "myrepo/myimage:latest"
    port: 8080
    envVars:
      - key: "DATABASE_URL"
        value: "jdbc:postgresql://db:5432/mydb"


type: kubernetes
infrastructure:
  provider: gcp
  deploymentTool: helm
  region: europe-west3

2️⃣ Kubernetes için YAML Örneği (Helm ile)

services:
  my-app:
    name: "spring-k8s"
    image: "gcr.io/my-project/myimage:latest"
    port: 8080


3️⃣ Docker Compose için YAML Örneği
type: docker-compose
infrastructure:
  provider: on-prem
  deploymentTool: docker-compose

services:
  my-app:
    name: "spring-compose"
    image: "myimage:latest"
    port: 8080
  db:
    image: "postgres:latest"
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret"