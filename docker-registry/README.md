## Ubuntu, Debian
```
$ apt-get install -y ca-certificates
$ kubectl get secret private-registry-certs -ojson |jq .data.\"ca.crt\" -r | base64 -d >> /usr/local/share/ca-certificates/custom-ca.crt
$ update-ca-certificates

## 다른 노드가 있을 경우
$ scp /usr/local/share/ca-certificates/custom-ca.crt node01:/usr/local/share/ca-certificates/
$ ssh node01
$ apt-get install -y ca-certificates
$ update-ca-certificates
```

## RHEL, CentOS
```
$ yum install -y ca-certificates
$ kubectl get secret private-registry-certs -ojson |jq .data.\"ca.crt\" -r | base64 -d >> /etc/pki/ca-trust/source/anchors/custom-ca.crt
$ update-ca-trust
```