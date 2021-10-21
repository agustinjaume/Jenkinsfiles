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
        apk add  curl nano net-tools bash wget
        apk add supervisor
        export CLOUD_SDK_VERSION=308.0.0   
        export PATH="/google-cloud-sdk/bin:$PATH"
        echo $PATH
        apk --no-cache add curl python py-crcmod bash libc6-compat && \
        curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
        tar xzf google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
        rm google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
        ln -s /lib /lib64 && \
        gcloud config set core/disable_usage_reporting true && \
        gcloud config set component_manager/disable_update_check true && \
        gcloud config set metrics/environment github_docker_image
        echo '---------------------------------------------------------------------------'
        echo 'google Cloud Version'
        gcloud --version
        env

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
