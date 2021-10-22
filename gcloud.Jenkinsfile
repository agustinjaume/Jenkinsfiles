pipeline {
  agent {
    kubernetes {
      label 'docker'  // all your pods will be named with this prefix, followed by a unique id
      // idleMinutes 10  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'google-cloud-sdk-alpine'  // 'docker' or 'maven' define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage('Run with node docker') {    
      steps {
      container('google-cloud-sdk-alpine') {
        echo '---------------------------------------------------------------------------'
        echo 'Hello World with google Cloud'

        sh '''#!/bin/sh
        ls -la
        pwd
        apk update
        apk add  curl nano net-tools bash wget
        apk add supervisor
        echo '---------------------------------------------------------------------------'
        echo 'google Cloud Version'
        gcloud version
        env
        '''
        withCredentials([file(credentialsId: 'secret-service-account-gcp', variable: 'FILE')]) {
           sh '''#!/bin/sh
           echo '---------------------------------------------------------------------------'
           echo $FILE > config.json
           cat config.json   
           export GOOGLE_APPLICATION_CREDENTIALS=$FILE
           gcloud auth activate-service-account  sa-jenkins@project-ideasextraordinarias.iam.gserviceaccount.com --key-file=$FILE
           echo '---------------------------------------------------------------------------'
           gsutil ls
           echo '---------------------------------------------------------------------------'
           gcloud projects list
           echo '---------------------------------------------------------------------------'
           '''           
        } // Credential
      }  // container
      } //Steps
    } //stage
  } //stages
} // pipeline
