
# OpenShift command cheat sheet

This is command cheat sheet for OpenShift.

## Create an app

```
oc new-app $GIT_URL
```

## List project

```
oc get project
```

## View all in project

```
oc get all
```

## Delete all in project

```
oc delete all --all
```

## List pod

```
oc get pod
```

## List pod with node

```
oc get pod -o wide
```

## List pod names (for scripting)

```
oc get pod -o name
```

## Get specific item using Go template (for scripting)

```
oc get dc docker-registry --template='{{range .spec.template.spec.containers}}{{.image}}{{end}}'
oc get service docker-registry --template='{{.spec.clusterIP}}'
oc get pod docker-registry-2-xxx --template='{{.status.podIP}}'
```

## Get pod log

```
oc logs $POD_NAME --timestamps
```

## Get CrashLoop pod log

```
oc logs -p $POD_NAME --timestamps
```

## rsh into pod

```
oc rsh $POD_NAME
```

## exec single command in pod

```
oc exec $POD_NAME $COMMAND
```

## Read resource schema doc

```
oc explain dc
oc explain dc.spec
```

## Export

```
oc export is or bc or dc or svc --as-template=app.yaml
```

## Create build from local Dockerfile and deploy

```
cat Dockerfile | oc new-build --dockerfile=- --to=$APP_NAME
oc new-app $APP_NAME
```

## Create build from local dir with Dockerfile and deploy

```
oc new-build --strategy=docker --binary=true --name=$APP_NAME
oc start-build $APP_NAME --from-dir=.
oc new-app $APP_NAME
```

## Allow access to OpenShift API from default application service account

```
oc policy adm add-role-to-user view -z default
```

## Debug

```
oc debug dc $DC_NAME
oc debug dc $DC_NAME --as-root=true
oc debug dc $DC_NAME --node-name=$NODENAME
```

## Mount PVC used by other pod for maintenance

```
oc run sleep --image=registry.access.redhat.com/rhel7 -- tail -f /dev/null
oc volume dc/sleep --add -t pvc --name=test --claim-name=test --mount-path=/test
oc rsh sleep-X-XXXXX
oc delete all -l app=sleep
```

## Get metrics

```
oc adm top pod --heapster-namespace=openshift-infra --heapster-scheme=https
```


## Claim PersistentVolume

```
oc new-app sonatype/nexus
oc volume dc nexus --remove --confirm
oc volume dc nexus --add --name=nexus-storage -t pvc --claim-name=nexus --claim-mode=ReadWriteMany --claim-size=1Gi --mount-path=/sonatype-work
```

## Define resource requests and limits in DeploymentConfig

```
oc set resources deployment nginx --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi
```

## Define livenessProve and readinessProve in DeploymentConfig

```
oc set probe dc/nginx --readiness --get-url=http://:8080/healthz --initial-delay-seconds=10
oc set probe dc/nginx --liveness --get-url=http://:8080/healthz --initial-delay-seconds=10
```

## Define Horizontal Pod Autoscaler (hpa)

```
oc autoscale dc $DC_NAME --max=4 --cpu-percent=10
```

## Define nodeSelector in DeploymentConfig

```
oc patch dc $DC_NAME -p "spec:
  template:
    spec:
      nodeSelector:
        region: infra"
```

## Define nodeSelector in Project

```
oc annotate namespace default openshift.io/node-selector=region=infra
```

## Disable defaultNodeSelector in Project

```
oc annotate namespace default openshift.io/node-selector=""
```

## Pruning

You need to create a user and add `system:image-pruner` role to the user.

```
oc adm policy add-cluster-role-to-user system:image-pruner pruner
```

```
oc login -u "system:admin"
oc adm prune deployments --confirm
oc adm prune builds --confirm
oc login -u pruner
oc adm prune images --confirm
oc login -u "system:admin"
```

## Manage nodes using Ansible ad-hoc command

```
ansible masters -a "sudo systemctl restart atomic-openshift-master"
ansible nodes -a "sudo systemctl restart atomic-openshift-node"
```

## Playbooks

```
ansible-playbook -vvv /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml
ansible-playbook -vvv /usr/share/ansible/openshift-ansible/playbooks/byo/config.yml
ansible-playbook -vvv /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/upgrades/v3_3/upgrade.yml
ansible-playbook -vvv /usr/share/ansible/openshift-ansible/playbooks/byo/openshift_facts.yml
```

