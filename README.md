```
oc get nodes
open http://store-store.mk.ck.osecloud.com
open https://studio.apicur.io

rm -rf ~/reccomendations-api
swagger-codegen generate -i ~/Downloads/reccomendations.yml -l spring ~/reccomendations-api --artifact-id recommendations -c ~/swagger-config.json

cd reccomendations-api
mvn clean package
mvn spring-boot:run

mvn io.fabric8:fabric8-maven-plugin:LATEST:setup
tail -n 30 pom.xml
mvn fabric8:deploy
mvn fabric8:debug

ext install vscode-java-debug
oc policy add-role-to-user edit system:serviceaccount:bi-qa:jenkins -n bi-dev
```

```
open https://github.com/debianmaster/nodejs-welcome/blob/ci-cd/devpipeline.yaml
```



```




kubectl apply -f <(istioctl kube-inject -f <(kubectl run --image=debianmaster/go-welcome cars-api -l app=cars-api,version=v1  -o yaml --dry-run))

kubectl apply -f <(istioctl kube-inject -f <(kubectl run --image=debianmaster/nodejs-welcome cars-web -l app=cars-web,version=v1  -o yaml --dry-run))

kubectl apply -f <(istioctl kube-inject -f <(kubectl apply -f http://central.maven.org/maven2/io/fabric8/devops/apps/keycloak/2.2.327/keycloak-2.2.327-kubernetes.yml --dry-run -o yaml))

kubectl apply -f <(istioctl kube-inject -f <(kubectl run --image=raesene/alpine-nettools tools -l app=tools,version=v1  -o yaml --dry-run))





apiVersion: v1
kind: Service
metadata:
  name: keycloak
  labels:
    app: keycloak
spec:
  ports:
  - port: 8080
    name: http
  selector:
    app: keycloak


apiVersion: v1
kind: Service
metadata:
  name: cars-api
  labels:
    app: cars-api
spec:
  ports:
  - port: 8000
    name: http
  selector:
    app: cars-api    


apiVersion: v1
kind: Service
metadata:
  name: cars-web
  labels:
    app: cars-api
spec:
  ports:
  - port: 8080
    name: http
  selector:
    app: cars-web  


/usr/local/bin/oc cluster up --public-hostname=mk.ck.osecloud.com --routing-suffix='mk.ck.osecloud.com'

oc login -u system:admin
oc adm policy add-cluster-role-to-user cluster-admin developer
oc adm policy add-scc-to-user anyuid -z istio-ingress-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z default -n istio-system
oc adm policy add-scc-to-user anyuid -z grafana -n istio-system
oc adm policy add-scc-to-user anyuid -z prometheus -n istio-system
oc create -f install/kubernetes/istio.yaml
oc project istio-system
oc expose svc istio-ingress
oc apply -f install/kubernetes/addons/prometheus.yaml
oc apply -f install/kubernetes/addons/grafana.yaml
oc apply -f install/kubernetes/addons/servicegraph.yaml
oc expose svc servicegraph
oc expose svc grafana
oc expose svc prometheus

oc adm policy add-scc-to-user privileged -z default
oc adm policy add-scc-to-user anyuid -z default
oc adm policy add-cluster-role-to-user cluster-admin admin


oc adm policy add-scc-to-user privileged -z default
oc adm policy add-scc-to-user anyuid -z default


export token=$(curl -s -X POST 'http://keycloak:8080/auth/realms/istio/protocol/openid-connect/token' -H "Content-Type: application/x-www-form-urlencoded" -d 'username=demo&password=test&grant_type=password&client_id=cars-web' | jq -r .access_token)


curl -vvv -H "Authorization: Bearer ${token}" http://cars-api:8000
```


