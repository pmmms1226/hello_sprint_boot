podTemplate(label: 'jenkins-mvn', 
// containers: [
//     containerTemplate(name: 'maven', image: 'maven:3-jdk-8-alpine', command: 'cat', ttyEnabled: true),
//     containerTemplate(name: 'docker', image: 'docker:1.12', command: 'cat', ttyEnabled: true),
//     containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:v2.7.0', command: 'cat', ttyEnabled: true),
//     containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.9.3', command: 'cat', ttyEnabled: true)
// ],
volumes:[
    hostPathVolume(mountPath: '/var/run/docker.sock'  , hostPath: '/var/run/docker.sock'),
    hostPathVolume(mountPath: '/root/.m2/repository'     , hostPath: '/root/.m2/repository')
    // Warning  FailedMount            24s (x2 over 2m)  ibm/ibmc-block          Error while attaching the device pv pvc-5a57e682-877b-11e8-9eda-12f85c7c1293 cannot be attached to the node 10.178.188.28. Error: PV pvc-5a57e682-877b-11e8-9eda-12f85c7c1293 is already attached to another node 10.178.188.4
    // Warning  FailedMount            18s (x2 over 2m)  kubelet, 10.178.188.28  Unable to mount volumes for pod "jenkins-slave-t0jb4-3gbbc_jenkins(88d91a76-8f23-11e8-8bb2-2aef178dbc0e)": timeout expired waiting for volumes to attach/mount for pod "jenkins"/"jenkins-slave-t0jb4-3gbbc". list of unattached/unmounted volumes=[volume-1]
    // persistentVolumeClaim(mountPath: '/home/jenkins/.m2', claimName: 'pvc-jenkins', readOnly: false)
]){

  node ('jenkins-mvn') {

    // User Custom Setting ////////////////////////////////////////////////////////////////////////////////////////////////
    def DEPLOY_TARGET = "alpha"
    def K8S_NAMESPACE = "serverless-console"

    def STAGE_EXECUTE_maven           = "true"
    def STAGE_EXECUTE_docker          = "true"
    def STAGE_EXECUTE_kubectl_config  = "true"
    def STAGE_EXECUTE_helm            = "true"
    def STAGE_EXECUTE_kubectl_apply   = "true"

    def SERVICE_NAME_serverless_console_backend = "serverless-console-backend"
    def SERVICE_NAME_pgbackend = "pgbackend"
    def IMAGE_VERSION = "0.0.1"
    def SERVICE_HOST = "alpha.action.cloudz.co.kr"
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


    // Default Setting
    // https://github.com/settings/tokens
    def KUBECONFIG_CONFIG_TOKEN = "f7efe9350775a502c9d76e7d751ef46c1547c30f"
    def KUBECONFIG_CONFIG_URL = "https://raw.githubusercontent.com/cloudsvcdev/kubeconfig/master/config"

    def KUBECONFIG_PEM_URL_DEV = "https://raw.githubusercontent.com/cloudsvcdev/kubeconfig/master/ca-seo01-zaction-dev.pem"
    def KUBECONFIG_PEM_FILENAME_DEV = "ca-seo01-zaction-dev.pem"
    
    def KUBECONFIG_PEM_URL_ALPHA = "https://raw.githubusercontent.com/cloudsvcdev/kubeconfig/master/ca-seo01-zaction-alpha.pem"
    def KUBECONFIG_PEM_FILENAME_ALPHA = "ca-seo01-zaction-alpha.pem"

    def KUBECONFIG_PEM_URL_BETA = "https://raw.githubusercontent.com/cloudsvcdev/kubeconfig/master/ca-seo01-zaction-beta.pem"
    def KUBECONFIG_PEM_FILENAME_BETA = "ca-seo01-zaction-beta.pem"

    def KUBECONFIG_PEM_URL_PROD = "https://raw.githubusercontent.com/cloudsvcdev/kubeconfig/master/ca-seo01-zaction-prod.pem"
    def KUBECONFIG_PEM_FILENAME_PROD = "ca-seo01-zaction-prod.pem"

    def KUBECONFIG_PEM_URL = KUBECONFIG_PEM_URL_ALPHA
    def KUBECONFIG_PEM_FILENAME = KUBECONFIG_PEM_FILENAME_ALPHA

    def DOCKER_REGISTRY_URL = "harbor.dev.action.cloudz.co.kr"
    def DOCKER_USERNAME = "admin"
    def DOCKER_PASSWORD = "!Cloudev00"

    // checkout sources
    checkout scm



    stage ('Maven Build') {
      container('maven') {
        if("${STAGE_EXECUTE_maven}" == "true"){
          println "Maven Build Started"
          sh "mvn clean package"
        }else{
          println "Maven Build Passed"
        }
        sh 'pwd'
        sh 'ls -al'
      }
    }
  }
}