# Building the container

- Build demo container


```
podman build -t bastion.example.com:5000/poc/net-toolbox -f Dockerfile.ubi

for a build behind a proxy use

export HTTPS_PROXY=http://10.20.30.40:80
podman build -t net-toolbox:latest --no-cache --build-arg=HTTPS_PROXY=http://10.20.30.40:80 -f Dockerfile.ubi.proxy
unset HTTPS_PROXY

or for a Fedora build

podman build -t bastion.example.com:5000/poc/net-toolbox -f Dockerfile.fedora


```

# Testing Multus in OCP4.1

- Create project for CNI configurations
```
oc new-project multinetwork-configs
```

- Update and create a NetworkAttachmentDefinition. Some examples are here:
  - [multus-macvlan-config-local.yaml](multus-macvlan-config-local.yaml)
  - [multus-macvlan-config-external-dhcp.yaml](multus-macvlan-config-external-dhcp.yaml)
  - [multus-macvlan-config-route-override.yaml](multus-macvlan-config-route-override.yaml)

```
oc create -f multus-macvlan-conf.yaml
```

```
oc get network-attachment-definitions.k8s.cni.cncf.io

oc edit network-attachment-definitions.k8s.cni.cncf.io macvlan-conf
```

```
oc create -f multus-pod-demo.yaml
```
