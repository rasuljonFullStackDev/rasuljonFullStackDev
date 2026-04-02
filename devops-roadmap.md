# DevOps Engineer bo'lish uchun Roadmap

## Bosqich 1: Asoslar (0-3 oy)

### Linux & Terminal
- Linux asoslari (Ubuntu/CentOS)
- Terminal buyruqlari: `ls`, `cd`, `grep`, `awk`, `sed`, `chmod`, `chown`
- Shell scripting (Bash)
- Fayl tizimi, jarayonlar, xotira boshqaruvi
- SSH, SCP, SFTP

### Tarmoq asoslari
- TCP/IP, DNS, HTTP/HTTPS, SSL/TLS
- IP manzillar, Subnet, Port
- Firewall, Load Balancer tushunchalari
- `ping`, `netstat`, `curl`, `wget`, `nmap`

### Dasturlash (kamida bittasi)
- Python (eng tavsiya etilgan DevOps uchun)
- Go yoki Bash scripting
- YAML va JSON formatlari

---

## Bosqich 2: Version Control & CI/CD (3-5 oy)

### Git
- Git asoslari: `clone`, `commit`, `push`, `pull`, `merge`, `rebase`
- Branching strategiyalar (GitFlow, Trunk-based)
- GitHub / GitLab / Bitbucket

### CI/CD (Continuous Integration / Deployment)
- Jenkins
- GitHub Actions
- GitLab CI/CD
- CircleCI / Travis CI
- Pipeline yaratish va avtomatlashtirish

---

## Bosqich 3: Konteynerizatsiya (5-7 oy)

### Docker
- Docker nima va nima uchun kerak
- Dockerfile yozish
- Docker Compose
- Image, Container, Volume, Network
- Docker Hub / Registry

### Kubernetes (K8s)
- Kubernetes arxitekturasi
- Pod, Deployment, Service, Ingress
- ConfigMap, Secret
- Helm Charts
- kubectl buyruqlari
- Minikube / Kind (lokal muhit)

---

## Bosqich 4: Cloud Platformalar (7-10 oy)

### Kamida bittasini o'rganing:

#### AWS (eng keng tarqalgan)
- EC2, S3, RDS, VPC
- IAM (Identity & Access Management)
- EKS, ECS, Lambda
- CloudWatch, CloudTrail
- **Sertifikat:** AWS Solutions Architect Associate

#### Google Cloud (GCP)
- Compute Engine, GKE, Cloud Storage
- **Sertifikat:** Google Associate Cloud Engineer

#### Azure
- Virtual Machines, AKS, Blob Storage
- **Sertifikat:** AZ-900, AZ-104

---

## Bosqich 5: Infrastructure as Code (9-12 oy)

### Terraform
- Resurslar yaratish (EC2, VPC, S3...)
- State management
- Modules va Workspaces
- Terraform Cloud

### Ansible
- Playbook yozish
- Inventory boshqaruvi
- Role va Template
- Server konfiguratsiyasini avtomatlashtirish

---

## Bosqich 6: Monitoring & Logging (10-13 oy)

### Monitoring
- **Prometheus** - metrikalar yig'ish
- **Grafana** - dashboard va vizualizatsiya
- **Alertmanager** - ogohlantirish tizimi
- Datadog / New Relic

### Logging
- **ELK Stack**: Elasticsearch + Logstash + Kibana
- **Loki** + Promtail
- Fluentd / Fluentbit

### Tracing
- Jaeger
- Zipkin
- OpenTelemetry

---

## Bosqich 7: Xavfsizlik - DevSecOps (12-15 oy)

- OWASP top 10
- Container xavfsizligi (Trivy, Snyk)
- Secrets boshqaruvi (HashiCorp Vault, AWS Secrets Manager)
- SAST / DAST skanerlash
- Network Policy Kubernetes'da
- SSL/TLS sertifikatlar (Let's Encrypt, cert-manager)

---

## Bosqich 8: Ilg'or mavzular (15+ oy)

### Service Mesh
- Istio
- Linkerd

### GitOps
- ArgoCD
- Flux

### Serverless
- AWS Lambda
- Google Cloud Functions

### Microservices arxitekturasi
- API Gateway
- Message Queue: RabbitMQ, Kafka

---

## Foydali Resurslar

### Kitoblar
- "The Phoenix Project" - Gene Kim
- "The DevOps Handbook" - Gene Kim
- "Site Reliability Engineering" - Google (bepul online)
- "Kubernetes in Action" - Marko Luksa

### Online Kurslar
- **Linux:** Linux Foundation kurslar
- **Docker/K8s:** KodeKloud, TechWorld with Nana (YouTube)
- **AWS:** Adrian Cantrill, Stephane Maarek (Udemy)
- **Terraform:** HashiCorp Learn (bepul)

### YouTube Kanallar
- TechWorld with Nana
- NetworkChuck
- Fireship
- That DevOps Guy

### Amaliyot Saytlari
- **KodeKloud** - interaktiv DevOps laboratoriyalar
- **Play with Docker** - bepul Docker muhiti
- **Killercoda** - Kubernetes amaliyoti
- **ACloudGuru** - cloud amaliyoti

---

## Sertifikatlar (Muhimlik bo'yicha)

| Sertifikat | Daraja | Narx |
|------------|--------|------|
| AWS Solutions Architect Associate | O'rta | ~$300 |
| CKA (Certified Kubernetes Admin) | O'rta | ~$395 |
| Terraform Associate | Boshlang'ich | ~$70 |
| AWS DevOps Professional | Yuqori | ~$300 |
| GCP Professional DevOps Engineer | Yuqori | ~$200 |

---

## Maosh Kutilmalari (2025-2026)

| Daraja | Tajriba | O'rtacha Maosh (USD/yil) |
|--------|---------|--------------------------|
| Junior DevOps | 0-2 yil | $50,000 - $80,000 |
| Mid DevOps | 2-5 yil | $80,000 - $120,000 |
| Senior DevOps | 5+ yil | $120,000 - $180,000 |
| DevOps Architect | 8+ yil | $150,000 - $250,000 |

---

## Birinchi Loyiha Uchun G'oyalar

1. **Portfolio server** - VPS'da Nginx + Docker + CI/CD pipeline
2. **Kubernetes cluster** - 3 node cluster + monitoring
3. **Terraform + AWS** - butun infratuzilmani kod bilan yaratish
4. **GitLab CI/CD** - to'liq pipeline: test → build → deploy
5. **Monitoring stack** - Prometheus + Grafana + alertlar

---

## Eslatma

> DevOps - bu faqat texnologiya emas, balki madaniyat va yondashuv.
> Amaliyot qiling, real loyihalarda ishtirok eting, open source'ga hissa qo'shing.
> Har kuni kamida 1-2 soat o'rganing va amaliyot qiling.

**Omad!**
