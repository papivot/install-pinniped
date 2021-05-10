# Install Pinniped Supervisor and configure Concierge for authentication

Supervisor cluster creation -> 

# Setup Supervisor Federation
`$ kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-supervisor.yaml`

```
$ kubectl create secret tls fed-tls-cert-secret --cert=tls.cert --key=tls.key -n pinniped-supervisor`
$ kubectl create secret tls pinniped-supervisor-default-tls-certificate --cert=tls.cert --key=tls.key -n pinniped-supervisor
```

`$ kubectl create -f 01-pinniped-sup-svc.yaml`

`$ kubectl create -f 02-pinniped-sup-fed.yaml`

# Setup OIDC IDP
`$ kubectl create -f 03-pinniped-sup-oidc-secret.yaml`

```
$ kubectl create -f 04-pinniped-sup-oidc.yaml
$ kubectl describe OIDCIdentityProvider -n pinniped-supervisor okta
```

Conceirge setup 

#
`audience="$(openssl rand -hex 8)"`

`kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-concierge.yaml`

`kubectl apply -f pinniped-con.yaml`

Login

#

`pinniped get kubeconfig > /tmp/pinniped-kubeconfig`

`kubectl --kubeconfig /tmp/pinniped-kubeconfig1 get pods -A`

`kubectl create clusterrolebinding nv-can-read --clusterrole view --user navneetv@vmware.com`

`kubectl --kubeconfig /tmp/pinniped-kubeconfig1 get pods -A`