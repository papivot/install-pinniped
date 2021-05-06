# Install Pinniped Supervisor and configure Concierge for authentication

Supervisor cluster creation -> 

# Setup Supervisor Federation
`kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-supervisor.yaml`

`kubectl create secret tls my-tls-cert-secret --cert=tls.cert --key=tls.key -n pinniped-supervisor`

`kubectl create secret tls pinniped-supervisor-default-tls-certificate --cert=tls.cert --key=tls.key -n pinniped-supervisor`

`kubectl create -f pinniped-sup-svc.yaml`

`kubectl create -f pinniped-sup-fed.yaml`

# Setup OIDC IDP
`kubectl create -f pinniped-sup-oidc-secret.yaml`

`kubectl create -f pinniped-sup-oidc.yaml`

Conceirge setup 

#
`audience="$(openssl rand -hex 8)"`
`kubectl apply -f https://get.pinniped.dev/latest/install-pinniped-concierge.yaml`
