pipeline {
    agent any
    stages { 
        stage('Example') {
            steps {
                echo 'Hello World'
                echo sh(returnStdout: true, script: 'env')
            }
        }
    }
}
