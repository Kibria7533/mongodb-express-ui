## First add bitnami because we will use bitnami mongo helm cahrt

```helm repo add bitnami https://charts.bitnami.com/bitnami
helm install mongodb bitnami/mongodb -f mongodb-custom-valuse.yaml
```
```
Here in custom values i just made few chenge like set root user name as root
and password as admin
NB:You may see in secret its base64 dont worry 
```

# Here below i am providing my custom values


```yaml
## @section Global parameters
## Global Docker image parameters
## Please, note that this will override the image parameters, including dependencies, configured to use the global value
## Current available global Docker image parameters: imageRegistry, imagePullSecrets and storageClass
##

## @param global.imageRegistry Global Docker image registry
## @param global.imagePullSecrets Global Docker registry secret names as an array
## @param global.storageClass Global StorageClass for Persistent Volume(s)
## @param global.namespaceOverride Override the namespace for resource deployed by the chart, but can itself be overridden by the local namespaceOverride
##
global:
  imageRegistry: ""
  ## E.g.
  ## imagePullSecrets:
  ##   - myRegistryKeySecretName
  ##
  imagePullSecrets: []
  storageClass: ""
  namespaceOverride: ""

## @section Common parameters
##

## @param nameOverride String to partially override mongodb.fullname template (will maintain the release name)
##
nameOverride: ""
## @param fullnameOverride String to fully override mongodb.fullname template
##
fullnameOverride: ""
## @param namespaceOverride String to fully override common.names.namespace
##
namespaceOverride: ""
## @param kubeVersion Force target Kubernetes version (using Helm capabilities if not set)
##
kubeVersion: ""
## @param clusterDomain Default Kubernetes cluster domain
##
clusterDomain: cluster.local
## @param extraDeploy Array of extra objects to deploy with the release
## extraDeploy:
## This needs to be uncommented and added to 'extraDeploy' in order to use the replicaset 'mongo-labeler' sidecar
## for dynamically discovering the mongodb primary pod
## suggestion is to use a hard-coded and predictable TCP port for the primary mongodb pod (here is 30001, choose your own)
## - apiVersion: v1
##   kind: Service
##   metadata:
##     name: mongodb-primary
##     namespace: the-mongodb-namespace
##     labels:
##       app.kubernetes.io/component: mongodb
##       app.kubernetes.io/instance: mongodb
##       app.kubernetes.io/managed-by: Helm
##       app.kubernetes.io/name: mongodb
##   spec:
##     type: NodePort
##     externalTrafficPolicy: Cluster
##     ports:
##       - name: mongodb
##         port: 30001
##         nodePort: 30001
##         protocol: TCP
##         targetPort: mongodb
##     selector:
##       app.kubernetes.io/component: mongodb
##       app.kubernetes.io/instance: mongodb
##       app.kubernetes.io/name: mongodb
##       primary: "true"
##
extraDeploy: []
## @param commonLabels Add labels to all the deployed resources (sub-charts are not considered). Evaluated as a template
##
commonLabels: {}
## @param commonAnnotations Common annotations to add to all Mongo resources (sub-charts are not considered). Evaluated as a template
##
commonAnnotations: {}

## Enable diagnostic mode in the deployment
##
diagnosticMode:
  ## @param diagnosticMode.enabled Enable diagnostic mode (all probes will be disabled and the command will be overridden)
  ##
  enabled: false
  ## @param diagnosticMode.command Command to override all containers in the deployment
  ##
  command:
    - sleep
  ## @param diagnosticMode.args Args to override all containers in the deployment
  ##
  args:
    - infinity

## @section MongoDB(&reg;) parameters
##

## Bitnami MongoDB(&reg;) image
## ref: https://hub.docker.com/r/bitnami/mongodb/tags/
## @param image.registry MongoDB(&reg;) image registry
## @param image.repository MongoDB(&reg;) image registry
## @param image.tag MongoDB(&reg;) image tag (immutable tags are recommended)
## @param image.digest MongoDB(&reg;) image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
## @param image.pullPolicy MongoDB(&reg;) image pull policy
## @param image.pullSecrets Specify docker-registry secret names as an array
## @param image.debug Set to true if you would like to see extra information on logs
##
image:
  registry: docker.io
  repository: bitnami/mongodb
  tag: 6.0.2-debian-11-r11
  digest: ""
  ## Specify a imagePullPolicy
  ## ref: https://kubernetes.io/docs/user-guide/images/#pre-pulling-images
  ##
  pullPolicy: IfNotPresent
  ## Optionally specify an array of imagePullSecrets.
  ## Secrets must be manually created in the namespace.
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
  ## e.g:
  ## pullSecrets:
  ##   - myRegistryKeySecretName
  ##
  pullSecrets: []
  ## Set to true if you would like to see extra information on logs
  ##
  debug: false

## @param schedulerName Name of the scheduler (other than default) to dispatch pods
## ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
##
schedulerName: ""
## @param architecture MongoDB(&reg;) architecture (`standalone` or `replicaset`)
##
architecture: standalone
## @param useStatefulSet Set to true to use a StatefulSet instead of a Deployment (only when `architecture=standalone`)
##
useStatefulSet: false
## MongoDB(&reg;) Authentication parameters
##
auth:
  ## @param auth.enabled Enable authentication
  ## ref: https://docs.mongodb.com/manual/tutorial/enable-authentication/
  ##
  enabled: true
  ## @param auth.rootUser MongoDB(&reg;) root user
  ##
  rootUser: root
  ## @param auth.rootPassword MongoDB(&reg;) root password
  ## ref: https://github.com/bitnami/containers/tree/main/bitnami/mongodb#setting-the-root-user-and-password-on-first-run
  ##
  rootPassword: "admin"
  ## MongoDB(&reg;) custom users and databases
  ## ref: https://github.com/bitnami/containers/tree/main/bitnami/mongodb#creating-a-user-and-database-on-first-run
  ## @param auth.usernames List of custom users to be created during the initialization
  ## @param auth.passwords List of passwords for the custom users set at `auth.usernames`
  ## @param auth.databases List of custom databases to be created during the initialization
  ##
  usernames: []
  passwords: []
  databases: []
  ## @param auth.username DEPRECATED: use `auth.usernames` instead
  ## @param auth.password DEPRECATED: use `auth.passwords` instead
  ## @param auth.database DEPRECATED: use `auth.databases` instead
  username: ""
  password: ""
  database: ""
  ## @param auth.replicaSetKey Key used for authentication in the replicaset (only when `architecture=replicaset`)
  ##
  replicaSetKey: ""
  ## @param auth.existingSecret Existing secret with MongoDB(&reg;) credentials (keys: `mongodb-passwords`, `mongodb-root-password`, `mongodb-metrics-password`, ` mongodb-replica-set-key`)
  ## NOTE: When it's set the previous parameters are ignored.
  ##
  existingSecret: ""
tls:
  ## @param tls.enabled Enable MongoDB(&reg;) TLS support between nodes in the cluster as well as between mongo clients and nodes
  ##
  enabled: false
  ## @param tls.autoGenerated Generate a custom CA and self-signed certificates
  ##
  autoGenerated: true
  ## @param tls.existingSecret Existing secret with TLS certificates (keys: `mongodb-ca-cert`, `mongodb-ca-key`)
  ## NOTE: When it's set it will disable secret creation.
  ##
  existingSecret: ""
  ## Add Custom CA certificate
  ## @param tls.caCert Custom CA certificated (base64 encoded)
  ## @param tls.caKey CA certificate private key (base64 encoded)
  ##
  caCert: ""
  caKey: ""
  standalone:
    ## @param tls.standalone.existingSecret Existing secret with TLS certificates (`tls.key`, `tls.crt`, `ca.crt`).
    ## NOTE: When it's set it will disable certificate self-generation from existing CA.
    ##
    existingSecret: ""
  replicaset:
    ## @param tls.replicaset.existingSecrets Array of existing secrets with TLS certificates (`tls.key`, `tls.crt`, `ca.crt`).
    ## existingSecrets:
    ##  - "mySecret-0"
    ##  - "mySecret-1"
    ## NOTE: When it's set it will disable certificate self-generation from existing CA.
    ##
    existingSecrets: []
  hidden:
    ## @param tls.hidden.existingSecrets Array of existing secrets with TLS certificates (`tls.key`, `tls.crt`, `ca.crt`).
    ## existingSecrets:
    ##  - "mySecret-0"
    ##  - "mySecret-1"
    ## NOTE: When it's set it will disable certificate self-generation from existing CA.
    ##
    existingSecrets: []
  arbiter:
    ## @param tls.arbiter.existingSecret Existing secret with TLS certificates (`tls.key`, `tls.crt`, `ca.crt`).
    ## NOTE: When it's set it will disable certificate self-generation from existing CA.
    ##
    existingSecret: ""
  ## Bitnami Nginx image
  ## @param tls.image.registry Init container TLS certs setup image registry
  ## @param tls.image.repository Init container TLS certs setup image repository
  ## @param tls.image.tag Init container TLS certs setup image tag (immutable tags are recommended)
  ## @param tls.image.digest Init container TLS certs setup image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
  ## @param tls.image.pullPolicy Init container TLS certs setup image pull policy
  ## @param tls.image.pullSecrets Init container TLS certs specify docker-registry secret names as an array
  ## @param tls.extraDnsNames Add extra dns names to the CA, can solve x509 auth issue for pod clients
  ##
  image:
    registry: docker.io
    repository: bitnami/nginx
    tag: 1.23.2-debian-11-r3
    digest: ""
    pullPolicy: IfNotPresent
    ## Optionally specify an array of imagePullSecrets.
    ## Secrets must be manually created in the namespace.
    ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    ## e.g:
    ## pullSecrets:
    ##   - myRegistryKeySecretName
    ##
    pullSecrets: []

  ## e.g:
  ## extraDnsNames
  ##   "DNS.6": "$my_host"
  ##   "DNS.7": "$test"
  ##
  extraDnsNames: []
  ## @param tls.mode Allows to set the tls mode which should be used when tls is enabled (options: `allowTLS`, `preferTLS`, `requireTLS`)
  ##
  mode: requireTLS
  ## Init Container resource requests and limits
  ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param tls.resources.limits Init container generate-tls-certs resource limits
  ## @param tls.resources.requests Init container generate-tls-certs resource requests
  ##
  resources:
    ## Example:
    ## limits:
    ##   cpu: 100m
    ##   memory: 128Mi
    ##
    limits: {}
    ## Examples:
    ## requests:
    ##   cpu: 100m
    ##   memory: 128Mi
    ##
    requests: {}
## @param hostAliases Add deployment host aliases
## https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/
##
hostAliases: []
## @param replicaSetName Name of the replica set (only when `architecture=replicaset`)
## Ignored when mongodb.architecture=standalone
##
replicaSetName: rs0
## @param replicaSetHostnames Enable DNS hostnames in the replicaset config (only when `architecture=replicaset`)
## Ignored when mongodb.architecture=standalone
## Ignored when externalAccess.enabled=true
##
replicaSetHostnames: true
## @param enableIPv6 Switch to enable/disable IPv6 on MongoDB(&reg;)
## ref: https://github.com/bitnami/containers/tree/main/bitnami/mongodb#enablingdisabling-ipv6
##
enableIPv6: false
## @param directoryPerDB Switch to enable/disable DirectoryPerDB on MongoDB(&reg;)
## ref: https://github.com/bitnami/containers/tree/main/bitnami/mongodb#enablingdisabling-directoryperdb
##
directoryPerDB: false
## MongoDB(&reg;) System Log configuration
## ref: https://github.com/bitnami/containers/tree/main/bitnami/mongodb#configuring-system-log-verbosity-level
## @param systemLogVerbosity MongoDB(&reg;) system log verbosity level
## @param disableSystemLog Switch to enable/disable MongoDB(&reg;) system log
##
systemLogVerbosity: 0
disableSystemLog: false
## @param disableJavascript Switch to enable/disable MongoDB(&reg;) server-side JavaScript execution
## ref: https://docs.mongodb.com/manual/core/server-side-javascript/
##
disableJavascript: false
## @param enableJournal Switch to enable/disable MongoDB(&reg;) Journaling
## ref: https://docs.mongodb.com/manual/reference/configuration-options/#mongodb-setting-storage.journal.enabled
##
enableJournal: true
## @param configuration MongoDB(&reg;) configuration file to be used for Primary and Secondary nodes
## For documentation of all options, see: http://docs.mongodb.org/manual/reference/configuration-options/
## Example:
## configuration: |-
##   # where and how to store data.
##   storage:
##     dbPath: /bitnami/mongodb/data/db
##     journal:
##       enabled: true
##     directoryPerDB: false
##   # where to write logging data
##   systemLog:
##     destination: file
##     quiet: false
##     logAppend: true
##     logRotate: reopen
##     path: /opt/bitnami/mongodb/logs/mongodb.log
##     verbosity: 0
##   # network interfaces
##   net:
##     port: 27017
##     unixDomainSocket:
##       enabled: true
##       pathPrefix: /opt/bitnami/mongodb/tmp
##     ipv6: false
##     bindIpAll: true
##   # replica set options
##   #replication:
##     #replSetName: replicaset
##     #enableMajorityReadConcern: true
##   # process management options
##   processManagement:
##      fork: false
##      pidFilePath: /opt/bitnami/mongodb/tmp/mongodb.pid
##   # set parameter options
##   setParameter:
##      enableLocalhostAuthBypass: true
##   # security options
##   security:
##     authorization: disabled
##     #keyFile: /opt/bitnami/mongodb/conf/keyfile
##
configuration: ""
## @section replicaSetConfigurationSettings settings applied during runtime (not via configuration file)
## If enabled, these are applied by a script which is called within setup.sh
## for documentation see https://docs.mongodb.com/manual/reference/replica-configuration/#replica-set-configuration-fields
## @param replicaSetConfigurationSettings.enabled Enable MongoDB(&reg;) Switch to enable/disable configuring MongoDB(&reg;) run time rs.conf settings
## @param replicaSetConfigurationSettings.configuration run-time rs.conf settings
##
replicaSetConfigurationSettings:
  enabled: false
  configuration: {}
##    chainingAllowed : false
##    heartbeatTimeoutSecs : 10
##    heartbeatIntervalMillis : 2000
##    electionTimeoutMillis : 10000
##    catchUpTimeoutMillis : 30000
## @param existingConfigmap Name of existing ConfigMap with MongoDB(&reg;) configuration for Primary and Secondary nodes
## NOTE: When it's set the arbiter.configuration parameter is ignored
##
existingConfigmap: ""
## @param initdbScripts Dictionary of initdb scripts
## Specify dictionary of scripts to be run at first boot
## Example:
## initdbScripts:
##   my_init_script.sh: |
##      #!/bin/bash
##      echo "Do something."
##
initdbScripts: {}
## @param initdbScriptsConfigMap Existing ConfigMap with custom initdb scripts
##
initdbScriptsConfigMap: ""
## Command and args for running the container (set to default if not set). Use array form
## @param command Override default container command (useful when using custom images)
## @param args Override default container args (useful when using custom images)
##
command: []
args: []
## @param extraFlags MongoDB(&reg;) additional command line flags
## Example:
## extraFlags:
##  - "--wiredTigerCacheSizeGB=2"
##
extraFlags: []
## @param extraEnvVars Extra environment variables to add to MongoDB(&reg;) pods
## E.g:
## extraEnvVars:
##   - name: FOO
##     value: BAR
##
extraEnvVars: []
## @param extraEnvVarsCM Name of existing ConfigMap containing extra env vars
##
extraEnvVarsCM: ""
## @param extraEnvVarsSecret Name of existing Secret containing extra env vars (in case of sensitive data)
##
extraEnvVarsSecret: ""

## @section MongoDB(&reg;) statefulset parameters
##

## @param annotations Additional labels to be added to the MongoDB(&reg;) statefulset. Evaluated as a template
##
annotations: {}
## @param labels Annotations to be added to the MongoDB(&reg;) statefulset. Evaluated as a template
##
labels: {}
## @param replicaCount Number of MongoDB(&reg;) nodes (only when `architecture=replicaset`)
## Ignored when mongodb.architecture=standalone
##
replicaCount: 2
## @param updateStrategy.type Strategy to use to replace existing MongoDB(&reg;) pods. When architecture=standalone and useStatefulSet=false,
## this parameter will be applied on a deployment object. In other case it will be applied on a statefulset object
## ref: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#update-strategies
## ref: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy
## Example:
## updateStrategy:
##  type: RollingUpdate
##  rollingUpdate:
##    maxSurge: 25%
##    maxUnavailable: 25%
##
updateStrategy:
  type: RollingUpdate
## @param podManagementPolicy Pod management policy for MongoDB(&reg;)
## Should be initialized one by one when building the replicaset for the first time
##
podManagementPolicy: OrderedReady
## @param podAffinityPreset MongoDB(&reg;) Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
##
podAffinityPreset: ""
## @param podAntiAffinityPreset MongoDB(&reg;) Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
##
podAntiAffinityPreset: soft
## Node affinity preset
## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity
##
nodeAffinityPreset:
  ## @param nodeAffinityPreset.type MongoDB(&reg;) Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ##
  type: ""
  ## @param nodeAffinityPreset.key MongoDB(&reg;) Node label key to match Ignored if `affinity` is set.
  ## E.g.
  ## key: "kubernetes.io/e2e-az-name"
  ##
  key: ""
  ## @param nodeAffinityPreset.values MongoDB(&reg;) Node label values to match. Ignored if `affinity` is set.
  ## E.g.
  ## values:
  ##   - e2e-az1
  ##   - e2e-az2
  ##
  values: []
## @param affinity MongoDB(&reg;) Affinity for pod assignment
## ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
## Note: podAffinityPreset, podAntiAffinityPreset, and nodeAffinityPreset will be ignored when it's set
##
affinity: {}
## @param nodeSelector MongoDB(&reg;) Node labels for pod assignment
## ref: https://kubernetes.io/docs/user-guide/node-selection/
##
nodeSelector: {}
## @param tolerations MongoDB(&reg;) Tolerations for pod assignment
## ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
##
tolerations: []
## @param topologySpreadConstraints MongoDB(&reg;) Spread Constraints for Pods
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/
##
topologySpreadConstraints: []
## @param lifecycleHooks LifecycleHook for the MongoDB(&reg;) container(s) to automate configuration before or after startup
##
lifecycleHooks: {}
## @param terminationGracePeriodSeconds MongoDB(&reg;) Termination Grace Period
##
terminationGracePeriodSeconds: ""
## @param podLabels MongoDB(&reg;) pod labels
## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
##
podLabels: {}
## @param podAnnotations MongoDB(&reg;) Pod annotations
## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
##
podAnnotations: {}
## @param priorityClassName Name of the existing priority class to be used by MongoDB(&reg;) pod(s)
## ref: https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/
##
priorityClassName: ""
## @param runtimeClassName Name of the runtime class to be used by MongoDB(&reg;) pod(s)
## ref: https://kubernetes.io/docs/concepts/containers/runtime-class/
##
runtimeClassName: ""
## MongoDB(&reg;) pods' Security Context.
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod
## @param podSecurityContext.enabled Enable MongoDB(&reg;) pod(s)' Security Context
## @param podSecurityContext.fsGroup Group ID for the volumes of the MongoDB(&reg;) pod(s)
## @param podSecurityContext.sysctls sysctl settings of the MongoDB(&reg;) pod(s)'
##
podSecurityContext:
  enabled: true
  fsGroup: 1001
  ## sysctl settings
  ## Example:
  ## sysctls:
  ## - name: net.core.somaxconn
  ##   value: "10000"
  ##
  sysctls: []
## MongoDB(&reg;) containers' Security Context (main and metrics container).
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container
## @param containerSecurityContext.enabled Enable MongoDB(&reg;) container(s)' Security Context
## @param containerSecurityContext.runAsUser User ID for the MongoDB(&reg;) container
## @param containerSecurityContext.runAsNonRoot Set MongoDB(&reg;) container's Security Context runAsNonRoot
##
containerSecurityContext:
  enabled: true
  runAsUser: 1001
  runAsNonRoot: true
## MongoDB(&reg;) containers' resource requests and limits.
## ref: https://kubernetes.io/docs/user-guide/compute-resources/
## We usually recommend not to specify default resources and to leave this as a conscious
## choice for the user. This also increases chances charts run on environments with little
## resources, such as Minikube. If you do want to specify resources, uncomment the following
## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
## @param resources.limits The resources limits for MongoDB(&reg;) containers
## @param resources.requests The requested resources for MongoDB(&reg;) containers
##
resources:
  ## Example:
  ## limits:
  ##    cpu: 100m
  ##    memory: 128Mi
  ##
  limits: {}
  ## Examples:
  ## requests:
  ##    cpu: 100m
  ##    memory: 128Mi
  ##
  requests: {}
## @param containerPorts.mongodb MongoDB(&reg;) container port
containerPorts:
  mongodb: 27017
## MongoDB(&reg;) pods' liveness probe. Evaluated as a template.
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
## @param livenessProbe.enabled Enable livenessProbe
## @param livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
## @param livenessProbe.periodSeconds Period seconds for livenessProbe
## @param livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
## @param livenessProbe.failureThreshold Failure threshold for livenessProbe
## @param livenessProbe.successThreshold Success threshold for livenessProbe
##
livenessProbe:
  enabled: true
  initialDelaySeconds: 30
  periodSeconds: 20
  timeoutSeconds: 10
  failureThreshold: 6
  successThreshold: 1
## MongoDB(&reg;) pods' readiness probe. Evaluated as a template.
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
## @param readinessProbe.enabled Enable readinessProbe
## @param readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
## @param readinessProbe.periodSeconds Period seconds for readinessProbe
## @param readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
## @param readinessProbe.failureThreshold Failure threshold for readinessProbe
## @param readinessProbe.successThreshold Success threshold for readinessProbe
##
readinessProbe:
  enabled: true
  initialDelaySeconds: 5
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 6
  successThreshold: 1
## Slow starting containers can be protected through startup probes
## Startup probes are available in Kubernetes version 1.16 and above
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-startup-probes
## @param startupProbe.enabled Enable startupProbe
## @param startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
## @param startupProbe.periodSeconds Period seconds for startupProbe
## @param startupProbe.timeoutSeconds Timeout seconds for startupProbe
## @param startupProbe.failureThreshold Failure threshold for startupProbe
## @param startupProbe.successThreshold Success threshold for startupProbe
##
startupProbe:
  enabled: false
  initialDelaySeconds: 5
  periodSeconds: 20
  timeoutSeconds: 10
  successThreshold: 1
  failureThreshold: 30
## @param customLivenessProbe Override default liveness probe for MongoDB(&reg;) containers
## Ignored when livenessProbe.enabled=true
##
customLivenessProbe: {}
## @param customReadinessProbe Override default readiness probe for MongoDB(&reg;) containers
## Ignored when readinessProbe.enabled=true
##
customReadinessProbe: {}
## @param customStartupProbe Override default startup probe for MongoDB(&reg;) containers
## Ignored when startupProbe.enabled=true
##
customStartupProbe: {}
## @param initContainers Add additional init containers for the hidden node pod(s)
## Example:
## initContainers:
##   - name: your-image-name
##     image: your-image
##     imagePullPolicy: Always
##     ports:
##       - name: portname
##         containerPort: 1234
##
initContainers: []
## @param sidecars Add additional sidecar containers for the MongoDB(&reg;) pod(s)
## Example:
## sidecars:
##   - name: your-image-name
##     image: your-image
##     imagePullPolicy: Always
##     ports:
##       - name: portname
##         containerPort: 1234
## This is an optional 'mongo-labeler' sidecar container that tracks replica-set for the primary mongodb pod
## and labels it dynamically with ' primary: "true" ' in order for an extra-deployed service to always expose
## and attach to the primary pod, this needs to be uncommented along with the suggested 'extraDeploy' example
## and the suggested rbac example for the pod to be allowed adding labels to mongo replica pods
## search 'mongo-labeler' through this file to find the sections that needs to be uncommented to make it work
##
## - name: mongo-labeler
##   image: korenlev/k8s-mongo-labeler-sidecar
##   imagePullPolicy: Always
##   env:
##     - name: LABEL_SELECTOR
##       value: "app.kubernetes.io/component=mongodb,app.kubernetes.io/instance=mongodb,app.kubernetes.io/name=mongodb"
##     - name: NAMESPACE
##       value: "the-mongodb-namespace"
##     - name: DEBUG
##       value: "true"
##
sidecars: []
## @param extraVolumeMounts Optionally specify extra list of additional volumeMounts for the MongoDB(&reg;) container(s)
## Examples:
## extraVolumeMounts:
##   - name: extras
##     mountPath: /usr/share/extras
##     readOnly: true
##
extraVolumeMounts: []
## @param extraVolumes Optionally specify extra list of additional volumes to the MongoDB(&reg;) statefulset
## extraVolumes:
##   - name: extras
##     emptyDir: {}
##
extraVolumes: []
## MongoDB(&reg;) Pod Disruption Budget configuration
## ref: https://kubernetes.io/docs/tasks/run-application/configure-pdb/
##
pdb:
  ## @param pdb.create Enable/disable a Pod Disruption Budget creation for MongoDB(&reg;) pod(s)
  ##
  create: false
  ## @param pdb.minAvailable Minimum number/percentage of MongoDB(&reg;) pods that must still be available after the eviction
  ##
  minAvailable: 1
  ## @param pdb.maxUnavailable Maximum number/percentage of MongoDB(&reg;) pods that may be made unavailable after the eviction
  ##
  maxUnavailable: ""

## @section Traffic exposure parameters
##

## Service parameters
##
service:
  ## @param service.nameOverride MongoDB(&reg;) service name
  ##
  nameOverride: ""
  ## @param service.type Kubernetes Service type (only for standalone architecture)
  ##
  type: ClusterIP
  ## @param service.portName MongoDB(&reg;) service port name (only for standalone architecture)
  ##
  portName: mongodb
  ## @param service.ports.mongodb MongoDB(&reg;) service port.
  ##
  ports:
    mongodb: 27017
  ## @param service.nodePorts.mongodb Port to bind to for NodePort and LoadBalancer service types (only for standalone architecture)
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport
  ##
  nodePorts:
    mongodb: ""
  ## @param service.clusterIP MongoDB(&reg;) service cluster IP (only for standalone architecture)
  ## e.g:
  ## clusterIP: None
  ##
  clusterIP: ""
  ## @param service.externalIPs Specify the externalIP value ClusterIP service type (only for standalone architecture)
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#external-ips
  ##
  externalIPs: []
  ## @param service.loadBalancerIP loadBalancerIP for MongoDB(&reg;) Service (only for standalone architecture)
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer
  ##
  loadBalancerIP: ""
  ## @param service.loadBalancerClass loadBalancerClass for MongoDB(&reg;) Service (only for standalone architecture)
  # ref: https://kubernetes.io/docs/concepts/services-networking/service/#load-balancer-class
  loadBalancerClass: ""
  ## @param service.loadBalancerSourceRanges Address(es) that are allowed when service is LoadBalancer (only for standalone architecture)
  ## ref: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
  ##
  loadBalancerSourceRanges: []
  ## @param service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
  ##
  extraPorts: []
  ## @param service.annotations Provide any additional annotations that may be required
  ##
  annotations: {}
  ## @param service.externalTrafficPolicy service external traffic policy (only for standalone architecture)
  ## ref https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip
  ##
  externalTrafficPolicy: Local
  ## @param service.sessionAffinity Control where client requests go, to the same pod or round-robin
  ## Values: ClientIP or None
  ## ref: https://kubernetes.io/docs/user-guide/services/
  ##
  sessionAffinity: None
  ## @param service.sessionAffinityConfig Additional settings for the sessionAffinity
  ## sessionAffinityConfig:
  ##   clientIP:
  ##     timeoutSeconds: 300
  ##
  sessionAffinityConfig: {}
## External Access to MongoDB(&reg;) nodes configuration
##
externalAccess:
  ## @param externalAccess.enabled Enable Kubernetes external cluster access to MongoDB(&reg;) nodes (only for replicaset architecture)
  ##
  enabled: true
  ## External IPs auto-discovery configuration
  ## An init container is used to auto-detect LB IPs or node ports by querying the K8s API
  ## Note: RBAC might be required
  ##
  autoDiscovery:
    ## @param externalAccess.autoDiscovery.enabled Enable using an init container to auto-detect external IPs by querying the K8s API
    ##
    enabled: true
    ## Bitnami Kubectl image
    ## ref: https://hub.docker.com/r/bitnami/kubectl/tags/
    ## @param externalAccess.autoDiscovery.image.registry Init container auto-discovery image registry
    ## @param externalAccess.autoDiscovery.image.repository Init container auto-discovery image repository
    ## @param externalAccess.autoDiscovery.image.tag Init container auto-discovery image tag (immutable tags are recommended)
    ## @param externalAccess.autoDiscovery.image.digest Init container auto-discovery image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
    ## @param externalAccess.autoDiscovery.image.pullPolicy Init container auto-discovery image pull policy
    ## @param externalAccess.autoDiscovery.image.pullSecrets Init container auto-discovery image pull secrets
    ##
    image:
      registry: docker.io
      repository: bitnami/kubectl
      tag: 1.25.3-debian-11-r4
      digest: ""
      ## Specify a imagePullPolicy
      ## Defaults to 'Always' if image tag is 'latest', else set to 'IfNotPresent'
      ## ref: https://kubernetes.io/docs/user-guide/images/#pre-pulling-images
      ##
      pullPolicy: IfNotPresent
      ## Optionally specify an array of imagePullSecrets (secrets must be manually created in the namespace)
      ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
      ## Example:
      ## pullSecrets:
      ##   - myRegistryKeySecretName
      ##
      pullSecrets: []
    ## Init Container resource requests and limits
    ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
    ## We usually recommend not to specify default resources and to leave this as a conscious
    ## choice for the user. This also increases chances charts run on environments with little
    ## resources, such as Minikube. If you do want to specify resources, uncomment the following
    ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
    ## @param externalAccess.autoDiscovery.resources.limits Init container auto-discovery resource limits
    ## @param externalAccess.autoDiscovery.resources.requests Init container auto-discovery resource requests
    ##
    resources:
      ## Example:
      ## limits:
      ##    cpu: 100m
      ##    memory: 128Mi
      ##
      limits: {}
      ## Examples:
      ## requests:
      ##    cpu: 100m
      ##    memory: 128Mi
      ##
      requests: {}
  ## Parameters to configure K8s service(s) used to externally access MongoDB(&reg;)
  ## A new service per broker will be created
  ##
  service:
    ## @param externalAccess.service.type Kubernetes Service type for external access. Allowed values: NodePort, LoadBalancer or ClusterIP
    ##
    type: LoadBalancer
    ## @param externalAccess.service.portName MongoDB(&reg;) port name used for external access when service type is LoadBalancer
    ##
    portName: "mongodb"
    ## @param externalAccess.service.ports.mongodb MongoDB(&reg;) port used for external access when service type is LoadBalancer
    ##
    ports:
      mongodb: 27017
    ## @param externalAccess.service.loadBalancerIPs Array of load balancer IPs for MongoDB(&reg;) nodes
    ## Example:
    ## loadBalancerIPs:
    ##   - X.X.X.X
    ##   - Y.Y.Y.Y
    ##
    loadBalancerIPs: []
    ## @param externalAccess.service.loadBalancerClass loadBalancerClass when service type is LoadBalancer
    # ref: https://kubernetes.io/docs/concepts/services-networking/service/#load-balancer-class
    loadBalancerClass: ""
    ## @param externalAccess.service.loadBalancerSourceRanges Address(es) that are allowed when service is LoadBalancer
    ## ref: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
    ## Example:
    ## loadBalancerSourceRanges:
    ## - 10.10.10.0/24
    ##
    loadBalancerSourceRanges: []
    ## @param externalAccess.service.externalTrafficPolicy MongoDB(&reg;) service external traffic policy
    ## ref https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip
    ##
    externalTrafficPolicy: Local
    ## @param externalAccess.service.nodePorts Array of node ports used to configure MongoDB(&reg;) advertised hostname when service type is NodePort
    ## Example:
    ## nodePorts:
    ##   - 30001
    ##   - 30002
    ##
    nodePorts: []
    ## @param externalAccess.service.domain Domain or external IP used to configure MongoDB(&reg;) advertised hostname when service type is NodePort
    ## If not specified, the container will try to get the kubernetes node external IP
    ## e.g:
    ## domain: mydomain.com
    ##
    domain: ""
    ## @param externalAccess.service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
    ##
    extraPorts: []
    ## @param externalAccess.service.annotations Service annotations for external access
    ##
    annotations: {}
      ## @param externalAccess.service.sessionAffinity Control where client requests go, to the same pod or round-robin
    ## Values: ClientIP or None
    ## ref: https://kubernetes.io/docs/user-guide/services/
    ##
    sessionAffinity: None
    ## @param externalAccess.service.sessionAffinityConfig Additional settings for the sessionAffinity
    ## sessionAffinityConfig:
    ##   clientIP:
    ##     timeoutSeconds: 300
    ##
    sessionAffinityConfig: {}
  ## External Access to MongoDB(&reg;) Hidden nodes configuration
  ##
  hidden:
    ## @param externalAccess.hidden.enabled Enable Kubernetes external cluster access to MongoDB(&reg;) hidden nodes
    ##
    enabled: false
    ## Parameters to configure K8s service(s) used to externally access MongoDB(&reg;)
    ## A new service per broker will be created
    ##
    service:
      ## @param externalAccess.hidden.service.type Kubernetes Service type for external access. Allowed values: NodePort or LoadBalancer
      ##
      type: LoadBalancer
      ## @param externalAccess.hidden.service.portName MongoDB(&reg;) port name used for external access when service type is LoadBalancer
      ##
      portName: "mongodb"
      ## @param externalAccess.hidden.service.ports.mongodb MongoDB(&reg;) port used for external access when service type is LoadBalancer
      ##
      ports:
        mongodb: 27017
      ## @param externalAccess.hidden.service.loadBalancerIPs Array of load balancer IPs for MongoDB(&reg;) nodes
      ## Example:
      ## loadBalancerIPs:
      ##   - X.X.X.X
      ##   - Y.Y.Y.Y
      ##
      loadBalancerIPs: []
      ## @param externalAccess.hidden.service.loadBalancerClass loadBalancerClass when service type is LoadBalancer
      # ref: https://kubernetes.io/docs/concepts/services-networking/service/#load-balancer-class
      loadBalancerClass: ""
      ## @param externalAccess.hidden.service.loadBalancerSourceRanges Address(es) that are allowed when service is LoadBalancer
      ## ref: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
      ## Example:
      ## loadBalancerSourceRanges:
      ## - 10.10.10.0/24
      ##
      loadBalancerSourceRanges: []
      ## @param externalAccess.hidden.service.externalTrafficPolicy MongoDB(&reg;) service external traffic policy
      ## ref https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip
      ##
      externalTrafficPolicy: Local
      ## @param externalAccess.hidden.service.nodePorts Array of node ports used to configure MongoDB(&reg;) advertised hostname when service type is NodePort. Length must be the same as replicaCount
      ## Example:
      ## nodePorts:
      ##   - 30001
      ##   - 30002
      ##
      nodePorts: []
      ## @param externalAccess.hidden.service.domain Domain or external IP used to configure MongoDB(&reg;) advertised hostname when service type is NodePort
      ## If not specified, the container will try to get the kubernetes node external IP
      ## e.g:
      ## domain: mydomain.com
      ##
      domain: ""
      ## @param externalAccess.hidden.service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
      ##
      extraPorts: []
      ## @param externalAccess.hidden.service.annotations Service annotations for external access
      ##
      annotations: {}
      ## @param externalAccess.hidden.service.sessionAffinity Control where client requests go, to the same pod or round-robin
      ## Values: ClientIP or None
      ## ref: https://kubernetes.io/docs/user-guide/services/
      ##
      sessionAffinity: None
      ## @param externalAccess.hidden.service.sessionAffinityConfig Additional settings for the sessionAffinity
      ## sessionAffinityConfig:
      ##   clientIP:
      ##     timeoutSeconds: 300
      ##
      sessionAffinityConfig: {}

## @section Persistence parameters
##

## Enable persistence using Persistent Volume Claims
## ref: https://kubernetes.io/docs/user-guide/persistent-volumes/
##
persistence:
  ## @param persistence.enabled Enable MongoDB(&reg;) data persistence using PVC
  ##
  enabled: true
  ## @param persistence.medium Provide a medium for `emptyDir` volumes.
  ## Requires persistence.enabled: false
  ##
  medium: ""
  ## @param persistence.existingClaim Provide an existing `PersistentVolumeClaim` (only when `architecture=standalone`)
  ## Requires persistence.enabled: true
  ## If defined, PVC must be created manually before volume will be bound
  ## Ignored when mongodb.architecture=replicaset
  ##
  existingClaim: ""
  ## @param persistence.resourcePolicy Setting it to "keep" to avoid removing PVCs during a helm delete operation. Leaving it empty will delete PVCs after the chart deleted
  resourcePolicy: ""
  ## @param persistence.storageClass PVC Storage Class for MongoDB(&reg;) data volume
  ## If defined, storageClassName: <storageClass>
  ## If set to "-", storageClassName: "", which disables dynamic provisioning
  ## If undefined (the default) or set to null, no storageClassName spec is
  ## set, choosing the default provisioner.
  ##
  storageClass: ""
  ## @param persistence.accessModes PV Access Mode
  ##
  accessModes:
    - ReadWriteOnce
  ## @param persistence.size PVC Storage Request for MongoDB(&reg;) data volume
  ##
  size: 8Gi
  ## @param persistence.annotations PVC annotations
  ##
  annotations: {}
  ## @param persistence.mountPath Path to mount the volume at
  ## MongoDB(&reg;) images.
  ##
  mountPath: /bitnami/mongodb
  ## @param persistence.subPath Subdirectory of the volume to mount at
  ## and one PV for multiple services.
  ##
  subPath: ""
  ## Fine tuning for volumeClaimTemplates
  ##
  volumeClaimTemplates:
    ## @param persistence.volumeClaimTemplates.selector A label query over volumes to consider for binding (e.g. when using local volumes)
    ## A label query over volumes to consider for binding (e.g. when using local volumes)
    ## See https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#labelselector-v1-meta for more details
    ##
    selector: {}
    ## @param persistence.volumeClaimTemplates.requests Custom PVC requests attributes
    ## Sometime cloud providers use additional requests attributes to provision custom storage instance
    ## See https://cloud.ibm.com/docs/containers?topic=containers-file_storage#file_dynamic_statefulset
    ##
    requests: {}
    ## @param persistence.volumeClaimTemplates.dataSource Add dataSource to the VolumeClaimTemplate
    ##
    dataSource: {}

## @section RBAC parameters
##

## ServiceAccount
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
##
serviceAccount:
  ## @param serviceAccount.create Enable creation of ServiceAccount for MongoDB(&reg;) pods
  ##
  create: true
  ## @param serviceAccount.name Name of the created serviceAccount
  ## If not set and create is true, a name is generated using the mongodb.fullname template
  ##
  name: ""
  ## @param serviceAccount.annotations Additional Service Account annotations
  ##
  annotations: {}
  ## @param serviceAccount.automountServiceAccountToken Allows auto mount of ServiceAccountToken on the serviceAccount created
  ## Can be set to false if pods using this serviceAccount do not need to use K8s API
  ##
  automountServiceAccountToken: true
## Role Based Access
## ref: https://kubernetes.io/docs/admin/authorization/rbac/
##
rbac:
  ## @param rbac.create Whether to create & use RBAC resources or not
  ## binding MongoDB(&reg;) ServiceAccount to a role
  ## that allows MongoDB(&reg;) pods querying the K8s API
  ## this needs to be set to 'true' to enable the mongo-labeler sidecar primary mongodb discovery
  ##
  create: false
  ## @param rbac.rules Custom rules to create following the role specification
  ## The example below needs to be uncommented to use the 'mongo-labeler' sidecar for dynamic discovery of the primary mongodb pod:
  ## rules:
  ##   - apiGroups:
  ##       - ""
  ##     resources:
  ##       - pods
  ##     verbs:
  ##       - get
  ##       - list
  ##       - watch
  ##       - update
  ##
  rules: []
## PodSecurityPolicy configuration
## Be sure to also set rbac.create to true, otherwise Role and RoleBinding won't be created.
## ref: https://kubernetes.io/docs/concepts/policy/pod-security-policy/
##
podSecurityPolicy:
  ## @param podSecurityPolicy.create Whether to create a PodSecurityPolicy. WARNING: PodSecurityPolicy is deprecated in Kubernetes v1.21 or later, unavailable in v1.25 or later
  ##
  create: false
  ## @param podSecurityPolicy.allowPrivilegeEscalation Enable privilege escalation
  ## Either use predefined policy with some adjustments or use `podSecurityPolicy.spec`
  ##
  allowPrivilegeEscalation: false
  ## @param podSecurityPolicy.privileged Allow privileged
  ##
  privileged: false
  ## @param podSecurityPolicy.spec Specify the full spec to use for Pod Security Policy
  ## ref: https://kubernetes.io/docs/concepts/policy/pod-security-policy/
  ## Defining a spec ignores the above values.
  ##
  spec: {}
  ## Example:
  ##    allowPrivilegeEscalation: false
  ##    fsGroup:
  ##      rule: 'MustRunAs'
  ##      ranges:
  ##        - min: 1001
  ##          max: 1001
  ##    hostIPC: false
  ##    hostNetwork: false
  ##    hostPID: false
  ##    privileged: false
  ##    readOnlyRootFilesystem: false
  ##    requiredDropCapabilities:
  ##      - ALL
  ##    runAsUser:
  ##      rule: 'MustRunAs'
  ##      ranges:
  ##        - min: 1001
  ##          max: 1001
  ##    seLinux:
  ##      rule: 'RunAsAny'
  ##    supplementalGroups:
  ##      rule: 'MustRunAs'
  ##      ranges:
  ##        - min: 1001
  ##          max: 1001
  ##    volumes:
  ##      - 'configMap'
  ##      - 'secret'
  ##      - 'emptyDir'
  ##      - 'persistentVolumeClaim'
  ##

## @section Volume Permissions parameters
##
## Init Container parameters
## Change the owner and group of the persistent volume(s) mountpoint(s) to 'runAsUser:fsGroup' on each component
## values from the securityContext section of the component
##
volumePermissions:
  ## @param volumePermissions.enabled Enable init container that changes the owner and group of the persistent volume(s) mountpoint to `runAsUser:fsGroup`
  ##
  enabled: false
  ## @param volumePermissions.image.registry Init container volume-permissions image registry
  ## @param volumePermissions.image.repository Init container volume-permissions image repository
  ## @param volumePermissions.image.tag Init container volume-permissions image tag (immutable tags are recommended)
  ## @param volumePermissions.image.digest Init container volume-permissions image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
  ## @param volumePermissions.image.pullPolicy Init container volume-permissions image pull policy
  ## @param volumePermissions.image.pullSecrets Specify docker-registry secret names as an array
  ##
  image:
    registry: docker.io
    repository: bitnami/bitnami-shell
    tag: 11-debian-11-r46
    digest: ""
    ## Specify a imagePullPolicy
    ## Defaults to 'Always' if image tag is 'latest', else set to 'IfNotPresent'
    ## ref: https://kubernetes.io/docs/user-guide/images/#pre-pulling-images
    ##
    pullPolicy: IfNotPresent
    ## Optionally specify an array of imagePullSecrets (secrets must be manually created in the namespace)
    ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    ## Example:
    ## pullSecrets:
    ##   - myRegistryKeySecretName
    ##
    pullSecrets: []
  ## Init Container resource requests and limits
  ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param volumePermissions.resources.limits Init container volume-permissions resource limits
  ## @param volumePermissions.resources.requests Init container volume-permissions resource requests
  ##
  resources:
    ## Example:
    ## limits:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    limits: {}
    ## Examples:
    ## requests:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    requests: {}
  ## Init container Security Context
  ## Note: the chown of the data folder is done to containerSecurityContext.runAsUser
  ## and not the below volumePermissions.securityContext.runAsUser
  ## When runAsUser is set to special value "auto", init container will try to chwon the
  ## data folder to autodetermined user&group, using commands: `id -u`:`id -G | cut -d" " -f2`
  ## "auto" is especially useful for OpenShift which has scc with dynamic userids (and 0 is not allowed).
  ## You may want to use this volumePermissions.securityContext.runAsUser="auto" in combination with
  ## podSecurityContext.enabled=false,containerSecurityContext.enabled=false and shmVolume.chmod.enabled=false
  ## @param volumePermissions.securityContext.runAsUser User ID for the volumePermissions container
  ##
  securityContext:
    runAsUser: 0

## @section Arbiter parameters
##

arbiter:
  ## @param arbiter.enabled Enable deploying the arbiter
  ##   https://docs.mongodb.com/manual/tutorial/add-replica-set-arbiter/
  ##
  enabled: true
  ## @param arbiter.hostAliases Add deployment host aliases
  ## https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/
  ##
  hostAliases: []
  ## @param arbiter.configuration Arbiter configuration file to be used
  ##   http://docs.mongodb.org/manual/reference/configuration-options/
  ##
  configuration: ""
  ## @param arbiter.existingConfigmap Name of existing ConfigMap with Arbiter configuration
  ## NOTE: When it's set the arbiter.configuration parameter is ignored
  ##
  existingConfigmap: ""
  ## Command and args for running the container (set to default if not set). Use array form
  ## @param arbiter.command Override default container command (useful when using custom images)
  ## @param arbiter.args Override default container args (useful when using custom images)
  ##
  command: []
  args: []
  ## @param arbiter.extraFlags Arbiter additional command line flags
  ## Example:
  ## extraFlags:
  ##  - "--wiredTigerCacheSizeGB=2"
  ##
  extraFlags: []
  ## @param arbiter.extraEnvVars Extra environment variables to add to Arbiter pods
  ## E.g:
  ## extraEnvVars:
  ##   - name: FOO
  ##     value: BAR
  ##
  extraEnvVars: []
  ## @param arbiter.extraEnvVarsCM Name of existing ConfigMap containing extra env vars
  ##
  extraEnvVarsCM: ""
  ## @param arbiter.extraEnvVarsSecret Name of existing Secret containing extra env vars (in case of sensitive data)
  ##
  extraEnvVarsSecret: ""
  ## @param arbiter.annotations Additional labels to be added to the Arbiter statefulset
  ##
  annotations: {}
  ## @param arbiter.labels Annotations to be added to the Arbiter statefulset
  ##
  labels: {}
  ## @param arbiter.topologySpreadConstraints MongoDB(&reg;) Spread Constraints for arbiter Pods
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/
  ##
  topologySpreadConstraints: []
  ## @param arbiter.lifecycleHooks LifecycleHook for the Arbiter container to automate configuration before or after startup
  ##
  lifecycleHooks: {}
  ## @param arbiter.terminationGracePeriodSeconds Arbiter Termination Grace Period
  ##
  terminationGracePeriodSeconds: ""
  ## @param arbiter.updateStrategy.type Strategy that will be employed to update Pods in the StatefulSet
  ## ref: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#update-strategies
  ## updateStrategy:
  ##  type: RollingUpdate
  ##  rollingUpdate:
  ##    maxSurge: 25%
  ##    maxUnavailable: 25%
  ##
  updateStrategy:
    type: RollingUpdate
  ## @param arbiter.podManagementPolicy Pod management policy for MongoDB(&reg;)
  ## Should be initialized one by one when building the replicaset for the first time
  ##
  podManagementPolicy: OrderedReady
  ## @param arbiter.schedulerName Name of the scheduler (other than default) to dispatch pods
  ## ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
  ##
  schedulerName: ""
  ## @param arbiter.podAffinityPreset Arbiter Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
  ##
  podAffinityPreset: ""
  ## @param arbiter.podAntiAffinityPreset Arbiter Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
  ##
  podAntiAffinityPreset: soft
  ## Node affinity preset
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity
  ##
  nodeAffinityPreset:
    ## @param arbiter.nodeAffinityPreset.type Arbiter Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
    ##
    type: ""
    ## @param arbiter.nodeAffinityPreset.key Arbiter Node label key to match Ignored if `affinity` is set.
    ## E.g.
    ## key: "kubernetes.io/e2e-az-name"
    ##
    key: ""
    ## @param arbiter.nodeAffinityPreset.values Arbiter Node label values to match. Ignored if `affinity` is set.
    ## E.g.
    ## values:
    ##   - e2e-az1
    ##   - e2e-az2
    ##
    values: []
  ## @param arbiter.affinity Arbiter Affinity for pod assignment
  ## ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
  ## Note: arbiter.podAffinityPreset, arbiter.podAntiAffinityPreset, and arbiter.nodeAffinityPreset will be ignored when it's set
  ##
  affinity: {}
  ## @param arbiter.nodeSelector Arbiter Node labels for pod assignment
  ## ref: https://kubernetes.io/docs/user-guide/node-selection/
  ##
  nodeSelector: {}
  ## @param arbiter.tolerations Arbiter Tolerations for pod assignment
  ## ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
  ##
  tolerations: []
  ## @param arbiter.podLabels Arbiter pod labels
  ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
  ##
  podLabels: {}
  ## @param arbiter.podAnnotations Arbiter Pod annotations
  ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
  ##
  podAnnotations: {}
  ## @param arbiter.priorityClassName Name of the existing priority class to be used by Arbiter pod(s)
  ## ref: https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/
  ##
  priorityClassName: ""
  ## @param arbiter.runtimeClassName Name of the runtime class to be used by Arbiter pod(s)
  ## ref: https://kubernetes.io/docs/concepts/containers/runtime-class/
  ##
  runtimeClassName: ""
  ## MongoDB(&reg;) Arbiter pods' Security Context.
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod
  ## @param arbiter.podSecurityContext.enabled Enable Arbiter pod(s)' Security Context
  ## @param arbiter.podSecurityContext.fsGroup Group ID for the volumes of the Arbiter pod(s)
  ## @param arbiter.podSecurityContext.sysctls sysctl settings of the Arbiter pod(s)'
  ##
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    ## sysctl settings
    ## Example:
    ## sysctls:
    ## - name: net.core.somaxconn
    ##   value: "10000"
    ##
    sysctls: []
  ## MongoDB(&reg;) Arbiter containers' Security Context (only main container).
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container
  ## @param arbiter.containerSecurityContext.enabled Enable Arbiter container(s)' Security Context
  ## @param arbiter.containerSecurityContext.runAsUser User ID for the Arbiter container
  ## @param arbiter.containerSecurityContext.runAsNonRoot Set Arbiter containers' Security Context runAsNonRoot
  ##
  containerSecurityContext:
    enabled: true
    runAsUser: 1001
    runAsNonRoot: true
  ## MongoDB(&reg;) Arbiter containers' resource requests and limits.
  ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param arbiter.resources.limits The resources limits for Arbiter containers
  ## @param arbiter.resources.requests The requested resources for Arbiter containers
  ##
  resources:
    ## Example:
    ## limits:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    limits: {}
    ## Examples:
    ## requests:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    requests: {}
  ## @param arbiter.containerPorts.mongodb MongoDB(&reg;) arbiter container port
  ##
  containerPorts:
    mongodb: 27017
  ## MongoDB(&reg;) Arbiter pods' liveness probe. Evaluated as a template.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param arbiter.livenessProbe.enabled Enable livenessProbe
  ## @param arbiter.livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
  ## @param arbiter.livenessProbe.periodSeconds Period seconds for livenessProbe
  ## @param arbiter.livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
  ## @param arbiter.livenessProbe.failureThreshold Failure threshold for livenessProbe
  ## @param arbiter.livenessProbe.successThreshold Success threshold for livenessProbe
  ##
  livenessProbe:
    enabled: true
    initialDelaySeconds: 30
    periodSeconds: 20
    timeoutSeconds: 10
    failureThreshold: 6
    successThreshold: 1
  ## MongoDB(&reg;) Arbiter pods' readiness probe. Evaluated as a template.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param arbiter.readinessProbe.enabled Enable readinessProbe
  ## @param arbiter.readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
  ## @param arbiter.readinessProbe.periodSeconds Period seconds for readinessProbe
  ## @param arbiter.readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
  ## @param arbiter.readinessProbe.failureThreshold Failure threshold for readinessProbe
  ## @param arbiter.readinessProbe.successThreshold Success threshold for readinessProbe
  ##
  readinessProbe:
    enabled: true
    initialDelaySeconds: 5
    periodSeconds: 20
    timeoutSeconds: 10
    failureThreshold: 6
    successThreshold: 1
  ## MongoDB(&reg;) Arbiter pods' startup probe. Evaluated as a template.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param arbiter.startupProbe.enabled Enable startupProbe
  ## @param arbiter.startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
  ## @param arbiter.startupProbe.periodSeconds Period seconds for startupProbe
  ## @param arbiter.startupProbe.timeoutSeconds Timeout seconds for startupProbe
  ## @param arbiter.startupProbe.failureThreshold Failure threshold for startupProbe
  ## @param arbiter.startupProbe.successThreshold Success threshold for startupProbe
  ##
  startupProbe:
    enabled: false
    initialDelaySeconds: 5
    periodSeconds: 10
    timeoutSeconds: 5
    successThreshold: 1
    failureThreshold: 30
  ## @param arbiter.customLivenessProbe Override default liveness probe for Arbiter containers
  ## Ignored when arbiter.livenessProbe.enabled=true
  ##
  customLivenessProbe: {}
  ## @param arbiter.customReadinessProbe Override default readiness probe for Arbiter containers
  ## Ignored when arbiter.readinessProbe.enabled=true
  ##
  customReadinessProbe: {}
  ## @param arbiter.customStartupProbe Override default startup probe for Arbiter containers
  ## Ignored when arbiter.startupProbe.enabled=true
  ##
  customStartupProbe: {}
  ## @param arbiter.initContainers Add additional init containers for the Arbiter pod(s)
  ## Example:
  ## initContainers:
  ##   - name: your-image-name
  ##     image: your-image
  ##     imagePullPolicy: Always
  ##     ports:
  ##       - name: portname
  ##         containerPort: 1234
  ##
  initContainers: []
  ## @param arbiter.sidecars Add additional sidecar containers for the Arbiter pod(s)
  ## Example:
  ## sidecars:
  ##   - name: your-image-name
  ##     image: your-image
  ##     imagePullPolicy: Always
  ##     ports:
  ##       - name: portname
  ##         containerPort: 1234
  ##
  sidecars: []
  ## @param arbiter.extraVolumeMounts Optionally specify extra list of additional volumeMounts for the Arbiter container(s)
  ## Examples:
  ## extraVolumeMounts:
  ##   - name: extras
  ##     mountPath: /usr/share/extras
  ##     readOnly: true
  ##
  extraVolumeMounts: []
  ## @param arbiter.extraVolumes Optionally specify extra list of additional volumes to the Arbiter statefulset
  ## extraVolumes:
  ##   - name: extras
  ##     emptyDir: {}
  ##
  extraVolumes: []
  ## MongoDB(&reg;) Arbiter Pod Disruption Budget configuration
  ## ref: https://kubernetes.io/docs/tasks/run-application/configure-pdb/
  ##
  pdb:
    ## @param arbiter.pdb.create Enable/disable a Pod Disruption Budget creation for Arbiter pod(s)
    ##
    create: false
    ## @param arbiter.pdb.minAvailable Minimum number/percentage of Arbiter pods that should remain scheduled
    ##
    minAvailable: 1
    ## @param arbiter.pdb.maxUnavailable Maximum number/percentage of Arbiter pods that may be made unavailable
    ##
    maxUnavailable: ""
  ## MongoDB(&reg;) Arbiter service parameters
  ##
  service:
    ## @param arbiter.service.nameOverride The arbiter service name
    ##
    nameOverride: ""
    ## @param arbiter.service.ports.mongodb MongoDB(&reg;) service port
    ##
    ports:
      mongodb: 27017
    ## @param arbiter.service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
    ##
    extraPorts: []
    ## @param arbiter.service.annotations Provide any additional annotations that may be required
    ##
    annotations: {}

## @section Hidden Node parameters
##

hidden:
  ## @param hidden.enabled Enable deploying the hidden nodes
  ##   https://docs.mongodb.com/manual/tutorial/configure-a-hidden-replica-set-member/
  ##
  enabled: false
  ## @param hidden.hostAliases Add deployment host aliases
  ## https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/
  ##
  hostAliases: []
  ## @param hidden.configuration Hidden node configuration file to be used
  ##   http://docs.mongodb.org/manual/reference/configuration-options/
  ##
  configuration: ""
  ## @param hidden.existingConfigmap Name of existing ConfigMap with Hidden node configuration
  ## NOTE: When it's set the hidden.configuration parameter is ignored
  ##
  existingConfigmap: ""
  ## Command and args for running the container (set to default if not set). Use array form
  ## @param hidden.command Override default container command (useful when using custom images)
  ## @param hidden.args Override default container args (useful when using custom images)
  ##
  command: []
  args: []
  ## @param hidden.extraFlags Hidden node additional command line flags
  ## Example:
  ## extraFlags:
  ##  - "--wiredTigerCacheSizeGB=2"
  ##
  extraFlags: []
  ## @param hidden.extraEnvVars Extra environment variables to add to Hidden node pods
  ## E.g:
  ## extraEnvVars:
  ##   - name: FOO
  ##     value: BAR
  ##
  extraEnvVars: []
  ## @param hidden.extraEnvVarsCM Name of existing ConfigMap containing extra env vars
  ##
  extraEnvVarsCM: ""
  ## @param hidden.extraEnvVarsSecret Name of existing Secret containing extra env vars (in case of sensitive data)
  ##
  extraEnvVarsSecret: ""
  ## @param hidden.annotations Additional labels to be added to thehidden node statefulset
  ##
  annotations: {}
  ## @param hidden.labels Annotations to be added to the hidden node statefulset
  ##
  labels: {}
  ## @param hidden.topologySpreadConstraints MongoDB(&reg;) Spread Constraints for hidden Pods
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/
  ##
  topologySpreadConstraints: []
  ## @param hidden.lifecycleHooks LifecycleHook for the Hidden container to automate configuration before or after startup
  ##
  lifecycleHooks: {}
  ## @param hidden.replicaCount Number of hidden nodes (only when `architecture=replicaset`)
  ## Ignored when mongodb.architecture=standalone
  ##
  replicaCount: 1
  ## @param hidden.terminationGracePeriodSeconds Hidden Termination Grace Period
  ##
  terminationGracePeriodSeconds: ""
 ## @param hidden.updateStrategy.type Strategy that will be employed to update Pods in the StatefulSet
  ## ref: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#update-strategies
  ## updateStrategy:
  ##  type: RollingUpdate
  ##  rollingUpdate:
  ##    maxSurge: 25%
  ##    maxUnavailable: 25%
  ##
  updateStrategy:
    type: RollingUpdate
  ## @param hidden.podManagementPolicy Pod management policy for hidden node
  ##
  podManagementPolicy: OrderedReady
  ## @param hidden.schedulerName Name of the scheduler (other than default) to dispatch pods
  ## ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
  ##
  schedulerName: ""
  ## @param hidden.podAffinityPreset Hidden node Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
  ##
  podAffinityPreset: ""
  ## @param hidden.podAntiAffinityPreset Hidden node Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
  ##
  podAntiAffinityPreset: soft
  ## Node affinity preset
  ## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity
  ## Allowed values: soft, hard
  ##
  nodeAffinityPreset:
    ## @param hidden.nodeAffinityPreset.type Hidden Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
    ##
    type: ""
    ## @param hidden.nodeAffinityPreset.key Hidden Node label key to match Ignored if `affinity` is set.
    ## E.g.
    ## key: "kubernetes.io/e2e-az-name"
    ##
    key: ""
    ## @param hidden.nodeAffinityPreset.values Hidden Node label values to match. Ignored if `affinity` is set.
    ## E.g.
    ## values:
    ##   - e2e-az1
    ##   - e2e-az2
    ##
    values: []
  ## @param hidden.affinity Hidden node Affinity for pod assignment
  ## ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
  ## Note: podAffinityPreset, podAntiAffinityPreset, and nodeAffinityPreset will be ignored when it's set
  ##
  affinity: {}
  ## @param hidden.nodeSelector Hidden node Node labels for pod assignment
  ## ref: https://kubernetes.io/docs/user-guide/node-selection/
  ##
  nodeSelector: {}
  ## @param hidden.tolerations Hidden node Tolerations for pod assignment
  ## ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
  ##
  tolerations: []
  ## @param hidden.podLabels Hidden node pod labels
  ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
  ##
  podLabels: {}
  ## @param hidden.podAnnotations Hidden node Pod annotations
  ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
  ##
  podAnnotations: {}
  ## @param hidden.priorityClassName Name of the existing priority class to be used by hidden node pod(s)
  ## ref: https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/
  ##
  priorityClassName: ""
  ## @param hidden.runtimeClassName Name of the runtime class to be used by hidden node pod(s)
  ## ref: https://kubernetes.io/docs/concepts/containers/runtime-class/
  ##
  runtimeClassName: ""
  ## MongoDB(&reg;) Hidden pods' Security Context.
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod
  ## @param hidden.podSecurityContext.enabled Enable Hidden pod(s)' Security Context
  ## @param hidden.podSecurityContext.fsGroup Group ID for the volumes of the Hidden pod(s)
  ## @param hidden.podSecurityContext.sysctls sysctl settings of the Hidden pod(s)'
  ##
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    ## sysctl settings
    ## Example:
    ## sysctls:
    ## - name: net.core.somaxconn
    ##   value: "10000"
    ##
    sysctls: []
  ## MongoDB(&reg;) Hidden containers' Security Context (only main container).
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container
  ## @param hidden.containerSecurityContext.enabled Enable Hidden container(s)' Security Context
  ## @param hidden.containerSecurityContext.runAsUser User ID for the Hidden container
  ## @param hidden.containerSecurityContext.runAsNonRoot Set Hidden containers' Security Context runAsNonRoot
  ##
  containerSecurityContext:
    enabled: true
    runAsUser: 1001
    runAsNonRoot: true
  ## MongoDB(&reg;) Hidden containers' resource requests and limits.
  ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param hidden.resources.limits The resources limits for hidden node containers
  ## @param hidden.resources.requests The requested resources for hidden node containers
  ##
  resources:
    ## Example:
    ## limits:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    limits: {}
    ## Examples:
    ## requests:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    requests: {}
  ## @param hidden.containerPorts.mongodb MongoDB(&reg;) hidden container port
  containerPorts:
    mongodb: 27017
  ## MongoDB(&reg;) Hidden pods' liveness probe. Evaluated as a template.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param hidden.livenessProbe.enabled Enable livenessProbe
  ## @param hidden.livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
  ## @param hidden.livenessProbe.periodSeconds Period seconds for livenessProbe
  ## @param hidden.livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
  ## @param hidden.livenessProbe.failureThreshold Failure threshold for livenessProbe
  ## @param hidden.livenessProbe.successThreshold Success threshold for livenessProbe
  ##
  livenessProbe:
    enabled: true
    initialDelaySeconds: 30
    periodSeconds: 20
    timeoutSeconds: 10
    failureThreshold: 6
    successThreshold: 1
  ## MongoDB(&reg;) Hidden pods' readiness probe. Evaluated as a template.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param hidden.readinessProbe.enabled Enable readinessProbe
  ## @param hidden.readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
  ## @param hidden.readinessProbe.periodSeconds Period seconds for readinessProbe
  ## @param hidden.readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
  ## @param hidden.readinessProbe.failureThreshold Failure threshold for readinessProbe
  ## @param hidden.readinessProbe.successThreshold Success threshold for readinessProbe
  ##
  readinessProbe:
    enabled: true
    initialDelaySeconds: 5
    periodSeconds: 20
    timeoutSeconds: 10
    failureThreshold: 6
    successThreshold: 1
  ## Slow starting containers can be protected through startup probes
  ## Startup probes are available in Kubernetes version 1.16 and above
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-startup-probes
  ## @param hidden.startupProbe.enabled Enable startupProbe
  ## @param hidden.startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
  ## @param hidden.startupProbe.periodSeconds Period seconds for startupProbe
  ## @param hidden.startupProbe.timeoutSeconds Timeout seconds for startupProbe
  ## @param hidden.startupProbe.failureThreshold Failure threshold for startupProbe
  ## @param hidden.startupProbe.successThreshold Success threshold for startupProbe
  ##
  startupProbe:
    enabled: false
    initialDelaySeconds: 5
    periodSeconds: 10
    timeoutSeconds: 5
    successThreshold: 1
    failureThreshold: 30
  ## @param hidden.customLivenessProbe Override default liveness probe for hidden node containers
  ## Ignored when hidden.livenessProbe.enabled=true
  ##
  customLivenessProbe: {}
  ## @param hidden.customReadinessProbe Override default readiness probe for hidden node containers
  ## Ignored when hidden.readinessProbe.enabled=true
  ##
  customReadinessProbe: {}
  ## @param hidden.customStartupProbe Override default startup probe for MongoDB(&reg;) containers
  ## Ignored when hidden.startupProbe.enabled=true
  ##
  customStartupProbe: {}
  ## @param hidden.initContainers Add init containers to the MongoDB(&reg;) Hidden pods.
  ## Example:
  ## initContainers:
  ##   - name: your-image-name
  ##     image: your-image
  ##     imagePullPolicy: Always
  ##     ports:
  ##       - name: portname
  ##         containerPort: 1234
  ##
  initContainers: []
  ## @param hidden.sidecars Add additional sidecar containers for the hidden node pod(s)
  ## Example:
  ## sidecars:
  ##   - name: your-image-name
  ##     image: your-image
  ##     imagePullPolicy: Always
  ##     ports:
  ##       - name: portname
  ##         containerPort: 1234
  ##
  sidecars: []
  ## @param hidden.extraVolumeMounts Optionally specify extra list of additional volumeMounts for the hidden node container(s)
  ## Examples:
  ## extraVolumeMounts:
  ##   - name: extras
  ##     mountPath: /usr/share/extras
  ##     readOnly: true
  ##
  extraVolumeMounts: []
  ## @param hidden.extraVolumes Optionally specify extra list of additional volumes to the hidden node statefulset
  ## extraVolumes:
  ##   - name: extras
  ##     emptyDir: {}
  ##
  extraVolumes: []
  ## MongoDB(&reg;) Hidden Pod Disruption Budget configuration
  ## ref: https://kubernetes.io/docs/tasks/run-application/configure-pdb/
  ##
  pdb:
    ## @param hidden.pdb.create Enable/disable a Pod Disruption Budget creation for hidden node pod(s)
    ##
    create: false
    ## @param hidden.pdb.minAvailable Minimum number/percentage of hidden node pods that should remain scheduled
    ##
    minAvailable: 1
    ## @param hidden.pdb.maxUnavailable Maximum number/percentage of hidden node pods that may be made unavailable
    ##
    maxUnavailable: ""
  ## Enable persistence using Persistent Volume Claims
  ## ref: https://kubernetes.io/docs/user-guide/persistent-volumes/
  ##
  persistence:
    ## @param hidden.persistence.enabled Enable hidden node data persistence using PVC
    ##
    enabled: true
    ## @param hidden.persistence.medium Provide a medium for `emptyDir` volumes.
    ## Requires hidden.persistence.enabled: false
    ##
    medium: ""
    ## @param hidden.persistence.storageClass PVC Storage Class for hidden node data volume
    ## If defined, storageClassName: <storageClass>
    ## If set to "-", storageClassName: "", which disables dynamic provisioning
    ## If undefined (the default) or set to null, no storageClassName spec is
    ## set, choosing the default provisioner.
    ##
    storageClass: ""
    ## @param hidden.persistence.accessModes PV Access Mode
    ##
    accessModes:
      - ReadWriteOnce
    ## @param hidden.persistence.size PVC Storage Request for hidden node data volume
    ##
    size: 8Gi
    ## @param hidden.persistence.annotations PVC annotations
    ##
    annotations: {}
    ## @param hidden.persistence.mountPath The path the volume will be mounted at, useful when using different MongoDB(&reg;) images.
    ##
    mountPath: /bitnami/mongodb
    ## @param hidden.persistence.subPath The subdirectory of the volume to mount to, useful in dev environments
    ## and one PV for multiple services.
    ##
    subPath: ""
    ## Fine tuning for volumeClaimTemplates
    ##
    volumeClaimTemplates:
      ## @param hidden.persistence.volumeClaimTemplates.selector A label query over volumes to consider for binding (e.g. when using local volumes)
      ## See https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#labelselector-v1-meta for more details
      ##
      selector: {}
      ## @param hidden.persistence.volumeClaimTemplates.requests Custom PVC requests attributes
      ## Sometime cloud providers use additional requests attributes to provision custom storage instance
      ## See https://cloud.ibm.com/docs/containers?topic=containers-file_storage#file_dynamic_statefulset
      ##
      requests: {}
      ## @param hidden.persistence.volumeClaimTemplates.dataSource Set volumeClaimTemplate dataSource
      ##
      dataSource: {}
  service:
    ## @param hidden.service.portName MongoDB(&reg;) service port name
    ##
    portName: "mongodb"
    ## @param hidden.service.ports.mongodb MongoDB(&reg;) service port
    ##
    ports:
      mongodb: 27017
    ## @param hidden.service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
    ##
    extraPorts: []
    ## @param hidden.service.annotations Provide any additional annotations that may be required
    ##
    annotations: {}

## @section Metrics parameters
##

metrics:
  ## @param metrics.enabled Enable using a sidecar Prometheus exporter
  ##
  enabled: false
  ## Bitnami MongoDB(&reg;) Promtheus Exporter image
  ## ref: https://hub.docker.com/r/bitnami/mongodb-exporter/tags/
  ## @param metrics.image.registry MongoDB(&reg;) Prometheus exporter image registry
  ## @param metrics.image.repository MongoDB(&reg;) Prometheus exporter image repository
  ## @param metrics.image.tag MongoDB(&reg;) Prometheus exporter image tag (immutable tags are recommended)
  ## @param metrics.image.digest MongoDB(&reg;) image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
  ## @param metrics.image.pullPolicy MongoDB(&reg;) Prometheus exporter image pull policy
  ## @param metrics.image.pullSecrets Specify docker-registry secret names as an array
  ##
  image:
    registry: docker.io
    repository: bitnami/mongodb-exporter
    tag: 0.34.0-debian-11-r30
    digest: ""
    pullPolicy: IfNotPresent
    ## Optionally specify an array of imagePullSecrets.
    ## Secrets must be manually created in the namespace.
    ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    ## e.g:
    ## pullSecrets:
    ##   - myRegistryKeySecretName
    ##
    pullSecrets: []

  ## @param metrics.username String with username for the metrics exporter
  ## If undefined the root user will be used for the metrics exporter
  username: ""
  ## @param metrics.password String with password for the metrics exporter
  ## If undefined but metrics.username is defined, a random password will be generated
  password: ""
  ## @param metrics.extraFlags String with extra flags to the metrics exporter
  ## ref: https://github.com/percona/mongodb_exporter/blob/master/mongodb_exporter.go
  ##
  extraFlags: ""
  ## Command and args for running the container (set to default if not set). Use array form
  ## @param metrics.command Override default container command (useful when using custom images)
  ## @param metrics.args Override default container args (useful when using custom images)
  ##
  command: []
  args: []
  ## Metrics exporter container resource requests and limits
  ## ref: https://kubernetes.io/docs/user-guide/compute-resources/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param metrics.resources.limits The resources limits for Prometheus exporter containers
  ## @param metrics.resources.requests The requested resources for Prometheus exporter containers
  ##
  resources:
    ## Example:
    ## limits:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    limits: {}
    ## Examples:
    ## requests:
    ##    cpu: 100m
    ##    memory: 128Mi
    ##
    requests: {}
  ## @param metrics.containerPort Port of the Prometheus metrics container
  ##
  containerPort: 9216
  ## Prometheus Exporter service configuration
  ##
  service:
    ## @param metrics.service.annotations [object] Annotations for Prometheus Exporter pods. Evaluated as a template.
    ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
    ##
    annotations:
      prometheus.io/scrape: "true"
      prometheus.io/port: "{{ .Values.metrics.service.ports.metrics }}"
      prometheus.io/path: "/metrics"
    ## @param metrics.service.type Type of the Prometheus metrics service
    ##
    type: ClusterIP
    ## @param metrics.service.ports.metrics Port of the Prometheus metrics service
    ##
    ports:
      metrics: 9216
    ## @param metrics.service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
    ##
    extraPorts: []
  ## Metrics exporter liveness probe
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#configure-probes)
  ## @param metrics.livenessProbe.enabled Enable livenessProbe
  ## @param metrics.livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
  ## @param metrics.livenessProbe.periodSeconds Period seconds for livenessProbe
  ## @param metrics.livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
  ## @param metrics.livenessProbe.failureThreshold Failure threshold for livenessProbe
  ## @param metrics.livenessProbe.successThreshold Success threshold for livenessProbe
  ##
  livenessProbe:
    enabled: true
    initialDelaySeconds: 15
    periodSeconds: 5
    timeoutSeconds: 5
    failureThreshold: 3
    successThreshold: 1
  ## Metrics exporter readiness probe
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#configure-probes)
  ## @param metrics.readinessProbe.enabled Enable readinessProbe
  ## @param metrics.readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
  ## @param metrics.readinessProbe.periodSeconds Period seconds for readinessProbe
  ## @param metrics.readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
  ## @param metrics.readinessProbe.failureThreshold Failure threshold for readinessProbe
  ## @param metrics.readinessProbe.successThreshold Success threshold for readinessProbe
  ##
  readinessProbe:
    enabled: true
    initialDelaySeconds: 5
    periodSeconds: 5
    timeoutSeconds: 1
    failureThreshold: 3
    successThreshold: 1
  ## Slow starting containers can be protected through startup probes
  ## Startup probes are available in Kubernetes version 1.16 and above
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-startup-probes
  ## @param metrics.startupProbe.enabled Enable startupProbe
  ## @param metrics.startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
  ## @param metrics.startupProbe.periodSeconds Period seconds for startupProbe
  ## @param metrics.startupProbe.timeoutSeconds Timeout seconds for startupProbe
  ## @param metrics.startupProbe.failureThreshold Failure threshold for startupProbe
  ## @param metrics.startupProbe.successThreshold Success threshold for startupProbe
  ##
  startupProbe:
    enabled: false
    initialDelaySeconds: 5
    periodSeconds: 10
    timeoutSeconds: 5
    successThreshold: 1
    failureThreshold: 30
  ## @param metrics.customLivenessProbe Override default liveness probe for MongoDB(&reg;) containers
  ## Ignored when livenessProbe.enabled=true
  ##
  customLivenessProbe: {}
  ## @param metrics.customReadinessProbe Override default readiness probe for MongoDB(&reg;) containers
  ## Ignored when readinessProbe.enabled=true
  ##
  customReadinessProbe: {}
  ## @param metrics.customStartupProbe Override default startup probe for MongoDB(&reg;) containers
  ## Ignored when startupProbe.enabled=true
  ##
  customStartupProbe: {}
  ## Prometheus Service Monitor
  ## ref: https://github.com/coreos/prometheus-operator
  ##      https://github.com/coreos/prometheus-operator/blob/master/Documentation/api.md
  ##
  serviceMonitor:
    ## @param metrics.serviceMonitor.enabled Create ServiceMonitor Resource for scraping metrics using Prometheus Operator
    ##
    enabled: false
    ## @param metrics.serviceMonitor.namespace Namespace which Prometheus is running in
    ##
    namespace: ""
    ## @param metrics.serviceMonitor.interval Interval at which metrics should be scraped
    ##
    interval: 30s
    ## @param metrics.serviceMonitor.scrapeTimeout Specify the timeout after which the scrape is ended
    ## e.g:
    ## scrapeTimeout: 30s
    ##
    scrapeTimeout: ""
    ## @param metrics.serviceMonitor.relabelings RelabelConfigs to apply to samples before scraping.
    ##
    relabelings: []
    ## @param metrics.serviceMonitor.metricRelabelings MetricsRelabelConfigs to apply to samples before ingestion.
    ##
    metricRelabelings: []
    ## @param metrics.serviceMonitor.labels Used to pass Labels that are used by the Prometheus installed in your cluster to select Service Monitors to work with
    ## ref: https://github.com/coreos/prometheus-operator/blob/master/Documentation/api.md#prometheusspec
    ##
    labels: {}
    ## @param metrics.serviceMonitor.selector Prometheus instance selector labels
    ## ref: https://github.com/bitnami/charts/tree/main/bitnami/prometheus-operator#prometheus-configuration
    ##
    selector: {}
    ## @param metrics.serviceMonitor.honorLabels Specify honorLabels parameter to add the scrape endpoint
    ##
    honorLabels: false
    ## @param metrics.serviceMonitor.jobLabel The name of the label on the target service to use as the job name in prometheus.
    ##
    jobLabel: ""
  ## Custom PrometheusRule to be defined
  ## ref: https://github.com/coreos/prometheus-operator#customresourcedefinitions
  ##
  prometheusRule:
    ## @param metrics.prometheusRule.enabled Set this to true to create prometheusRules for Prometheus operator
    ##
    enabled: false
    ## @param metrics.prometheusRule.additionalLabels Additional labels that can be used so prometheusRules will be discovered by Prometheus
    ##
    additionalLabels: {}
    ## @param metrics.prometheusRule.namespace Namespace where prometheusRules resource should be created
    ##
    namespace: ""
    ## @param metrics.prometheusRule.rules Rules to be created, check values for an example
    ## ref: https://github.com/coreos/prometheus-operator/blob/master/Documentation/api.md#rulegroup
    ##      https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/
    ##
    ## This is an example of a rule, you should add the below code block under the "rules" param, removing the brackets
    ## rules:
    ## - alert: HighRequestLatency
    ##   expr: job:request_latency_seconds:mean5m{job="myjob"} > 0.5
    ##   for: 10m
    ##   labels:
    ##     severity: page
    ##   annotations:
    ##     summary: High request latency
    ##
    rules: []

```
