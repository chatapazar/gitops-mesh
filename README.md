https://github.com/rhthsa/openshift-demo/blob/main/openshift-service-mesh.md

cd openshift-demo
oc new-project istio-system
oc create -f manifests/smcp.yaml -n istio-system

check control plane
bin/get-smcp-status.sh istio-system

oc new-project project1 
oc describe smmr/default -n istio-system | grep -A2 Spec:

SUBDOMAIN=$(oc whoami --show-console|awk -F'apps.' '{print $2}')
echo $SUBDOMAIN
oc apply -f manifests/smmr.yaml -n istio-system
---> in git

oc apply -f manifests/frontend.yaml
oc apply -f manifests/frontend-service.yaml
oc apply -f manifests/frontend-gateway.yaml
oc apply -f manifests/frontend-destination-rule.yaml
oc apply -f manifests/frontend-virtual-service.yaml
oc apply -f manifests/backend.yaml
oc apply -f manifests/backend-destination-rule.yaml
oc apply -f manifests/backend-virtual-service-v1-v2-50-50.yaml

--> test

FRONTEND_ISTIO_ROUTE=$(oc get route -n istio-system|grep istio-system-frontend-gateway |awk '{print $2}')
while [ 1 ];
do
        OUTPUT=$(curl -s $FRONTEND_ISTIO_ROUTE)
        printf "%s\n" $OUTPUT
        sleep .2
done

show kiali --> project1, topology
Namespace: select checkbox "project1"
Display: select checkbox "Requests percentage" and "Traffic animation"
applications, frontend, traces, view in tracting
istio-system project, grafana, manage, control plane, service dashboard


gitops
--> URL: https://openshift-gitops-server-openshift-gitops.apps.cluster-270c.270c.sandbox140.opentlc.com
--> admin password: BgEMLkcm2i4Fpz7GtUArTwW13saPKdjf