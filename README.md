macOS
=====

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

`mkdir certs`

In `.env`:

```
    DOCKER_NAME=localhoster
    DOCKER_BASE_URL=localhoster.docker.test
```

`mkcert -install`
`mkcert -key-file ./certs/key.pem -cert-file ./certs/cert.pem test 'docker.test' '*.docker.test'`

`docker network create web`

`docker-compose -f traefik.yaml up -d`

-> https://docker.test/dashboard/
-> https://whoami.docker.test/
-> https://portainer.docker.test/#/home


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

`docker-compose -f traefik.yaml up -d`

References
==========

Mac:

https://github.com/FiloSottile/mkcert

https://vninja.net/2020/02/06/macos-custom-dns-resolvers/

https://github.com/birros/docker-traefik-mkcert

Linux:

https://medium.com/soulweb-academy/docker-local-dev-stack-with-traefik-https-dnsmasq-locally-trusted-certificate-for-ubuntu-20-04-5f036c9af83d