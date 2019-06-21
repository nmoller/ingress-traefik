# Installer treafik chart

Comme d'habitude.. j'utilise helm comme moteur de template sans tiller...

```bash
# chercher la chart
helm fetch stable/traefik --untar
# au même niveau où la chart a été untar.
cp traefik/values.yaml .
# modifier le fichier values.yaml
helm template traefik -f values.yaml
```

Le nom du release est très sensible lors de la validation. Ce qui a fonctionné:

```bash
helm template traefik -f values.yaml --name tr0-0-1 \
 |tee traefik-0.0.1.yaml

kubectl -n kube-system create -f traefik-0.0.1.yaml
```

Le `ingress` doit être dans le ns `kube-system`. Pour voir fonctionnement, on
utilise les manifests de <https://docs.traefik.io/user-guide/kubernetes/>

```bash
kubectl apply -n siad-moodle-dev-01 -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/cheese-services.yaml
service "stilton" created
service "cheddar" created
service "wensleydale" created

kubectl apply -n siad-moodle-dev-01 -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/cheese-deployments.yaml

deployment "stilton" created
deployment "cheddar" created
deployment "wensleydale" created

# Modifier pour utiliser [name].nmoller.io à la place de [name].minikube
kubectl apply -n siad-moodle-dev-01 -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/cheese-ingress.yaml
ingress "cheese" created
```
