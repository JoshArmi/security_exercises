# Self-Signed Certificates

In this exercise we're going to create our own SSL certificate and run nginx with that certificate.

## Run Nginx

First we need to run a Docker container with Nginx in it:

`docker run -p 443:443 -it nginx bash`

## Install OpenSSL

We're going to use openssl to generate the certificate and key we need to serve secured traffic

`apt-get update && apt-get install -y openssl`

OpenSSL is an implementation of the SSL standard, and is a good standard toolbox for dealing with certificates, you can use it to analyse existing certificates as well as create new ones as we're going to do here.

## Create the Certificate and Key

Now let's create the certificate

First we need to create a folder to put the certificate and key into

Run: `mkdir /root/certs && cd /root/certs`

And create the certificate and key

Run: `openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCertificate.crt -keyout MyKey.key`

It will prompt you to enter information, which in this case you can enter anything you want

Lets break down the options we selected:

`-newkey rsa:4096` - We're creating a 4096 bit RSA key, the longer the key, the more amount of time to crack
`-x509` - Create a self signed certificate
`-sha256` - Generate the certificate request with the 256-bit SHA (Secure Hash Algorithm)
`-days` - How long is the certificate valid for before it expires
`-nodes` - Do not require a passphrase for the certificate (otherwise we'd have to enter it every time we rebooted nginx)

## Configure Nginx to use the Certificate

In this case we're wanting to front the whole of Nginx with the certificate to we need to add to the global config

Install a text editor

Run: `apt-get install -y nano`

Open nginx.conf

Run: `nano /etc/nginx/nginx.conf`

Add these lines to the http block

```bash
http {
    ssl_certificate     /root/certs/MyCertificate.crt;
    ssl_certificate_key /root/certs/MyKey.key;
    ssl_ciphers         EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH;
    ssl_protocols       TLSv1.1 TLSv1.2;
```

Now we need to tell the server to listen on port 443 and over HTTPS

Open the default site configuration

Run: `nano /etc/nginx/conf.d/default.conf`

Add these lines to the server block at the top

```bash
server {
    listen              443 ssl default_server;
    listen              [::]:443 ssl default_server ;
```

Now as we overrode the command of the container we need to actually run `nginx`

Run: `nginx`

Now open your browser and go to `https://localhost`

You'll get a warning when you browse, why do you think that is?

If you accept and continue, then you'll get the nginx default page

## Great Success But We Can Do Better
