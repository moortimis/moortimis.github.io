---
title: "SSL Debugging on OpenShift"
date: 2018-01-14T08:12:32+13:00
draft: true
---

#
openssl verify -CAfile /etc/origin/master/ca.crt /etc/origin/master/master.server.crt
openssl verify -CAfile /etc/origin/master/ca-bundle.crt /etc/origin/master/master.server.crt
/etc/origin/master/master.server.crt: OK

grep certificate-authority-data /etc/origin/master/admin.kubeconfig | awk '{ print $2 }' | base64 -d | md5sum
grep certificate-authority-data /${USER}/.kube/config | awk '{ print $2 }' | base64 -d | md5sum
md5sum /etc/origin/master/ca-bundle.crt

# grep certificate-authority-data /etc/origin/master/admin.kubeconfig | awk '{ print $2 }' | base64 -d | md5sum
fb33bfae97375e964fc5c75aec16a894  -
# grep certificate-authority-data /root/.kube/config | awk '{ print $2 }' | base64 -d | md5sum
fb33bfae97375e964fc5c75aec16a894  -
# md5sum /etc/origin/master/ca-bundle.crt
fb33bfae97375e964fc5c75aec16a894  /etc/origin/master/ca-bundle.crt

grep certificate-authority-data /etc/origin/master/admin.kubeconfig | awk '{ print $2 }' | base64 -d | openssl x509 -noout -text
diff /${USER}/.kube/config /etc/origin/master/admin.kubeconfig 

openssl s_client -CAfile /etc/origin/master/ca-bundle.crt -connect oc-master.domain.com:8443
oc status --config=/etc/origin/master/admin.kubeconfig
