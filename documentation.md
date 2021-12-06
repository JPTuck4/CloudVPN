# Cloud VPN Documentation

## 1: Create an Ubuntu 20.04 Droplet
Go to https://m.do.co/c/4d7f4ff9cfe4 and sign up for a new account

Choose the cheapest droplet ($5/month)

Choose preferred Data Center

Choose password and create a password

## 2: Install Docker
Install Necessary Tools:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

Add docker Key:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add Docker Repo for 64 bit PC:
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Switch to correct Repo:
```
apt-cache policy docker-ce
```

Install Docker:
```
sudo apt install docker-ce -y
```
## 3: Install Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
Set permission:
```
sudo chmod +x /usr/local/bin/docker-compose
```

## 4: Set up wiregaurd:
Run these:
```
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```

Copy and paste this into ~/wiregaurd/docker-compose.yml:
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - SERVERURL=188.166.173.247
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

Hit CTRL + X, Y, ENTER to save and exit the file.

Start wireguard:
```
cd ~/wireguard/
```
```
docker-compose up -d
```

## 5: Connect Mobile Device to VPN:
```
docker-compose logs -f wireguard
```
Open Wireguard VPN application on your phone, click +, Create from QR code

Before:

<img src="image2.png?raw=true&s=10" width="300">


After:

<img src="image0.png?raw=true&s=10" width="300">
<img src="image1.png?raw=true&s=10" width="300">

## 6: Connect PC to VPN:
Download wireguard on laptop

Open config file
```
cat ~/wireguard/config/peer_pc1/peer_pc1.conf
```

Copy contents and paste into empty tunnel in wireguard

Activate Tunnel

<img src="Capture.png?raw=true&s=10" width="300">
<img src="Capture1.png?raw=true&s=10" width="300">
