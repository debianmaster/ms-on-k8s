```
oc get nodes
open http://store-store.mk.ck.osecloud.com
open https://studio.apicur.io

rm -rf ~/reccomendations-api
swagger-codegen generate -i ~/Downloads/reccomendations.yml -l spring ~/reccomendations-api -c ~/swagger-config.json

cd reccomendations-api
mvn io.fabric8:fabric8-maven-plugin:LATEST:setup
tail -n 30 pom.xml
mvn fabric8:deploy
mvn fabric8:debug

ext install vscode-java-debug
oc policy add-role-to-user edit system:serviceaccount:bi-qa:jenkins -n bi-dev
```
