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
        echo '---------------------------------------------------------------------------'
        echo 'Hello World with google Cloud'
        sh '''#!/bin/sh
        ls -la
        pwd
        apk update
        apk add  curl nano net-tools python bash
        apk add supervisor
        curl -sSL https://sdk.cloud.google.com | bash
        PATH $PATH:/root/google-cloud-sdk/bin

        '''
        withCredentials([file(credentialsId: 'secret-service-account-gcp', variable: 'FILE')]) {
           sh '''#!/bin/sh
           echo '---------------------------------------------------------------------------'
           // echo $FILE
           $FILE > kubeconfig.yaml
           cat kubeconfig.yaml   
           env
           '''           
        } // Credential
      }  // container
      } //Steps
    } //stage
  } //stages
} // pipeline
