# Install helm chart

Install the frontend app AFTER installing [beservices](../../main/setup-beservices/)

    helm install frontend-client-chart frontend-client-chart -n frontend

## Testing the setup

The setup allows only the ```frontendclient service account``` from ```frontend``` project to call the springboot-app services in the ```beservices``` project.

This call fails:

    oc exec $(oc get pod -l app=anyclient -n frontend -o jsonpath={.items..metadata.name}) -c anyclient -n frontend -- curl http://springboot-app.beservices:8080/books -s -o /dev/null --connect-timeout 2 -w "%{http_code}\n"

The call cames from a pod service account that is **not allowed** and returns 403

This call works:

    oc exec $(oc get pod -l app=frontendclient -n frontend -o jsonpath={.items..metadata.name}) -c frontendclient -n frontend -- curl http://springboot-app.beservices:8080/books -s -o /dev/null --connect-timeout 2 -w "%{http_code}\n"

The call cames from a pod service account that is **allowed** and returns 200