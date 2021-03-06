# Summary
The toniebox communicates over https with its servers. It uses its Certificate Authority certifcate (flash:/cert/ca.der) to verify the tls connection. The box authenticates itself with a client certificate (flash:/cert/client.der) + private rsa key (flash:/cert/private.der).
## Known domains
* prod.de.tbs.toys
* rtnl.bxcl.de

# Certificate conversion
To use the certificates and the rsa key with most tools you will need to convert it from DER to PEM
```
openssl x509 -inform DER -outform PEM -in ca.der -out ca.crt
openssl x509 -inform DER -outform PEM -in client.der -out client.crt
openssl rsa -inform DER -outform PEM -in private.der -out private.key
```

# Generate custom root ca certificate
```
openssl genrsa -out ca-custom.key 4096
openssl req -new -x509 -days 1826 -key ca-custom.key -out ca.crt
```

# CA Certificate conversion
To use your generated ca certificate for the toniebox you will have to convert it back to DER format.
```
openssl x509 -inform PEM -outform DER -in ca-custom.crt -out ca-custom.der
```

# WIP Fake Server / Client - DOES NOT WORK!
No data seems to be send or received this way
https://github.com/toniebox-reverse-engineering/toniebox/tree/master/tools

# SSLSplit Command - DOES NOT WORK!
Connection isn't held
```
sslsplit -D -l /tmp/sslsplit/connections.log -j /tmp/sslsplit/ -S /tmp/sslsplit/logdir/ -c /tmp/sslsplit/ca.crt -k /tmp/sslsplit/ca.key -a /tmp/sslsplit/toniebox/client.crt -b /tmp/sslsplit/toniebox/private.key -W /tmp/sslsplit/gendir/ -M /tmp/sslsplit/sslkeylogfile.log -P https 0.0.0.0 8443
```

# SSLSplit Tutorial
https://blog.heckel.io/2013/08/04/use-sslsplit-to-transparently-sniff-tls-ssl-connections/