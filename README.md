=============================================================
This project was part of the [Cartesi Online Hackathon 2023](https://taikai.network/cartesi/hackathons/cartesi-hackathon)
=============================================================
## About

In early 2023, Bard (Google’s A.I chatbot) made claims that Google used Gmail data to train its A.I model. Part of this data included users’ personal emails and conversations. 

This brings into focus whether we can trust these big companies that manage our email servers, hence the need for a decentralized email service - **YourMailer**. 

## Aims and Objectives 

- To build an email service where all interactions between the client and server are logged onto the blockchain (1)
- To build an email service where any access to user data is logged onto the blockchain (2)

## Setting Up

The sections below intend to show how you can test the feasibilty of my application on your local environment (i.e to show that it can be built on Cartesi Rollups). The project is built on industry-grade open-source software. This demostrates that the project can actually be useful in a real world setting. 

For the purpose of the PoC, only Objective (1) is achieved here.

## How the Email Service Works

![Architrecture]()

This is a description of the architecture. 

1. On the client side, you have an email client. For this, you can use an open-source email client called **Thunderbird**. 

2. The email client interacts with the **Nginx server**. This server is a transparent proxy that is used to route email requests for various domains. 

3. By design, Nginx has an **HTTP Authentication server** that determines the Upstream server for a given email request. We will use the embedded Tomcat Server, which ships with Spring Boot for our email service. 

4. This authentication server is where our application will interact with the Cartesi Rollups. Our application frontend sends the body of the email to the blockchain. The Cartesi backend then gets the data and sends a response to the Nginx proxy allowing it to route the email request to the appropriate Upstream server and port. 

(**Note:** It is not secure or practical to directly post users’ email messages on the blockchain. However, for the purpose of demonstration, we’ll be using that. In a practical setting, we could post a hash of the email body to the blockchain to prove that it was processed by our Cartesi DApp).

5. The Upstream server is Java Apache Mail Enterprise Server(JAMES). This processes the email request and relays it back to the Nginx server.  

## Tech Stack
This section describes the required technologies on the client and server side. 

## Client
Thunderbird Email Client. You can download this from [here](https://www.thunderbird.net/en-US/). If you are using Ubuntu, you can get it from the apt repository:

```sh 
sudo apt-get install thunderbird # at least v 102 
```

## Server

### 1. Nginx Proxy server (v 1.23.4 )

Please note that the one provided by the apt repository does not come with the mail modules installed. Therefore, you’ll need to manually compile Nginx from the source code. You can refer to [this documentation](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#sources) for how to compile and install it. Please note that as you are configuring the build options, you’ll need to include the  --with-mail and --with-mail_ssl_module options.

Here is the sample build configuration that I used:

```sh
   ./configure \
   --prefix=/etc/nginx \
   --conf-path=/etc/nginx/nginx.conf \
   --error-log-path=/var/log/nginx/error.log \
   --http-log-path=/var/log/nginx/access.log \
   --pid-path=/usr/local/nginx/nginx.pid \
   --with-pcre=./pcre2-10.40 \
   --with-zlib=./zlib-1.2.13 \
   --sbin-path=/usr/sbin/nginx \
   --with-http_ssl_module \
   --with-http_v2_module \
   --with-http_stub_status_module \
   --with-http_realip_module \
   --with-file-aio \
   --with-threads \
   --with-stream \
   --with-stream_ssl_preread_module \
   --with-mail \
   --with-mail_ssl_module \
   --with-openssl=./openssl-1.1.1p \
   --with-zlib=./zlib-1.2.13 \
   --http-client-body-temp-path=/tmp/client-body-path \
   --http-proxy-temp-path=/tmp/proxy-temp-path \
```

### 2. Spring Boot Application

First, you’ll need to install the following: Spring Boot v3.0.6, Maven 3.9.1, and Java 17. Please ensure that you Java 17 because Spring Boot v3.0.6 requires this to work.

Spring Boot comes embedded with Tomcat 10 server. This is the server we will be using as our Authentication server for Nginx requests.

### 3. JAMES (Java Apache Mail Enterprise Server)

We will use the JAMES demo Docker image provided by Apache. You can follow the [instructions here](https://github.com/apache/james-project/blob/master/docs/modules/servers/pages/15-minute-demo.adoc) on how to download and the use it. 

## Key Things to Note

This section highlights possible sources of errors that you need to pay attention to when setting up this email service on your local environment:

Remember to include the domain name that your JAMES server is using to the /etc/hosts file.
Since you are using a proxy, you’ll need to configure different ports on which your JAMES server accepts IMAP, SMTP and POP3 protocols. A key point in designing this application is that client should continue to use the standard ports that they are used to (i.e 25, 143, 110, 993…)
