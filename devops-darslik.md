# DevOps — 0 dan To'liq Darslik (O'zbek tilida)

---

## Mundarija

1. [DevOps nima?](#1-devops-nima)
2. [Linux asoslari](#2-linux-asoslari)
3. [Bash Scripting](#3-bash-scripting)
4. [Tarmoq asoslari](#4-tarmoq-asoslari)
5. [Git va Version Control](#5-git-va-version-control)
6. [Docker](#6-docker)
7. [CI/CD](#7-cicd)
8. [Kubernetes](#8-kubernetes)
9. [Terraform](#9-terraform)
10. [Ansible](#10-ansible)
11. [Monitoring](#11-monitoring)
12. [AWS asoslari](#12-aws-asoslari)

---

# 1. DevOps nima?

## Ta'rif

**DevOps** = **Dev**elopment (Dasturlash) + **Op**eration**s** (Tizim boshqaruvi)

DevOps — bu dasturchilar va tizim administratorlari o'rtasidagi bo'shliqni yo'q qiluvchi yondashuv. Maqsad: dasturlarni tezroq, ishonchli va avtomatik tarzda yetkazib berish.

## Eski usul vs DevOps

```
ESKI USUL:
  Dasturchi → kod yozadi → IT ga beradi → IT server ga qo'yadi → muammo chiqadi → bir-birini ayblaydi

DEVOPS:
  Dasturchi + DevOps → kod yozadi → avtomatik test → avtomatik deploy → monitoring
```

## DevOps Engineer nima qiladi?

- Server va infratuzilmani boshqaradi
- CI/CD pipeline yaratadi (avtomatik deploy)
- Docker va Kubernetes bilan ishlaydi
- Monitoring va alertlar sozlaydi
- Cloud (AWS/GCP/Azure) ni boshqaradi
- Xavfsizlikni ta'minlaydi

## DevOps sikli

```
Plan → Code → Build → Test → Release → Deploy → Operate → Monitor → (qaytadan Plan)
```

---

# 2. Linux Asoslari

## Nima uchun Linux?

Server dunyosining **90%+** Linux ishlatadi. DevOps uchun Linux bilish — majburiy.

## Linux o'rnatish

**Variant 1:** VirtualBox orqali Ubuntu o'rnatish
**Variant 2:** WSL2 (Windows uchun)
**Variant 3:** Cloud server (AWS EC2 — bepul tier)

---

## 2.1 Terminal bilan ishlash

```bash
# Terminal ochish: Ctrl + Alt + T (Ubuntu)

# Hozirgi papkani ko'rish
pwd
# Natija: /home/username

# Papkalar ro'yxati
ls
ls -la          # batafsil ko'rsatish (yashirin fayllar bilan)
ls -lh          # fayl hajmini o'qish qulay formatda

# Papkaga kirish
cd /home
cd Desktop
cd ..           # bir yuqoriga
cd ~            # home papkaga
cd -            # oldingi papkaga
```

---

## 2.2 Fayl va Papka Boshqaruvi

```bash
# Papka yaratish
mkdir loyiha
mkdir -p loyiha/frontend/src    # ichma-ich papka

# Fayl yaratish
touch index.html
touch fayl1.txt fayl2.txt       # bir nechta fayl

# Fayl o'qish
cat fayl.txt                    # to'liq o'qish
less fayl.txt                   # sahifa-sahifa o'qish (q - chiqish)
head -10 fayl.txt               # dastlabki 10 qator
tail -10 fayl.txt               # oxirgi 10 qator
tail -f log.txt                 # real vaqtda kuzatish

# Nusxa ko'chirish
cp fayl.txt /tmp/
cp -r papka/ /tmp/              # papka nusxasi (r = recursive)

# Ko'chirish / Nomini o'zgartirish
mv fayl.txt yangi_nom.txt
mv fayl.txt /home/user/

# O'chirish
rm fayl.txt
rm -rf papka/                   # papkani butunlay o'chirish (EHTIYOT BO'LING!)

# Fayl qidirish
find / -name "nginx.conf"       # butun sistemada
find . -name "*.txt"            # hozirgi papkada
find /var/log -name "*.log" -mtime -7   # 7 kundan yangi loglar
```

---

## 2.3 Matn Bilan Ishlash

```bash
# grep — matn qidirish
grep "error" log.txt            # log.txtda "error" qidirish
grep -i "error" log.txt         # katta-kichik harf farqi yo'q
grep -r "TODO" ./src/           # papka ichida qidirish
grep -n "error" log.txt         # qator raqami bilan

# Foydalanish misoli
cat /var/log/syslog | grep "error"

# sort — saralash
sort fayl.txt
sort -r fayl.txt                # teskari tartibda

# wc — hisoblash
wc -l fayl.txt                  # qatorlar soni
wc -w fayl.txt                  # so'zlar soni

# cut — ustun ajratish
cut -d',' -f1 data.csv          # vergul bilan ajratilgan 1-ustun

# sed — matnni almashtirish
sed 's/eski/yangi/g' fayl.txt           # eski so'zni yangi bilan
sed -i 's/localhost/0.0.0.0/g' app.conf # faylni o'zini o'zgartirish

# awk — kengaytirilgan matn ishlash
awk '{print $1, $3}' fayl.txt   # 1 va 3 ustunni chiqarish
awk -F',' '{print $2}' data.csv # vergul separator, 2-ustun
```

---

## 2.4 Ruxsatlar (Permissions)

```bash
# Ruxsatlarni ko'rish
ls -la
# -rwxr-xr-- 1 user group 1234 Jan 1 fayl.sh
#  |||||||
#  |||||└─ Others: r-- = faqat o'qish
#  ||└──── Group:  r-x = o'qish + bajarish
#  └────── Owner:  rwx = hammasi

# Ruxsat berish
chmod +x skript.sh              # bajarish ruxsati
chmod 755 skript.sh             # rwxr-xr-x
chmod 644 fayl.txt              # rw-r--r--
chmod -R 755 papka/             # papka va ichidagi hammasi

# Ruxsat raqamlari:
# 7 = rwx (read + write + execute)
# 6 = rw- (read + write)
# 5 = r-x (read + execute)
# 4 = r-- (faqat read)

# Egasini o'zgartirish
chown user:group fayl.txt
chown -R www-data:www-data /var/www/
```

---

## 2.5 Jarayonlar (Processes)

```bash
# Jarayonlarni ko'rish
ps aux                          # barcha jarayonlar
ps aux | grep nginx             # nginx jarayonlari

top                             # real vaqtda jarayonlar (q - chiqish)
htop                            # ko'rgazmali top (o'rnatish kerak)

# Jarayonni to'xtatish
kill 1234                       # jarayon ID bilan
kill -9 1234                    # majburan to'xtatish
killall nginx                   # nom bilan

# Fon jarayonlar
./skript.sh &                   # fon rejimida ishga tushirish
jobs                            # fon jarayonlar ro'yxati
fg 1                            # fonga qaytarish
nohup ./skript.sh &             # terminaldan chiqqandan keyin ham ishlaydi
```

---

## 2.6 Disk va Xotira

```bash
# Disk holati
df -h                           # disk bo'shligi
du -sh papka/                   # papka hajmi
du -sh /var/log/*               # har bir log fayli hajmi

# Xotira holati
free -h                         # RAM holati

# Katta fayllarni topish
find / -size +100M 2>/dev/null  # 100MBdan katta fayllar
du -ah / | sort -rh | head -20  # eng katta 20 fayl
```

---

## 2.7 Tarmoq Buyruqlari

```bash
# IP manzilni ko'rish
ip addr
ifconfig                        # eski usul

# Ulanishni tekshirish
ping google.com
ping -c 4 google.com            # 4 ta paket yuborish

# Port tekshirish
netstat -tulpn                  # ochiq portlar
ss -tulpn                       # zamonaviy netstat

# HTTP so'rovlar
curl https://example.com
curl -I https://example.com     # faqat header
curl -X POST -d '{"key":"val"}' https://api.com/endpoint

# Fayl yuklab olish
wget https://example.com/file.zip
curl -O https://example.com/file.zip
```

---

## 2.8 Foydalanuvchilar

```bash
# Foydalanuvchi yaratish
sudo adduser ali
sudo passwd ali                 # parol o'rnatish

# Guruhga qo'shish
sudo usermod -aG sudo ali       # sudo huquqi berish
sudo usermod -aG docker ali     # docker guruhiga

# Foydalanuvchilar ro'yxati
cat /etc/passwd
id ali                          # user ma'lumotlari

# Sudo bilan ishlash
sudo apt update                 # root sifatida buyruq
sudo su                         # root ga o'tish
sudo su - ali                   # ali foydalanuvchiga o'tish
```

---

## 2.9 Paketlarni Boshqarish (Ubuntu/Debian)

```bash
sudo apt update                 # paketlar ro'yxatini yangilash
sudo apt upgrade                # barcha paketlarni yangilash
sudo apt install nginx          # o'rnatish
sudo apt remove nginx           # o'chirish
sudo apt purge nginx            # konfiglar bilan o'chirish
apt search nginx                # qidirish
apt show nginx                  # ma'lumot
```

---

## 2.10 Servislar (Systemd)

```bash
# Servisni boshqarish
sudo systemctl start nginx      # ishga tushirish
sudo systemctl stop nginx       # to'xtatish
sudo systemctl restart nginx    # qayta ishga tushirish
sudo systemctl reload nginx     # konfigni qayta yuklash

# Avtomatik ishga tushirish
sudo systemctl enable nginx     # tizim yuklanganda ishga tushirsin
sudo systemctl disable nginx    # o'chirish

# Holat tekshirish
sudo systemctl status nginx
sudo journalctl -u nginx        # nginx loglari
sudo journalctl -u nginx -f     # real vaqtda
sudo journalctl -u nginx --since "1 hour ago"
```

---

# 3. Bash Scripting

## 3.1 Birinchi Skript

```bash
#!/bin/bash
# Fayl: salom.sh

echo "Salom, Dunyo!"
echo "Bugun: $(date)"
echo "Men: $(whoami)"
```

```bash
# Bajarishga ruxsat berish va ishga tushirish
chmod +x salom.sh
./salom.sh
```

---

## 3.2 O'zgaruvchilar

```bash
#!/bin/bash

# O'zgaruvchi
ISM="Ali"
YOSH=25
PI=3.14

echo "Ismim: $ISM"
echo "Yoshim: ${YOSH}"         # {} bilan ham ishlatish mumkin

# Buyruq natijasini o'zgaruvchiga saqlash
SANA=$(date +%Y-%m-%d)
HOZIRGI_PAPKA=$(pwd)
FAYL_SONI=$(ls | wc -l)

echo "Sana: $SANA"
echo "Papka: $HOZIRGI_PAPKA"
echo "Fayllar: $FAYL_SONI"

# Muhit o'zgaruvchilari
echo "Home: $HOME"
echo "User: $USER"
echo "Path: $PATH"
```

---

## 3.3 Input (Kirish)

```bash
#!/bin/bash

# Foydalanuvchidan kiritish
echo -n "Ismingizni kiriting: "
read ISM
echo "Salom, $ISM!"

# Parol (ko'rinmaydigan)
read -sp "Parol: " PAROL
echo ""
echo "Parol kiritildi"

# Argumentlar
# ./skript.sh arg1 arg2 arg3
echo "Skript nomi: $0"
echo "1-argument: $1"
echo "2-argument: $2"
echo "Barcha argumentlar: $@"
echo "Argumentlar soni: $#"
```

---

## 3.4 Shartlar (If-Else)

```bash
#!/bin/bash

YOSH=20

# Oddiy shart
if [ $YOSH -ge 18 ]; then
    echo "Voyaga yetgan"
else
    echo "Voyaga yetmagan"
fi

# Ko'p shartlar
if [ $YOSH -lt 13 ]; then
    echo "Bola"
elif [ $YOSH -lt 18 ]; then
    echo "O'smir"
elif [ $YOSH -lt 65 ]; then
    echo "Katta"
else
    echo "Keksa"
fi

# Taqqoslash operatorlari:
# -eq  teng
# -ne  teng emas
# -gt  katta
# -lt  kichik
# -ge  katta yoki teng
# -le  kichik yoki teng

# Fayl tekshirish
FAYL="/etc/nginx/nginx.conf"
if [ -f "$FAYL" ]; then
    echo "Fayl mavjud"
fi

if [ -d "/var/log" ]; then
    echo "Papka mavjud"
fi

if [ ! -f "$FAYL" ]; then
    echo "Fayl yo'q!"
fi

# Matn taqqoslash
MUHIT="production"
if [ "$MUHIT" = "production" ]; then
    echo "Production muhiti!"
fi
```

---

## 3.5 Tsikllar

```bash
#!/bin/bash

# for tsikli
for i in 1 2 3 4 5; do
    echo "Raqam: $i"
done

# Diapazon
for i in {1..10}; do
    echo $i
done

# Fayllar ustida
for FAYL in *.txt; do
    echo "Fayl: $FAYL"
    wc -l "$FAYL"
done

# Massiv ustida
SERVERLAR=("web1" "web2" "db1" "db2")
for SERVER in "${SERVERLAR[@]}"; do
    echo "Server tekshirilmoqda: $SERVER"
    ping -c 1 $SERVER > /dev/null && echo "$SERVER: OK" || echo "$SERVER: XATO"
done

# while tsikli
HISOBLAGICH=0
while [ $HISOBLAGICH -lt 5 ]; do
    echo "Hisoblagich: $HISOBLAGICH"
    ((HISOBLAGICH++))
done

# Fayl qatorlarini o'qish
while IFS= read -r QATOR; do
    echo "Qator: $QATOR"
done < serverlar.txt
```

---

## 3.6 Funksiyalar

```bash
#!/bin/bash

# Funksiya e'lon qilish
salomlash() {
    echo "Salom, $1!"
}

hisoblash() {
    local A=$1
    local B=$2
    local NATIJA=$((A + B))
    echo $NATIJA
}

server_tekshir() {
    local SERVER=$1
    if ping -c 1 $SERVER > /dev/null 2>&1; then
        echo "$SERVER: Ishlayapti"
        return 0
    else
        echo "$SERVER: Ishlamayapti!"
        return 1
    fi
}

# Funksiyani chaqirish
salomlash "Ali"
salomlash "Vali"

NATIJA=$(hisoblash 10 20)
echo "10 + 20 = $NATIJA"

server_tekshir "google.com"
```

---

## 3.7 Amaliy Skriptlar

```bash
#!/bin/bash
# backup.sh — papkani backup qilish

MANBA="/var/www/html"
MANZIL="/backup"
SANA=$(date +%Y%m%d_%H%M%S)
BACKUP_FAYL="backup_$SANA.tar.gz"

echo "Backup boshlanmoqda..."

# Backup papkasini yaratish
mkdir -p $MANZIL

# Arxiv yaratish
tar -czf $MANZIL/$BACKUP_FAYL $MANBA

if [ $? -eq 0 ]; then
    echo "Backup muvaffaqiyatli: $MANZIL/$BACKUP_FAYL"
    echo "Hajmi: $(du -sh $MANZIL/$BACKUP_FAYL | cut -f1)"
else
    echo "XATO: Backup amalga oshmadi!"
    exit 1
fi

# 7 kundan eski backuplarni o'chirish
find $MANZIL -name "backup_*.tar.gz" -mtime +7 -delete
echo "Eski backuplar o'chirildi"
```

---

# 4. Tarmoq Asoslari

## 4.1 IP Manzil

```
IP manzil: 192.168.1.100
            |   |  | |
            |   |  | └─ Host qismi (0-255)
            |   |  └─── Host qismi
            |   └────── Tarmoq qismi
            └─────────── Tarmoq qismi

Subnet mask: 255.255.255.0  yoki  /24
CIDR: 192.168.1.0/24  →  192.168.1.1 dan 192.168.1.254 gacha (254 ta qurilma)

Private IP diapazoni:
  10.0.0.0/8         →  10.x.x.x
  172.16.0.0/12      →  172.16.x.x - 172.31.x.x
  192.168.0.0/16     →  192.168.x.x
```

## 4.2 Portlar

```
Mashhur portlar:
  22    — SSH
  80    — HTTP
  443   — HTTPS
  3306  — MySQL
  5432  — PostgreSQL
  6379  — Redis
  27017 — MongoDB
  8080  — HTTP alternativa
  9090  — Prometheus
  3000  — Grafana / Node.js
```

## 4.3 DNS

```bash
# DNS tekshirish
nslookup google.com
dig google.com
dig google.com +short      # faqat IP

# /etc/hosts — lokal DNS
cat /etc/hosts
# 127.0.0.1    localhost
# 192.168.1.10 myserver.local
```

## 4.4 Firewall (UFW)

```bash
# UFW o'rnatish va yoqish
sudo apt install ufw
sudo ufw enable

# Qoidalar qo'shish
sudo ufw allow 22          # SSH
sudo ufw allow 80          # HTTP
sudo ufw allow 443         # HTTPS
sudo ufw allow 8080/tcp    # faqat TCP

# Bloklaş
sudo ufw deny 3306         # MySQL tashqaridan bloklash

# Holat ko'rish
sudo ufw status verbose

# O'chirish
sudo ufw delete allow 8080
```

---

# 5. Git va Version Control

## 5.1 Git Nima?

Git — kod o'zgarishlarini kuzatib boruvchi tizim. Har bir o'zgarish saqlanadi va kerak bo'lsa oldingi holatga qaytish mumkin.

## 5.2 Git O'rnatish va Sozlash

```bash
sudo apt install git

git config --global user.name "Ali Valiyev"
git config --global user.email "ali@example.com"
git config --global core.editor "nano"
git config --list
```

## 5.3 Asosiy Buyruqlar

```bash
# Yangi repo yaratish
git init

# Mavjud reponi yuklab olish
git clone https://github.com/user/repo.git
git clone git@github.com:user/repo.git     # SSH bilan

# Holat ko'rish
git status

# Fayllarni stage ga qo'shish
git add fayl.txt
git add .                   # barcha o'zgarishlar
git add src/                # papka

# Commit qilish
git commit -m "Login sahifasi qo'shildi"

# Uzoq repoga yuborish
git push origin main
git push origin feature/login   # branch

# Yangilanishlarni olish
git pull origin main

# Logni ko'rish
git log
git log --oneline           # qisqacha
git log --oneline --graph   # daraxt ko'rinishida
```

## 5.4 Branch (Shox)

```bash
# Branchlar ro'yxati
git branch
git branch -a               # remote branchlar ham

# Yangi branch yaratish
git branch feature/login
git checkout -b feature/login    # yaratib, o'tish

# Branch ga o'tish
git checkout main
git switch main             # zamonaviy usul

# Branch birlashtirish (merge)
git checkout main
git merge feature/login

# Branch o'chirish
git branch -d feature/login
git push origin --delete feature/login   # remote dan
```

## 5.5 Amaliy Ish Jarayoni (GitFlow)

```bash
# 1. Yangi xususiyat uchun branch ochish
git checkout -b feature/to-lov-tizimi

# 2. Kod yozish...

# 3. Commit qilish
git add .
git commit -m "To'lov tizimi API integratsiyasi"

# 4. Asosiy branch ni olish
git pull origin main

# 5. Merge qilish
git checkout main
git merge feature/to-lov-tizimi

# 6. Push qilish
git push origin main

# 7. Branchni o'chirish
git branch -d feature/to-lov-tizimi
```

## 5.6 .gitignore

```
# .gitignore fayli — bu fayllarni git kuzatmasin

# Muhit o'zgaruvchilari
.env
.env.local
.env.production

# Dependency papkalar
node_modules/
vendor/
__pycache__/
*.pyc

# Build fayllar
dist/
build/
*.min.js

# IDE fayllar
.vscode/
.idea/
*.swp

# Log fayllar
*.log
logs/

# OS fayllar
.DS_Store
Thumbs.db
```

---

# 6. Docker

## 6.1 Docker Nima?

Docker — ilovalarni **konteyner** deb ataluvchi izolyatsiyalangan muhitda ishga tushirish texnologiyasi.

```
MUAMMO: "Menda ishlaydi, serverda ishlamaydi"
YECHIM:  Docker — har yerda bir xil muhit
```

```
Virtual Machine (VM)         Docker Container
┌─────────────────┐          ┌─────────────────┐
│   App           │          │   App           │
│   Libraries     │          │   Libraries     │
│   Guest OS      │          │   (OS yo'q!)    │
│   Hypervisor    │          │   Docker Engine  │
│   Host OS       │          │   Host OS       │
└─────────────────┘          └─────────────────┘
Hajmi: GB                    Hajmi: MB
Tezlik: sekin                Tezlik: tez
```

## 6.2 Docker O'rnatish

```bash
# Ubuntu ga Docker o'rnatish
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Foydalanuvchini docker guruhiga qo'shish (sudo siz ishlash uchun)
sudo usermod -aG docker $USER
newgrp docker

# Tekshirish
docker --version
docker run hello-world
```

## 6.3 Asosiy Docker Buyruqlari

```bash
# Image yuklab olish
docker pull nginx
docker pull ubuntu:22.04
docker pull python:3.11-slim

# Imagelar ro'yxati
docker images

# Konteyner ishga tushirish
docker run nginx
docker run -d nginx                          # fon rejimida
docker run -d -p 8080:80 nginx              # port ulash
docker run -d -p 8080:80 --name mening-nginx nginx   # nom berish
docker run -it ubuntu:22.04 bash            # interaktiv terminal

# Konteynerlar ro'yxati
docker ps                   # ishlayotganlar
docker ps -a                # hammasi

# Konteyner boshqarish
docker stop mening-nginx
docker start mening-nginx
docker restart mening-nginx

# Konteyner ichiga kirish
docker exec -it mening-nginx bash
docker exec mening-nginx nginx -t           # buyruq bajarish

# Loglar
docker logs mening-nginx
docker logs -f mening-nginx                 # real vaqtda

# O'chirish
docker rm mening-nginx                      # konteyner
docker rmi nginx                            # image
docker system prune                         # ishlatilmaganlarni tozalash

# Ma'lumot
docker inspect mening-nginx
docker stats                                # resurslar
```

## 6.4 Dockerfile

```dockerfile
# Dockerfile — image qurish ko'rsatmasi

# Asosiy image
FROM python:3.11-slim

# Ishchi papka
WORKDIR /app

# Paketlar faylini ko'chirish
COPY requirements.txt .

# Paketlarni o'rnatish
RUN pip install --no-cache-dir -r requirements.txt

# Kod fayllarini ko'chirish
COPY . .

# Port ochish (hujjat uchun, majburiy emas)
EXPOSE 8000

# Muhit o'zgaruvchisi
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Ishga tushirish buyrug'i
CMD ["python", "app.py"]
```

```bash
# Image qurish
docker build -t mening-ilova:1.0 .
docker build -t mening-ilova:latest .

# Ishga tushirish
docker run -d -p 8000:8000 mening-ilova:1.0
```

## 6.5 Docker Compose

```yaml
# docker-compose.yml — bir nechta konteyner boshqarish

version: '3.8'

services:
  # Web ilova
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - DEBUG=False
    volumes:
      - ./src:/app/src
    depends_on:
      - db
      - redis
    restart: unless-stopped

  # Ma'lumotlar bazasi
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  # Cache
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  # Nginx (reverse proxy)
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - web

volumes:
  postgres_data:
```

```bash
# Docker Compose buyruqlari
docker compose up -d            # ishga tushirish
docker compose down             # to'xtatish
docker compose logs -f          # loglar
docker compose ps               # holat
docker compose exec web bash    # konteyner ichiga kirish
docker compose pull             # imagelarni yangilash
docker compose build            # qayta qurish
```

## 6.6 Docker Registry

```bash
# Docker Hub ga yuklash
docker login
docker tag mening-ilova:1.0 username/mening-ilova:1.0
docker push username/mening-ilova:1.0

# Private registry
docker tag mening-ilova:1.0 registry.company.com/mening-ilova:1.0
docker push registry.company.com/mening-ilova:1.0
```

---

# 7. CI/CD

## 7.1 CI/CD Nima?

```
CI = Continuous Integration   → Doimiy Integratsiya
CD = Continuous Delivery      → Doimiy Yetkazib Berish
CD = Continuous Deployment    → Doimiy Deploy
```

**Jarayon:**
```
Kod yozildi → Git push → Avtomatik:
  ✓ Testlar ishlaydi
  ✓ Kod sifati tekshiriladi
  ✓ Docker image quriladi
  ✓ Serverga deploy qilinadi
```

## 7.2 GitHub Actions

```yaml
# .github/workflows/main.yml

name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    name: Test
    runs-on: ubuntu-latest

    steps:
      - name: Kodni yuklab olish
        uses: actions/checkout@v4

      - name: Python o'rnatish
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Paketlarni o'rnatish
        run: |
          pip install -r requirements.txt
          pip install pytest

      - name: Testlarni ishlatish
        run: pytest tests/ -v

  build:
    name: Docker Image Qurish
    runs-on: ubuntu-latest
    needs: test     # test muvaffaqiyatli bo'lsa

    steps:
      - uses: actions/checkout@v4

      - name: Docker Hub ga login
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Docker image qurish va yuklash
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            username/myapp:latest
            username/myapp:${{ github.sha }}

  deploy:
    name: Serverga Deploy
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'

    steps:
      - name: SSH orqali deploy
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            docker pull username/myapp:latest
            docker compose -f /app/docker-compose.yml up -d
            docker system prune -f
```

## 7.3 GitLab CI/CD

```yaml
# .gitlab-ci.yml

stages:
  - test
  - build
  - deploy

variables:
  DOCKER_IMAGE: registry.gitlab.com/$CI_PROJECT_PATH

test:
  stage: test
  image: python:3.11
  script:
    - pip install -r requirements.txt
    - pytest tests/ -v --junitxml=report.xml
  artifacts:
    reports:
      junit: report.xml

build:
  stage: build
  image: docker:24
  services:
    - docker:24-dind
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $DOCKER_IMAGE:$CI_COMMIT_SHA .
    - docker push $DOCKER_IMAGE:$CI_COMMIT_SHA
    - docker tag $DOCKER_IMAGE:$CI_COMMIT_SHA $DOCKER_IMAGE:latest
    - docker push $DOCKER_IMAGE:latest
  only:
    - main

deploy:
  stage: deploy
  script:
    - ssh ubuntu@$SERVER_IP "
        docker pull $DOCKER_IMAGE:latest &&
        docker compose up -d &&
        docker system prune -f"
  only:
    - main
  environment:
    name: production
```

---

# 8. Kubernetes

## 8.1 Kubernetes Nima?

Kubernetes (K8s) — Docker konteynerlarini avtomatik boshqaruvchi tizim.

```
Muammo: 100 ta server, 500 ta konteyner — qanday boshqarish?
Yechim: Kubernetes — hamma narsani avtomatik qiladi

Kubernetes nima qiladi:
  ✓ Konteynerlarni serverlar bo'ylab taqsimlaydi
  ✓ Agar konteyner o'lib qolsa, yangi ochadi
  ✓ Yukni serverlar o'rtasida teng taqsimlaydi
  ✓ Yangi versiyani zero-downtime bilan deploy qiladi
  ✓ Avtomatik masshtablash (ko'p traffic → ko'p konteyner)
```

## 8.2 Arxitektura

```
┌─────────────────── Kubernetes Cluster ──────────────────┐
│                                                          │
│  Control Plane (Master):                                 │
│  ├── API Server      → barcha so'rovlar shu orqali       │
│  ├── etcd            → ma'lumotlar bazasi                │
│  ├── Scheduler       → qaysi nodega joylashtirilsin      │
│  └── Controller Mgr  → kerakli holatni ta'minlaydi       │
│                                                          │
│  Worker Nodes (ishchi serverlar):                        │
│  ├── Node 1                                              │
│  │   ├── kubelet     → K8s agenti                        │
│  │   ├── kube-proxy  → tarmoq qoidalari                  │
│  │   ├── Pod (nginx konteyner)                           │
│  │   └── Pod (python konteyner)                          │
│  ├── Node 2                                              │
│  │   ├── Pod (nginx konteyner)                           │
│  │   └── Pod (postgres konteyner)                        │
└─────────────────────────────────────────────────────────┘
```

## 8.3 O'rnatish (Lokal — Minikube)

```bash
# Minikube o'rnatish
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# kubectl o'rnatish
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install kubectl /usr/local/bin/kubectl

# Ishga tushirish
minikube start
minikube status
kubectl cluster-info
```

## 8.4 kubectl Asosiy Buyruqlar

```bash
# Resurslar ko'rish
kubectl get pods
kubectl get pods -n kube-system     # namespace bilan
kubectl get pods -A                 # barcha namespace
kubectl get deployments
kubectl get services
kubectl get nodes
kubectl get all                     # hamma narsa

# Batafsil ma'lumot
kubectl describe pod nginx-pod
kubectl describe node worker-1

# Loglar
kubectl logs nginx-pod
kubectl logs -f nginx-pod           # real vaqtda
kubectl logs nginx-pod -c container1  # ko'p konteynerli pod

# Pod ichiga kirish
kubectl exec -it nginx-pod -- bash
kubectl exec -it nginx-pod -- sh

# Fayl nusxalash
kubectl cp nginx-pod:/etc/nginx/nginx.conf ./nginx.conf

# O'chirish
kubectl delete pod nginx-pod
kubectl delete -f deployment.yaml

# Masshtab
kubectl scale deployment nginx --replicas=5

# Port forward (lokal test uchun)
kubectl port-forward pod/nginx-pod 8080:80
kubectl port-forward service/nginx-svc 8080:80
```

## 8.5 Pod

```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.25
      ports:
        - containerPort: 80
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

```bash
kubectl apply -f pod.yaml
kubectl get pods
```

## 8.6 Deployment

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3           # 3 ta pod
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.25
          ports:
            - containerPort: 80
          env:
            - name: MUHIT
              value: "production"
          resources:
            requests:
              memory: "64Mi"
              cpu: "250m"
            limits:
              memory: "128Mi"
              cpu: "500m"
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
```

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl rollout status deployment/nginx-deployment

# Yangi versiyaga o'tish
kubectl set image deployment/nginx-deployment nginx=nginx:1.26
kubectl rollout status deployment/nginx-deployment

# Oldingi versiyaga qaytish
kubectl rollout undo deployment/nginx-deployment
```

## 8.7 Service

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx          # bu labelga ega podlarga ulasin
  ports:
    - protocol: TCP
      port: 80          # Service porti
      targetPort: 80    # Pod porti
  type: ClusterIP       # faqat cluster ichida

---
# Tashqaridan kirish uchun NodePort
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080   # 30000-32767 oralig'i
  type: NodePort
```

## 8.8 ConfigMap va Secret

```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: "postgres-service"
  DATABASE_PORT: "5432"
  APP_ENV: "production"

---
# secret.yaml (base64 encoded!)
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DATABASE_PASSWORD: cGFzc3dvcmQxMjM=   # base64: password123
  API_KEY: bXlzZWNyZXRrZXk=            # base64: mysecretkey
```

```bash
# Secret yaratish (buyruq bilan, base64 siz)
kubectl create secret generic app-secret \
  --from-literal=DATABASE_PASSWORD=password123 \
  --from-literal=API_KEY=mysecretkey

# ConfigMap/Secret ni deployment da ishlatish
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
```

---

# 9. Terraform

## 9.1 Terraform Nima?

Terraform — infratuzilmani **kod** sifatida yaratish vositasi (Infrastructure as Code).

```
ESKI USUL: AWS console ga kirib, qo'lda server yaratish
TERRAFORM:  Kod yozasiz → terraform apply → server avtomatik yaratiladi

Afzalliklar:
  ✓ Takrorlanadigan (bir xil infra istalgan joyda)
  ✓ Versiyalash (git bilan)
  ✓ Hujjatlash (kod = hujjat)
  ✓ Avtomatlashtirish
```

## 9.2 O'rnatish

```bash
# Ubuntu
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

terraform --version
```

## 9.3 Asosiy Strukturа

```hcl
# main.tf

# Provider — qaysi cloud
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# Resurs yaratish — EC2 server
resource "aws_instance" "web_server" {
  ami           = "ami-0c02fb55956c7d316"   # Ubuntu 22.04
  instance_type = "t2.micro"

  tags = {
    Name = "WebServer"
    Env  = "Production"
  }
}

# S3 Bucket yaratish
resource "aws_s3_bucket" "static_files" {
  bucket = "mening-saytim-static-2024"
}
```

## 9.4 Asosiy Buyruqlar

```bash
# Boshlash
terraform init          # providerlarni yuklab olish

# Reja ko'rish (hech narsa o'zgartirmaydi)
terraform plan

# Infrani yaratish
terraform apply
terraform apply -auto-approve   # tasdiqlashsiz

# Holatni ko'rish
terraform show
terraform state list

# O'chirish
terraform destroy
terraform destroy -target aws_instance.web_server   # faqat bittasini
```

## 9.5 O'zgaruvchilar

```hcl
# variables.tf
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "db_password" {
  description = "Database password"
  type        = string
  sensitive   = true    # logda ko'rinmaydi
}

# main.tf da ishlatish
provider "aws" {
  region = var.region
}

resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

```bash
# O'zgaruvchi qiymati berish
terraform apply -var="region=eu-west-1"
terraform apply -var-file="prod.tfvars"

# terraform.tfvars fayli (avtomatik o'qiladi)
# region        = "eu-west-1"
# instance_type = "t3.medium"
```

---

# 10. Ansible

## 10.1 Ansible Nima?

Ansible — serverlarni avtomatik sozlash vositasi.

```
MUAMMO: 50 ta serverga nginx o'rnatish, sozlash
YECHIM:  Ansible — bir buyruq bilan hammasini qiladi

Afzalliklar:
  ✓ Agentsiz (SSH orqali ishlaydi)
  ✓ YAML (o'rganish oson)
  ✓ Idempotent (bir necha marta ishlatsa ham xavfsiz)
```

## 10.2 O'rnatish

```bash
sudo apt install ansible
ansible --version
```

## 10.3 Inventory

```ini
# inventory.ini — serverlar ro'yxati

[web_serverlar]
web1 ansible_host=192.168.1.10
web2 ansible_host=192.168.1.11

[db_serverlar]
db1 ansible_host=192.168.1.20
db2 ansible_host=192.168.1.21

[barcha:children]
web_serverlar
db_serverlar

[barcha:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

```bash
# Ulanishni tekshirish
ansible all -i inventory.ini -m ping
ansible web_serverlar -i inventory.ini -m ping

# Bir buyruq bajarish
ansible all -i inventory.ini -m shell -a "uptime"
ansible all -i inventory.ini -m shell -a "df -h"
```

## 10.4 Playbook

```yaml
# nginx_setup.yml
---
- name: Web serverlarni sozlash
  hosts: web_serverlar
  become: yes       # sudo bilan

  vars:
    nginx_port: 80
    app_papka: /var/www/html

  tasks:
    - name: Apt cache yangilash
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Nginx o'rnatish
      apt:
        name: nginx
        state: present

    - name: Nginx ishga tushirish
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Sayt papkasini yaratish
      file:
        path: "{{ app_papka }}"
        state: directory
        owner: www-data
        group: www-data
        mode: '0755'

    - name: Nginx konfiguratsiyasi
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
      notify: Nginx restart

    - name: Firewall sozlash
      ufw:
        rule: allow
        port: "{{ nginx_port }}"
        proto: tcp

  handlers:
    - name: Nginx restart
      service:
        name: nginx
        state: restarted
```

```bash
# Playbook ishga tushirish
ansible-playbook -i inventory.ini nginx_setup.yml

# Test rejimi (hech narsa o'zgartirmaydi)
ansible-playbook -i inventory.ini nginx_setup.yml --check

# Faqat bitta task
ansible-playbook -i inventory.ini nginx_setup.yml --tags "nginx"

# Verbose
ansible-playbook -i inventory.ini nginx_setup.yml -v
```

---

# 11. Monitoring

## 11.1 Nima uchun Monitoring?

```
Monitoring bo'lmasa:
  → Foydalanuvchi: "Sayt ishlamayapti!"
  → Siz: "Oh yo'q... qachondan?"

Monitoring bilan:
  → Sistem: "OGOHLANTIRISH: CPU 90%dan oshdi!"
  → Siz: muammo foydalanuvchi sezmasdan oldin hal qilinadi
```

## 11.2 Prometheus + Grafana

```yaml
# docker-compose.yml — Monitoring stack

version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=30d'

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin123
    volumes:
      - grafana_data:/var/lib/grafana
    depends_on:
      - prometheus

  node-exporter:
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro

volumes:
  prometheus_data:
  grafana_data:
```

```yaml
# prometheus.yml — sozlash

global:
  scrape_interval: 15s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']

rule_files:
  - "alert_rules.yml"

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

  - job_name: 'nginx'
    static_configs:
      - targets: ['nginx-exporter:9113']
```

## 11.3 Alert Qoidalari

```yaml
# alert_rules.yml
groups:
  - name: server_alerts
    rules:
      - alert: ServerDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Server ishlamayapti: {{ $labels.instance }}"

      - alert: HighCPU
        expr: 100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "CPU yuqori: {{ $value }}%"

      - alert: DiskSpaceLow
        expr: (node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100 < 10
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Disk bo'sh joyi: {{ $value }}%"
```

---

# 12. AWS Asoslari

## 12.1 Asosiy Xizmatlar

```
EC2   → Virtual serverlar
S3    → Fayl saqlash (Object Storage)
RDS   → Boshqariladigan ma'lumotlar bazasi
VPC   → Virtual shaxsiy tarmoq
IAM   → Kirish huquqlari boshqaruvi
ELB   → Load Balancer
EKS   → Boshqariladigan Kubernetes
Lambda → Serverless funksiyalar
```

## 12.2 EC2 (Server) Yaratish

```bash
# AWS CLI o'rnatish
sudo apt install awscli
aws configure
# AWS Access Key ID: [...]
# AWS Secret Access Key: [...]
# Default region name: us-east-1
# Default output format: json

# EC2 yaratish
aws ec2 run-instances \
  --image-id ami-0c02fb55956c7d316 \
  --instance-type t2.micro \
  --key-name mening-kalit \
  --security-group-ids sg-xxxxx \
  --subnet-id subnet-xxxxx \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]'

# Serverlar ro'yxati
aws ec2 describe-instances --query 'Reservations[].Instances[].{ID:InstanceId,IP:PublicIpAddress,State:State.Name}'

# SSH bilan ulanish
ssh -i mening-kalit.pem ubuntu@<IP_MANZIL>
```

## 12.3 S3 (Fayl Saqlash)

```bash
# Bucket yaratish
aws s3 mb s3://mening-bucket-nomi

# Fayl yuklash
aws s3 cp fayl.txt s3://mening-bucket/
aws s3 cp papka/ s3://mening-bucket/papka/ --recursive

# Fayllar ro'yxati
aws s3 ls s3://mening-bucket/

# Yuklab olish
aws s3 cp s3://mening-bucket/fayl.txt ./

# O'chirish
aws s3 rm s3://mening-bucket/fayl.txt
aws s3 rb s3://mening-bucket --force    # bucket o'chirish
```

## 12.4 IAM (Xavfsizlik)

```
Eng muhim qoidalar:
  ✓ Hech qachon root account ishlatmang
  ✓ Har bir kishi/servis uchun alohida user/role
  ✓ Minimal ruxsat berish (Principle of Least Privilege)
  ✓ MFA (2FA) yoqing
  ✓ Access key larni kod ichida yozmang
```

---

## Xulosa va Keyingi Qadamlar

### O'rganish tartibi:
```
1. Linux (2-4 hafta)
   └── Terminal, Bash, Fayl tizimi, Xizmatlar

2. Git (1-2 hafta)
   └── Asosiy buyruqlar, Branching, GitHub

3. Docker (2-3 hafta)
   └── Konteyner, Dockerfile, Compose

4. CI/CD (2-3 hafta)
   └── GitHub Actions yoki GitLab CI

5. Kubernetes (4-6 hafta)
   └── Pod, Deployment, Service, kubectl

6. Cloud - AWS (4-6 hafta)
   └── EC2, S3, VPC, IAM

7. Terraform (2-3 hafta)
   └── Infratuzilmani kod bilan yaratish

8. Ansible (2-3 hafta)
   └── Server konfiguratsiyasini avtomatlashtirish

9. Monitoring (2-3 hafta)
   └── Prometheus, Grafana, Alertlar
```

### Amaliyot loyihalari:
1. **Loyiha 1:** Django/Flask ilovani Docker bilan ishga tushirish
2. **Loyiha 2:** GitHub Actions bilan CI/CD pipeline yaratish
3. **Loyiha 3:** Kubernetes da multi-service ilova
4. **Loyiha 4:** Terraform bilan AWS infra yaratish
5. **Loyiha 5:** To'liq monitoring stack sozlash

### Foydali manbalar:
- **roadmap.sh/devops** — interaktiv roadmap
- **TechWorld with Nana** — YouTube
- **KodeKloud** — amaliyot platformasi
- **Linux Foundation** — sertifikatlar
- **AWS Free Tier** — bepul bulut amaliyoti

---

> Har kuni kamida 1-2 soat o'rganing va amaliyot qiling.
> Nazariya + amaliyot = muvaffaqiyat!
