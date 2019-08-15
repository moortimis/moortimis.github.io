# Portworx Sandboix Installation

## Setup and install etcd-cluster

```bash
oc new-project portworx-sandbox

export ROLE_NAME=etcd-operator
export ROLE_BINDING_NAME=etcd-operator
export NAMESPACE=portworx-sandbox

curl https://raw.githubusercontent.com/coreos/etcd-operator/master/example/rbac/cluster-role-template.yaml | sed  -e "s/<ROLE_NAME>/${ROLE_NAME}/g" | oc apply -f -

curl https://raw.githubusercontent.com/coreos/etcd-operator/master/example/rbac/cluster-role-binding-template.yaml | \
  sed  -e "s/<ROLE_NAME>/${ROLE_NAME}/g" \
  -e "s/<ROLE_BINDING_NAME>/${ROLE_BINDING_NAME}/g" \
  -e "s/<NAMESPACE>/${NAMESPACE}/g" \
  | oc apply -f -

oc describe clusterrolebinding etcd-operator

oc create -f https://raw.githubusercontent.com/coreos/etcd-operator/master/example/deployment.yaml

cat << EOF >> etc-cluster2.yml
apiVersion: "etcd.database.coreos.com/v1beta2"
kind: "EtcdCluster"
metadata:
  name: "portworx-sandbox-etcd-cluster"
spec:
  size: 3
  version: "3.2.13"
EOF

oc apply -f etcd-cluster.yml

oc get svc -l etcd_cluster=portworx-sandbox-etcd-cluster
oc run --rm -i --tty fun --image quay.io/coreos/etcd --restart=Never -- /bin/sh
ETCDCTL_API=3 etcdctl --endpoints http://portworx-sandbox-etcd-cluster-client:2379 put foo bar
ETCDCTL_API=3 etcdctl --endpoints http://portworx-sandbox-etcd-cluster-client:2379 get foo

```

```bash
export NAMESPACE=portworx-sandbox

oc adm policy add-scc-to-user privileged system:serviceaccount:${NAMESPACE}:px-account
oc adm policy add-scc-to-user privileged system:serviceaccount:${NAMESPACE}:portworx-pvc-controller-account
oc adm policy add-scc-to-user anyuid system:serviceaccount:default:default

docker login -u '6646581|a2f15ab8f147c988b5d3d5d0e76a0a4f-msd-poc' -p eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiI3YmRhZGNjMDYxZTg0NTZlYjk0NzMwNGY3MDZiOGRmNCJ9.whgXge-aqTs3bP8vtpMIxcfKyZk0EDRSOY74PbVfzvbKKw6gc_rgbLwHz5Tv3Zz80i8Jg-p0g3FsvnIgWt8VNyX2kwkM_riqvAx9WQP2FuDgcwfZdbQxjRaqfYYpo4kZgtbbjXbX755x4Q9UibTwBnJVEQPoxE0SMtYEhTuDMLOiHbAqYVF4a3qAxnJxV4gySCZ-T8meNS0fDYbjKNwdJwGy9e4Z5gOWqUEGUq63zMSjKKEoUtmor1vNQdaSJrbeWsVGmwfaiJAoz46sQBtJDv--7DpaUORkG5db0QmC1TZZ1ufSTL8ui8EmlTh1GIxHDDteVpDWkFyvPBZOFxPFO5UW4UvAkUWma6wMSW9u2juSH0d7b-UpAY6qO3-iBJydtbUwf25z1suMBheIhFUf7ExUCsgYDgWDuj6oo4LsVUt1FPJ8vJ_aE_0Slkxojeg57BLPvsJ0huDN1PT7AoeQ4sVHa4wgX21S77rfKmoSmByf-lt_lkeN1OPCqzwUJrtS6Fb1Y-CoisTrRVioa4R8Ab3LHbADfjqwlhLlsagwFdPEEKhaYiNfQfFu14ZNCG1YCV6j0zt3VI8yovPqaJdp6T-mB6nm6SRThaanZk3cphXmQbnT8W3e3j0rlabQ5aUzQTN8ge0hO_8JEuN7GtW-XOEPQPJLGr4xds4h3AW30YY registry.redhat.io

oc create secret docker-registry regcred \
         --docker-server=registry.redhat.io \
         --docker-username='6646581|a2f15ab8f147c988b5d3d5d0e76a0a4f-msd-poc' \
         --docker-password='eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiI3YmRhZGNjMDYxZTg0NTZlYjk0NzMwNGY3MDZiOGRmNCJ9.whgXge-aqTs3bP8vtpMIxcfKyZk0EDRSOY74PbVfzvbKKw6gc_rgbLwHz5Tv3Zz80i8Jg-p0g3FsvnIgWt8VNyX2kwkM_riqvAx9WQP2FuDgcwfZdbQxjRaqfYYpo4kZgtbbjXbX755x4Q9UibTwBnJVEQPoxE0SMtYEhTuDMLOiHbAqYVF4a3qAxnJxV4gySCZ-T8meNS0fDYbjKNwdJwGy9e4Z5gOWqUEGUq63zMSjKKEoUtmor1vNQdaSJrbeWsVGmwfaiJAoz46sQBtJDv--7DpaUORkG5db0QmC1TZZ1ufSTL8ui8EmlTh1GIxHDDteVpDWkFyvPBZOFxPFO5UW4UvAkUWma6wMSW9u2juSH0d7b-UpAY6qO3-iBJydtbUwf25z1suMBheIhFUf7ExUCsgYDgWDuj6oo4LsVUt1FPJ8vJ_aE_0Slkxojeg57BLPvsJ0huDN1PT7AoeQ4sVHa4wgX21S77rfKmoSmByf-lt_lkeN1OPCqzwUJrtS6Fb1Y-CoisTrRVioa4R8Ab3LHbADfjqwlhLlsagwFdPEEKhaYiNfQfFu14ZNCG1YCV6j0zt3VI8yovPqaJdp6T-mB6nm6SRThaanZk3cphXmQbnT8W3e3j0rlabQ5aUzQTN8ge0hO_8JEuN7GtW-XOEPQPJLGr4xds4h3AW30YY' \
         --docker-email=tim.moor2@datacom.co.nz \
         -n ${NAMESPACE}

kubectl label nodes infr01.dev.openshift.local infr02.dev.openshift.local node01.dev.openshift.local px/metadata-node=true

```
