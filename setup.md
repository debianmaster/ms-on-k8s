```sh
oc project store
istioctl kube-inject -f <(oc new-app debianmaster/store-fe --name=store -l app=store,version=v1 --dry-run -o yaml) | oc apply -f-
istioctl kube-inject -f <(oc new-app debianmaster/store-products:v1 --name=products -l app=products,version=v1 --dry-run -o yaml) | oc apply -f-




oc new-app mongodb -l app=mongodb --name=productsdb \
  -e MONGODB_ADMIN_PASSWORD=password  -e MONGODB_USER=app_user \
  -e MONGODB_DATABASE=store  -e MONGODB_PASSWORD=password

oc env dc products MONGO_USER=app_user MONGO_PASSWORD=password MONGO_SERVER=productsdb MONGO_PORT=27017 MONGO_DB=store \
mongo_url='mongodb://app_user:password@productsdb/store'


oc rsh mongodbcontainer
use store
db.auth('app_user','password')

var categories = [
		{id:1,CatName:"Wireless",SubCats:[{id:1,name:"RF"},{id:2,name:"XBEE"},{id:3,name:"Wifi"},{id:4,name:"Bluetooth"}]},
		{id:2,CatName:"Development Boards",SubCats:[{id:5,name:"Arduino"},{id:6,name:"ARM"},{id:7,name:"8051"},{id:8,name:"AVR"}]},
		{id:3,CatName:"Sensors",SubCats:[{id:9,name:"Ultrasonic"},{id:10,name:"GPS"},{id:11,name:"IR"},{id:12,name:"Light"}]}
	];	

var products=[ {product_id:'cable_1',name:"Wire",title:"Wire",img:'img/storeImages/08566-01-L_l_th.jpg',images:['storeImages/08566-01-L_l_th.jpg'],
        documents:"Wire",features:"Wire",shipping:55,caption:"Cable Wire",price:20,subCat:1}]

db.Categories.insert(categories)

db.Products.insert(products)

oc delete svc products 
oc create service clusterip products --tcp=8080:8080


istioctl kube-inject -f <(oc new-app debianmaster/store-recommendations:v1 --name=recommendations -l app=recommendations,version=v1 --dry-run -o yaml -n bi) | oc apply -n bi -f-

oc env dc products MONGO_USER=app_user MONGO_PASSWORD=password MONGO_SERVER=productsdb MONGO_PORT=27017 MONGO_DB=store \
mongo_url='mongodb://app_user:password@productsdb/store'


  593  oc new-project infra
  594  kubectl apply -f <(istioctl kube-inject -f <(kubectl apply -f http://central.maven.org/maven2/io/fabric8/devops/apps/keycloak/2.2.327/keycloak-2.2.327-kubernetes.yml --dry-run -o yaml))
  595  oc create service clusterip products --tcp=8080:8080
  596  oc expose deploy keycloak --port=8080

```
