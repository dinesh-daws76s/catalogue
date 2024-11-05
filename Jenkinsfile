pipeline {
    agent {
        label 'agent'
    }
    environment {
          NAME = 'dinesh babu Ramayanam'
          }

    stages {
        stage('build') {
            steps {
                echo 'This is build 2'
            }
        }
        stage('Configure') {
            steps {
                echo 'This is build 2'
            }
        }
        stage('deploy') {
             steps {
                sh """
                   echo "This stage is Deployment stage"
                """
            }
        }
    }
  post { 
    always {
        echo 'This will execute always when job executed'
    }
    success {
        sh """
          echo "My name is $NAME"
        """
    }
    unsuccessful {
        echo 'This will execute at them time of job failed'
    }
  }
 }
