podTemplate(label: 'jenkins-mvn-test', 
containers: [
    containerTemplate(name: 'maven', image: 'maven:3-jdk-8-alpine', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker:1.12', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:v2.7.0', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.9.3', command: 'cat', ttyEnabled: true)
],
volumes:[
    hostPathVolume(mountPath: '/var/run/docker.sock'  , hostPath: '/var/run/docker.sock'),
    hostPathVolume(mountPath: '/root/.m2/repository'     , hostPath: '/root/.m2/repository')
],idleMinutes: 10
){

  node ('jenkins-mvn-test') {

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
        sh 'echo ${env.BUILD_NUMBER}'
        sh 'pwd'
        sh 'rm -rf ./target'
        sh 'ls -al'
      }
    }
  }
}