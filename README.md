macOS
=====

`docker network create web`

`brew install mkcert`

`brew install nss`

`brew install dnsmasq`

`mkdir -pv $(brew --prefix)/etc/`

`echo 'address=/.test/127.0.0.1' >> $(brew --prefix)/etc/dnsmasq.conf`

`sudo mkdir -v /etc/resolver`

In `/etc/resolver/docker.test`:

```
    nameserver 127.0.0.1
```

`sudo killall -HUP mDNSResponder`

`cp .env.example .env`

`mkcert -install`

`mkcert -key-file ./certs/key.pem -cert-file ./certs/cert.pem test 'docker.test' '*.docker.test'`

`docker network create web`

`docker-compose up -d`


Linux
=====

`docker network create web`

`sudo systemctl disable systemd-resolved`

`sudo systemctl stop systemd-resolved`

`ls -lh /etc/resolv.conf`

`sudo rm /etc/resolv.conf`

`sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolv.conf'`

`sudo bash -c 'echo "nameserver 1.1.1.1" >> /etc/resolv.conf'`

`sudo vim /etc/NetworkManager/NetworkManager.conf`
    In section `[main]`:

    `dns=none`
`sudo service network-manager restart`

`sudo apt install dnsmasq`

`sudo bash -c 'echo "address=/.test/127.0.0.1" >> /etc/dnsmasq.conf'`

`sudo mkdir -v /etc/resolver && sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'`

`sudo systemctl restart dnsmasq`

`sudo apt install libnss3-tools -y`

`wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-amd64`

`sudo mv mkcert-v1.4.3-linux-amd64 /usr/local/bin/mkcert && chmod +x /usr/local/bin/mkcert`

`mkcert -install`

`mkcert -key-file ./certs/key.pem -cert-file ./certs/cert.pem test 'docker.test' '*.docker.test'`

`cp .env.example .env`

`docker-compose up -d`


Windows
=======

Running Docker commands from Windows Terminal (https://aka.ms/terminal) or the VS Code (https://code.visualstudio.com/) terminal is recommended.

`docker network create web`

Install Chocolately from https://chocolatey.org/install and make sure you install it using Powershell as an admin (Windows Powershell in the Start Menu, right click -> Run as administrator)

`Get-ExecutionPolicy`

If it's `Restricted`, run `Set-ExecutionPolicy AllSigned`

`Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`

`choco install mkcert`

`mkcert -install`

`mkcert -key-file ./certs/key.pem -cert-file ./certs/cert.pem test 'docker.test' '*.docker.test'`

`cp .env.example .env`

`docker-compose up -d`

You can't use `dnsmasq` on Windows, so you'll need to add the entries to the hosts file manually.

1. Find Notepad in the Start Menu, right click -> Run as administrator
2. Open `C:\Windows\System32\drivers\etc\hosts` (you will need to select the All Files file filter option).
3. Add these three entries to the end of the file:

```
    127.0.0.1 docker.test
    127.0.0.1 whoami.docker.test
    127.0.0.1 portainer.docker.test
```

4. Save and quit

NB: You will need to add each new project to the `hosts` file in a similar way, with `127.0.0.1` at the start of the line, then the domain of the project.

After Installation
==================

Visit these sites in your browser (only Chrome on Windows):

-> https://docker.test/dashboard/
-> https://whoami.docker.test/
-> https://portainer.docker.test/#/home

Note that these are HTTPS sites, unlike the insecure `localhost` you would get with Docker normally. Browsers restrict certain features (eg: geolocation) to HTTPS.

Add or create other projects that use **localhoster**, eg: https://github.com/hendynz/myrails

References
==========

https://github.com/FiloSottile/mkcert

Windows:

https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/

Mac:

https://vninja.net/2020/02/06/macos-custom-dns-resolvers/

https://github.com/birros/docker-traefik-mkcert

Linux:

https://medium.com/soulweb-academy/docker-local-dev-stack-with-traefik-https-dnsmasq-locally-trusted-certificate-for-ubuntu-20-04-5f036c9af83d