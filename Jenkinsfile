pipeline {
    agent {
        label 'agent'
    }
    environment {
          packageVersion = ' '
          }

    stages {
        stage('getpackageversion') {
            steps {
               script {
                    // Read the JSON file
                    def package = readJSON file: 'package.json'
                    
                    // Access the version value
                    packageVersion = package.version
                    
                    // Print the version
                    echo "Version: ${packageVersion}"
            }
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
