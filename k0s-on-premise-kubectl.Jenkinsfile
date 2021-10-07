pipeline {
  agent {
    kubernetes {
      label 'docker'  // all your pods will be named with this prefix, followed by a unique id
      // idleMinutes 10  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'docker'  // 'docker' or 'maven' define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage('Run with node docker') {    
      steps {
      container('docker') {
        echo 'Hello World'
        apt update
        apt install -y curl nano net-tools
        apt install -y supervisor
        curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.20.1/bin/linux/amd64/kubectl \
        && chmod +x ./kubectl \
        && mv ./kubectl /usr/local/bin/kubectl
        sh '''
        ls -la
        pwd
        '''
        withCredentials([file(credentialsId: 'kubeconfig-105', variable: 'FILE')]) {
           sh '''#!/bin/sh
           env
            // echo $FILE
           $FILE > kubeconfig.yaml
           cat kubeconfig.yaml   
           export KUBECONFIG=$FILE
           kubectl get all
           '''           
        } // Credential
      }  // container
      } //Steps
    } //stage
  } //stages
} // pipeline
