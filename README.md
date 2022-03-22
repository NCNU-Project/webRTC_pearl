# webRTC_pearl
## before start
1. change the default setting
    ```
    perl -i -pe "s/lab.test.ncnu.org/${USER}.lab.test.ncnu.org/g" ./webrtc/index.html ./docker-compose.yml
    ```
2. build your own docker image
    ```
    cd ./webrtc
    docker build -t webrtc-demo .
    ```
3. install the certificate helper
    ```
    sudo ./ssl.ca/install.sh
    ```
4. generate the ssl certificate
    ```
    # at the repo's root directory
    # create a folder consisted certificate
    mkdir ssl
    
    # generate the root certificate
    new-root-ca
    
    # generate the server's private key and CSR
    new-server-cert {YOUR_NAME}.lab.test.ncnu.org
    
    # use the ca's private key to sign the CSR
    sign-server-cert {YOUR_NAME}.lab.test.ncnu.org
    ```
5. import the root CA
    1. `scp {YOUR_NAME}@{YOUR_NAME}.lab.test.ncnu.org:~/webRTC_pearl/ssl/ca.crt .`
    1. open `chrome://settings/security`, click manage certificate
    2. goto `Authorities` tab, and click `import` button
    3. import the root certificate `ssl/ca.crt`

## how to start?
### use docker compose
`sudo docker-compose -f docker-compose.yml up`
