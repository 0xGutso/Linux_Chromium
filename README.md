# Linux VDS'e Chromium Kurmak

- Linux'a Chromium kurarak çalıştırmak istediğimiz eklentileri, bilgisayarımıza kurmak yerine buraya kurup daha iyi puan alabilir ve daha iyi verim alabiliyoruz

# Gerekli Dosyaları Yükle
```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Docker version check
docker --version
```

# Zaman Dilimi Kontrolü
```
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

# Chromium Yükleme
1. Dosyaları yaratalım
```
mkdir chromium
cd chromium
```
2. Docker compose dosyası yaratalım
```
nano docker-compose.yaml
```
3. Kodu yapıştıralım ve kendimize göre düzenleyelim.
- ``CUSTOM_USER`` ve ``PASSWORD`` kısmını kendinize göre düzenleyin
- ``TZ`` = Zaman dilimi ayarı, kendinize göre ayarlayabilirsiniz
```
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #tercihen
    environment:
      - CUSTOM_USER=     #tercihen
      - PASSWORD=    #tercihen
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Istanbul
      - CHROME_CLI=https://github.com/0xGutso #tercihen
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```
- CTRL + X + Y + ENTER ile kayıt edip çıkıyoruz.

# Chromium çalıştırma
```
cd $HOME && cd chromium

docker compose up -d
```

# Sunucuya bağlanma
- https://Sunucu_IP:3010/

    ya da

- https://Sunucu_IP:3011/