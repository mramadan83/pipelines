# for spring boot
- create the pvc(s):
- oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/pvc/shared-pvc.yaml
- oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/pvc/infra-pvc.yaml
- create the config map which contains the settings.xml for maven
- oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/pipelines/springboot/config.yaml
- create the secrte that will be used to push to quay:
- oc create secret docker-registry quay-credentials \
  --docker-server=quay.io \
  --docker-username=your-quay-username \
  --docker-password=your-quay-password \
  --docker-email=you@example.com \
  -n your-namespace
- link the secret to  the pipeline service account and default service account (used by deployments)
- oc secrets link pipeline quay-credentials --for=pull,mount
- oc secrets link default quay-credentials --for=pull,mount
- create the pipeline
- oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/pipelines/springboot/pipline.yaml
- all the tasks are ClusterTasks, so no need to create tasks till now

- sample curl
- curl https://spring-boot-hello-world-pipelines.apps.l1ibqh3c.eastus.aroapp.io/hello

- #################################################
if your namespace can't use the cluster tasks
check first
- oc auth can-i use clustertasks/git-clone --as=system:serviceaccount:<your-namespace>:pipeline
if, no do the followoing
- oc apply -f 
