---
weight: 70
title: openssl
---

# openssl

## cert

## key

## hash

## s_connect

> 查看服务器证书

```shell

openssl s_client -showcerts -servername httpbin.org \
    -connect httpbin.org:443 </dev/null

```

## secp256k1
> 生成 secp256k1 公钥和私钥

```shell
# Install keccak-256sum
brew install sha3sum

# Generate the private key in pem format
openssl ecparam -name secp256k1 -genkey -noout > private-key.pem

# Generate the public key in pem format from private key
cat private-key.pem | openssl ec -pubout > public-key.pem

# Generate the private and public keys in hex format from private key in pem format
cat private-key.pem | openssl ec -text -noout > key.txt

# Generate the public key in hex format from public key in pem format
cat public-key.pem | openssl ec -pubin -text -noout > pub-key.txt

# Extract the public key and remove the EC prefix 0x04
cat key.txt | grep pub -A 5 | tail -n +2 | tr -d '\n[:space:]:' | sed 's/^04//' > pub.hex
cat pub-key.txt | grep pub -A 5 | tail -n +2 | tr -d '\n[:space:]:' | sed 's/^04//' > pub.hex

# Extract the private key and remove the leading zero byte
cat key.txt | grep priv -A 3 | tail -n +2 | tr -d '\n[:space:]:' | sed 's/^00//' > priv.hex

# Generate the hash and take the address part
cat pub.hex | keccak-256sum -x -l | tr -d ' -' | tail -c 41 > address.txt
```
