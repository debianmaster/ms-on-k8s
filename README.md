oc get nodes
open http://store-store.mk.ck.osecloud.com
open https://studio.apicur.io

swagger-codegen generate -i ~/Downloads/reccomendations.yaml -l spring ~/reccomendations-api -c ~/swagger-config.json

cd spring-start-sample
mvn io.fabric8:fabric8-maven-plugin:LATEST:setup
tail -n 30 pom.xml
mvn clean install
mvn fabric8:deploy
mvn fabric8:run
mvn fabric8:debug

ext install vscode-java-debug
oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n bi-dev
oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n bi-qa

