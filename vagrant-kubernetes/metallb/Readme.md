# How to install metallb

---

## [Metallb 공식페이지](https://metallb.universe.tf/)

Metallb는 베어메탈에서 로드밸런서를 쓸 수 있도록 도와주는 서비스이다.

cncf 공식으로 채용된 로드밸런서는 porter이지만 개인적으로는 metallb가 좋다고 생각한다.



## Step1. 네임스페이스 & Metallb 설치

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml

kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```



## Step2. ConfigMap 생성

```bash
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 172.16.20.220-172.16.20.250
```

- 중요한건 아래 주소범위는 virtualbox 설정일 때 적용할 수 있는 범위이다.
- 내가 K8S 노드 IP 범위를 어떻게 잡았느냐에 따라서 설정을 바꿔줘야 한다.