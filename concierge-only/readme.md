kubectl apply -f concierge.yaml

pinniped get kubeconfig \                                                                                                                                                  ─╯
  --oidc-client-id sdfsdfsfsfsdfsdfsdf \
  --oidc-scopes openid,email \
  --oidc-listen-port 12345 \
  > my-cluster.yaml

kubectl --kubeconfig my-cluster.yaml get namespaces
