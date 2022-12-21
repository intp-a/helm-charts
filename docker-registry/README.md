## Ubuntu, Debian
```
## Root CA 추가
$ apt-get install -y ca-certificates
$ kubectl get secret private-registry-tls-cert -ojson |jq .data.\"ca.crt\" -r | base64 -d >> /usr/local/share/ca-certificates/custom-ca.crt
$ update-ca-certificates

## 데몬 리로드
$ systemctl restart containerd
$ systemctl restart kubelet

## Host File 업데이트
$ echo $(k get svc/private-registry-svc -ojson | jq .spec.clusterIP -r)" private-registry-svc.default.svc.cluster.local" >> /etc/hosts

## 다른 노드가 있을 경우
## Root CA 추가
$ scp /usr/local/share/ca-certificates/custom-ca.crt node01:/usr/local/share/ca-certificates/
$ ssh node01 "update-ca-certificates"

## 데몬 리로드
$ ssh node01 "systemctl restart containerd"
$ ssh node01 "systemctl restart kubelet"

## Host File 업데이트
$ CUSTOMHOST=$(cat /etc/hosts | grep private-registry)
$ ssh node01 "echo ${CUSTOMHOST} >> /etc/hosts"

```

## RHEL, CentOS
```
## Root CA 추가
$ yum install -y ca-certificates
$ kubectl get secret private-registry-tls-cert -ojson |jq .data.\"ca.crt\" -r | base64 -d >> /etc/pki/ca-trust/source/anchors/custom-ca.crt
$ update-ca-trust

## 데몬 리로드
$ systemctl restart containerd
$ systemctl restart kubelet

## Host File 업데이트
$ echo $(k get svc/private-registry-svc -ojson | jq .spec.clusterIP -r)" private-registry-svc.default.svc.cluster.local" >> /etc/hosts

# 다른 노드가 있을 경우
## Root CA 추가
$ scp /etc/pki/ca-trust/source/anchors/custom-ca.crt node01:/etc/pki/ca-trust/source/anchors/
$ ssh node01 "update-ca-trust"

## 데몬 리로드
$ ssh node01 "systemctl restart containerd"
$ ssh node01 "systemctl restart kubelet"

## Host File 업데이트
$ CUSTOMHOST=$(cat /etc/hosts | grep private-registry)
$ ssh node01 "echo ${CUSTOMHOST} >> /etc/hosts"
```


## MERGED
```
## Root CA 추가
apt-get install -y ca-certificates
kubectl get secret private-registry-tls-cert -ojson |jq .data.\"ca.crt\" -r | base64 -d >> /usr/local/share/ca-certificates/custom-ca.crt
update-ca-certificates

## 데몬 리로드
systemctl restart containerd
systemctl restart kubelet

## Host File 업데이트
echo $(k get svc/private-registry-svc -ojson | jq .spec.clusterIP -r)" private-registry-svc.default.svc.cluster.local" >> /etc/hosts

## 다른 노드가 있을 경우
## Root CA 추가
scp /usr/local/share/ca-certificates/custom-ca.crt node01:/usr/local/share/ca-certificates/
ssh node01 "update-ca-certificates"

## 데몬 리로드
ssh node01 "systemctl restart containerd"
ssh node01 "systemctl restart kubelet"

## Host File 업데이트
CUSTOMHOST=$(cat /etc/hosts | grep private-registry)
ssh node01 "echo ${CUSTOMHOST} >> /etc/hosts"
```
