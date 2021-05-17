# Install Pinniped Supervisor and configure Concierge for authentication

### Setup 

Clone this repo and modify the `config.param.template` file as per your enviornment. Save the file as `config.param`. For the Supervisor issuer, make sure the tls.key and tls.cert are present in the current folder. You may need to generate the certs if not using a public cert. 

`$ source config.param`


## Supervisor setup

### Setup Supervisor Federation

Install the Supervisor controller software on the management cluster 
`$ kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-supervisor.yaml`

Expose the Pinniped Federation Domain using service type loadbalancer. COnfigure TLS secrets for SSL traffic. If using Ingress, the certificate needs to be applied at the Ingress layer. 
```
$ kubectl create secret tls fed-tls-cert-secret --cert=tls.cert --key=tls.key -n pinniped-supervisor
$ kubectl create secret tls pinniped-supervisor-default-tls-certificate --cert=tls.cert --key=tls.key -n pinniped-supervisor
```

`$ kubectl create -f 01-pinniped-sup-svc.yaml`

`$ envsubst < 02-pinniped-sup-fed.yaml | kubectl create -f-`

### Setup OIDC IDP
Create the OIDC IDP authentication secret.
`$ envsubst < 03-pinniped-sup-oidc-secret.yaml | kubectl create -f-`

Create the OIDC IDP for the Supervisor
```
$ envsubst <  04-pinniped-sup-oidc.yaml | kubectl create -f-
$ kubectl describe OIDCIdentityProvider -n pinniped-supervisor okta
```

## Concierge setup 

Install the concierge  software on the management cluster 
`$ kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-concierge.yaml`

Create the JWTAuthenticator to get tokens from the Supervisor
`$ envsubst < pinniped-con.yaml | kubectl apply -f- `


## Login using Pinniped client

`$ pinniped get kubeconfig > /tmp/pinniped-kubeconfig`

`$ kubectl --kubeconfig /tmp/pinniped-kubeconfig get pods -A`

`$ kubectl create clusterrolebinding nv-can-read --clusterrole view --user navneetv@vmware.com`

`$ kubectl --kubeconfig /tmp/pinniped-kubeconfig get pods -A`
