pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
          containers:
          - name: busybox
            image: busybox
            tty: true
        '''
    }
  }
  stages {
    stage('Run maven') {
      steps {
        echo 'Hello World'
        ls -la
        pwd
      }
    }
  }
}
