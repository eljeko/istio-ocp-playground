# Install helm chart

Install the frontend app AFTER the beservices

    helm install frontend-client-chart frontend-client-chart

**Note:** `Host: *.foo.svc.cluster.local` configured in the file 
limits the matches to services in foo namespace only.

The you can test the setup:


This call should fail:

    oc exec $(oc get pod -l app=anyclient -n frontend -o jsonpath={.items..metadata.name}) -c anyclient -n frontend -- curl http://springboot-app.beservices:8080/books -s -o /dev/null --connect-timeout 2 -w "%{http_code}\n"

This call should work:

    oc exec $(oc get pod -l app=frontendclient -n frontend -o jsonpath={.items..metadata.name}) -c frontendclient -n frontend -- curl http://springboot-app.beservices:8080/books -s -o /dev/null --connect-timeout 2 -w "%{http_code}\n"

for from in "foo" "bar" "legacy"; do for to in "foo" "bar" "legacy"; do oc exec $(oc get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" --connect-timeout 2 -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done


