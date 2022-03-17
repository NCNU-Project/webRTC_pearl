# webRTC_pearl
## before start
### edit the zone file `vim bind/zones/db.pearl.club`, change line 18's ip to server's ip
```
@    IN   A    YOUR_IP
```
### generate the ssl certificate
```
# at the repo's root directory
# create a folder consisted certificate
$ mkdir ssl

# install the certificate helper
$ sudo ssl.ca/install.sh

# generate the root certificate
$ new-root-ca

# generate the server's private key and CSR
$ new-server-cert www.pearl.lab

# use the ca's private key to sign the CSR
$ sign-server-cert www.pearl.lab
```
### import the root CA
1. open `chrome://settings/security`, click manage certificate
2. goto `Authorities` tab, and click `import` button
3. import the root certificate `ssl/ca.crt`

## how to start?
### use docker compose
`sudo docker-compose -f docker-compose.yml up`

### change dns(ubuntu, local environment)
1. because 53 port has been used by systemd-resolved, so we need to find dns container's briged ip
  ```
  $ docker inspect webrtc_pearl_bind_1 -f \
    '{{ .NetworkSettings.Networks.webrtc_pearl_default.IPAddress }}
  ```
2. add the dns container's ip to `/etc/systemd/resolved.conf`
    ```
    [Resolve]
    DNS=<must be replace by the container's ip>
    #FallbackDNS=8.8.8.8
    Domains=hellostrong.org
    #LLMNR=no
    #MulticastDNS=no
    #DNSSEC=no
    #DNSOverTLS=no
    #Cache=no-negative
    #DNSStubListener=yes
    #ReadEtcHosts=yes
    ```
3. link the systemd-resolved managed resolve.conf file to `/etc/resolv.conf`
    ```
    sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
    ```
