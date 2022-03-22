# webRTC_pearl
## before start
1. change the default setting
    ```
    perl -i -pe "s/lab.test.ncnu.org/${USER}.lab.test.ncnu.org/g" ./webrtc/index.html ./docker-compose.yml ./ssl.ca/new-server-cert.sh
    ```
2. build your own docker image
    ```
    cd ./webrtc
    docker build -t webrtc-demo .
    ```
3. install the certificate helper
    ```
    cd ./ssl.ca
    sudo ./install.sh
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
    2. open `chrome://settings/security`, click manage certificate
    3. goto `Authorities` tab, and click `import` button
    4. import the root certificate `ssl/ca.crt`
    5. goto `chrome://restart` to restart the chrome 
    6. check whether CA(org-National Chi Nan University) is imported

## how to start?
### use docker compose
`sudo docker-compose -f docker-compose.yml up`
