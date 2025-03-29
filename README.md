# 🔐 mTLS Lab com Docker, OpenSSL e cURL

This lab is a practive, step by step, on how to configure a mutual authentication TLS (mTLS) using:

 - OpenSSL for certificates generation;
 - NGINX as a server
 - cURL as a client
 - Docker/Docker compose for orchestration

 > [!WARNING] 
 > **Do not commit any private keys** (e.g., `ca.key`, `server.key`, `client.key`) to your repository. The `.gitignore` file is prepared to prevent accidental commits.

 ---

 ## 🧠 Basic Concepts

 ### What is TLS?
 TLS (Transport Layer Security) is a protocol that protects HTTPs connections. It authenticates the server, and encrypts the data traffic.

### What is mTLS?
 mTLS (mutual TLS) is a TLS extension where **client and server authenticates mutually**:
  - The server **verifies the client certificate**
  - The client **verifies the server certificate**

---

## 🧪 Etapa 1: Gerar certificados com OpenSSL

> Since this is a study notes repo, all the steps are done manually, for a complete understanding. All commands should be executed within the `certs/` folder.

### 📁 Expected Directory Structure

```
mtls-lab/
└── certs/
    ├── ca.crt / ca.key # CA certificate and private key (do not commit ca.key)
    ├── server.crt / server.key / server.csr # Server certificate, key and CSR (do not commit server.key) 
    ├── client.crt / client.key / client.csr # Client certificate, key and CSR (do not commit client.key)
```

---

### 🔹 1. Gerar a CA (Certificate Authority)

The CA is the entity that signs and validates the certificates for both the server and the client.

#### 1.1 Generate the CA Private Key
```bash
openssl genrsa -out ca.key 4096
```
> [!CAUTION]
> This creates the CA private key (ca.key), which will be used to sign the other certificates. Keep this file secret!

#### 1.2 Generate the CA Certificate (Self-signed)
```bash
openssl req -x509 -new -nodes -key ca.key -sha256 -days 365 \
  -out ca.crt \
  -subj "/C=BR/ST=DF/L=Brasilia/O=MyCA/CN=MyCA"
```
> [!IMPORTANT]
> This produces the CA certificate (ca.crt) valid for 1 year, used by both the server and the client to verify certificate authenticity.

--- 

### 🔹 2. Gerar the server certificate (for NGINX)

#### 2.1 Generate the Server Private Key
```bash
openssl genrsa -out server.key 4096
```
> [!CAUTION]
> The file server.key must be kept secure and should not be committed to any repository.



#### 2.2 Generate the Certificate Signing Request (CSR)
```bash
openssl req -new -key server.key -out server.csr \
  -subj "/C=BR/ST=DF/L=Brasilia/O=MyServer/CN=localhost"

```


#### 2.3 Sign the CSR with the CA
```bash
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key \
  -CAcreateserial -out server.crt -days 365 -sha256
```
> [!NOTE]
> After these commands, you have:
>
> server.crt: The server certificate.
>
> server.key: The server’s private key (keep it secure).
