Step Prepare
###################################################################
https://github.com/rhthsa/openshift-demo/blob/main/openshift-service-mesh.md
add worker node to 3
install operator 
- ElasticSearch
- Jaeger
- Kiali
- OpenShift Service Mesh
- OpenShift GitOps Operator

cd openshift-demo
oc new-project istio-system
oc create -f manifests/smcp.yaml -n istio-system

check control plane
bin/get-smcp-status.sh istio-system

oc new-project project1 
oc create -f manifests/smmr.yaml -n istio-system
oc describe smmr/default -n istio-system | grep -A2 Spec:

SUBDOMAIN=$(oc whoami --show-console|awk -F'apps.' '{print $2}')
echo $SUBDOMAIN
---> in git --> gitops-mesh
cd gitops-mesh
base/frontend-gateway.yaml  --> change domain
base/frontend-virtual-service.yaml --> change domain
--> commit to git


oc get pods -n openshift-gitops



Step Demo
###################################################################
gitops

ARGOCD=$(oc get route/openshift-gitops-server -n openshift-gitops -o jsonpath='{.spec.host}')
echo https://$ARGOCD


PASSWORD=$(oc extract secret/openshift-gitops-cluster -n openshift-gitops --to=-) 2>/dev/null
echo $PASSWORD

create manual application
- application name: demo-mesh
- project: default
- sync policy: manual
- source: https://github.com/chatapazar/gitops-mesh.git
- path: manifests/apps-kustomize/overlays/dev
- cluster: default
- namespace: project1
- kustomize: leave default
- create

- sync

--> test

FRONTEND_ISTIO_ROUTE=$(oc get route -n istio-system|grep project1-frontend-gateway |awk '{print $2}')
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

remark: if you have problem about gateway, you can restart pod 'istiod'