Step Prepare
###################################################################
https://github.com/chatapazar/backend_quarkus

remove limit range of ci-cd project before run setup_ci_cd_tool.sh
nexus set repositories, maven-releases --> deployment policy to allow redeploy

oc project dev
oc create secret docker-registry nexus-images \
    --docker-server=nexus-registry-ci-cd.apps.cluster-550d.550d.sandbox1520.opentlc.com \
    --docker-username=admin \
    --docker-password=admin

oc secrets link default nexus-images --for=pull

Step Demo
###################################################################
git url: https://github.com/chatapazar/backend_quarkus.git

run build


tekton
https://github.com/siamaksade/tekton-cd-demo
remove limitrange of project before run