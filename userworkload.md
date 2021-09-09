


oc exec -n project2 $(oc get pods -n project2 | grep backend | head -n 1 | awk '{print $1}') \
-- curl -s  http://localhost:8080/q/metrics

oc exec -n project2 $(oc get pods -n project2 | grep backend | head -n 1 | awk '{print $1}') \
-- curl -s http://localhost:8080/q/metrics/application


FRONTEND_URL=https://$(oc get route frontend -n project2 -o jsonpath='{.spec.host}')
while [ 1 ];
do
  curl $FRONTEND_URL
  printf "\n"
  sleep .2
done


rate(application_com_example_quarkus_BackendResource_countBackend_total[1m])

grafana admin/openshift


