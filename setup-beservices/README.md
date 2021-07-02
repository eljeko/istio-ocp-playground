## Install the charts

    helm install java-simple-chart java-simple-chart
    helm install java-build-chart java-build-chart

## Build and deploy the ReST app

    cd <PATH_TO>/simple-java-helm-chart/springboot-service

build the app:

    mvn clean package spring-boot:repackage

Start the oc build:
  
    oc start-build springboot-app-build --from-file target/bookstore-books-rest-api-1.0.jar
