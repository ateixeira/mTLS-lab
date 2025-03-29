# mTLS Lab com Docker, OpenSSL e cURL

This lab is a practive, step by step, on how to configure a mutual authentication TLS (mTLS) using:

 - OpenSSL for certificates generation;
 - NGINX as a server
 - cURL as a client
 - Docker/Docker compose for orchestration

 ---

 ## Basic Concepts

 ### What is TLS?
 TLS (Transport Layer Security) is a protocol that protects HTTPs connections. It authenticates the server, and encrypts the data traffic.

### What is mTLS?
 mTLS (mutual TLS) is a TLS extension where **client and server authenticates mutually**:
  - The server **verifies the client certificate**
  - The client **verifies the server certificate**

---


 