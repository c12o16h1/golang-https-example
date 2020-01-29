## Example of https (tls) with self-signed certificate and golang server + client

### Generating certificates

1. Case where server called by domain (any domain name will work)
```
openssl req \
    -newkey rsa:2048 \
    -nodes \
    -days 3650 \
    -x509 \
    -keyout ca.key \
    -out ca.crt \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl req \
    -newkey rsa:2048 \
    -nodes \
    -keyout server.key \
    -out server.csr \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl req \
    -x509 \
    -nodes \
    -newkey rsa:2048 \
    -keyout client.key \
    -out client.crt \
    -days 3650 \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl x509 \
    -req \
    -days 365 \
    -sha256 \
    -in server.csr \
    -CA ca.crt \
    -CAkey ca.key \
    -CAcreateserial \
    -out server.crt
```

2. Case where server is IP (will work only on particular IP)

```
openssl req \
    -newkey rsa:2048 \
    -nodes \
    -days 3650 \
    -x509 \
    -keyout ca.key \
    -out ca.crt \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl req \
    -newkey rsa:2048 \
    -nodes \
    -keyout server.key \
    -out server.csr \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl req \
    -x509 \
    -nodes \
    -newkey rsa:2048 \
    -keyout client.key \
    -out client.crt \
    -days 3650 \
    -subj "/C=US/ST=California/L=Los Angeles/O=Good Company/OU=IT Department/CN=*"
openssl x509 \
    -req \
    -days 365 \
    -sha256 \
    -in server.csr \
    -CA ca.crt \
    -CAkey ca.key \
    -CAcreateserial \
    -out server.crt \
    -extfile <(echo subjectAltName = IP:127.0.0.1)
```

Where IP is `127.0.0.1` in this case.

### Usage
1. Run server `go run ./server/main.go`

2. Run client `go run ./client/main.go`
