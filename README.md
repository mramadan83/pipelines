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

  ###############################################

  to clone private repository with Authentication
  - create personal access token on git
  - create secret generic github-pat-secret \
    --type=kubernetes.io/basic-auth \
    --from-literal=username=$GITHUB_USERNAME \
    --from-literal=password=$TEKTON_TUTORIAL_GITHUB_PAT
  - oc annotate secret github-pat-secret "tekton.dev/git-0=https://github.com"
  - oc patch serviceaccount github-bot -p '{"secrets": [{"name": "github-pat-secret"}]}'
 
  - you can check results by
  - oc get sa pipeline -o yaml


- #################################################
if your namespace can't use the cluster tasks
check first
- oc auth can-i use clustertasks/git-clone --as=system:serviceaccount:<your-namespace>:pipeline
if, no do the followoing
- in the pipelines -> tasks make sure that the cluster tasks is installed

  ############################################

  call the pipeline from the postman and webhook
  - oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/triggers/trigger-template.yaml
  - oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/triggers/trigger-binding.yaml
  - oc apply -f https://raw.githubusercontent.com/mramadan83/pipelines/refs/heads/main/triggers/event-listner.yaml
  - oc get services
  - oc expose service/<event-listner-created-service-name>
  now a deployment for the event listner should be created and pod is running
  also a service and route for the event listner should be created
#-------------------
call the pipeline from posman
- create request POST
- add the route url in the url
- add header "Content-Type" with value "application/json"
- sample body raw ---> json

{
  "repository": {
    "clone_url": "https://github.com/mramadan83/spring-boot-hello-world.git",
    "name": "spring-boot-hello-world"
  },
  "ref": "main",
  "namespace": "poc"  // Replace with your desired namespace
}

    ##########
    some investigations for the trigering
    - oc get eventlisteners
    - oc describe route <route-name>
    - oc describe service <service-name>
    - oc get endpoints <service-name>
    - oc get replicasets
    - oc describe replicaset <replica-set>
