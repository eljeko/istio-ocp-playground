## Install the charts

This chart contains the buildconfig, imagestream

    cd <PATH_TO>/simple-java-helm-chart/java-build-chart
    helm install java-build-chart java-build-chart -n beservices

This chart contains the deployment, service, and **istio** manifests

    cd <PATH_TO>/simple-java-helm-chart/java-build-chart
    helm install java-simple-chart java-simple-chart -n beservices

### Istio configuration

The istio setup consists of:

1. An [AuthorizationPolicy](../../main/setup-beservices/simple-java-helm-chart/java-simple-chart/templates/istio/allow-nothing.yaml) with ```allow-nothing```
2. An [AuthorizationPolicy](../../main/setup-beservices/simple-java-helm-chart/java-simple-chart/templates/istio/authorization-caller.yaml) that allow only a specific service account from ```frontend``` project to call the service in ```beservice``` project and allow all call from ```beservice``` project
3. A [PeerAuthentication](../../main/setup-beservices/simple-java-helm-chart/java-simple-chart/templates/istio/istio-peer-auth.yaml) with ```mtls mode STRICT```

## Build and deploy the ReST app

    cd <PATH_TO>/simple-java-helm-chart/springboot-service

build the app:

    mvn clean package spring-boot:repackage

Start the oc build:
  
    oc start-build springboot-app-build --from-file target/bookstore-books-rest-api-1.0.jar -n beservices
