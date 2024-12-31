
```
[ec2-user@dev-001 ~]$ wget https://www.openssl.org/source/openssl-1.1.1d.tar.gz
--2021-11-16 13:54:25--  https://www.openssl.org/source/openssl-1.1.1d.tar.gz
Resolving www.openssl.org (www.openssl.org)... 184.27.21.43, 2600:140b:2:9a4::c1e, 2600:140b:2:9a6::c1e
Connecting to www.openssl.org (www.openssl.org)|184.27.21.43|:443... connected.
ERROR: cannot verify www.openssl.org's certificate, issued by ‘/C=US/O=Let's Encrypt/CN=R3’:
Issued certificate has expired.
To connect to www.openssl.org insecurely, use `--no-check-certificate'.
```

**Solution**:
```
yum update openssl openssl-devel -y
```
or:
```
yum update openssl openssl-devel -y
```
