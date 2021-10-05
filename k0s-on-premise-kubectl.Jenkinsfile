
pipeline {
  agent {
    kubernetes {
      // label 'promo-app'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'docker'  // 'docker' or 'maven' define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage('Run maven') {
      steps {
        echo 'Hello World'
        sh '''
        ls -la
        pwd
        '''
        withCredentials([file(credentialsId: 'kubeconfig-105', variable: 'FILE')]) {
           sh 'use $FILE'
        } // Credential
      } //Steps
    } //stage
  } //stages
} // pipeline
