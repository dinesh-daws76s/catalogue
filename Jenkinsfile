pipeline {
    agent any
    environment {
          packageVersion = ''
          }

    stages {
        stage('getpackageversion') {
            steps {
               script {
                    // Read the JSON file
                    def packageJson = readJSON file: 'package.json'
                    
                    // Access the version value
                    packageVersion = packageJson.version
                    
                    // Print the version
                    echo "application version: $packageVersion"
                 }
              }
           }
        stage('Installing dependencies') {
            steps {
               sh """
                   npm install 
                   
               """
            }
        }
        stage('Build') {
             steps {
                sh """
                   ls -la
                   zip -r -q catalogue.zip ./* -x ".git" -x "*.zip"
                   ls -l
                """
            }
        }
    }
  post { 
    always {
        echo 'This will execute always when job executed'
        deleteDir()
    }
    success {
        sh """
          echo "JOB is successful"
        """
    }
    unsuccessful {
        echo 'This will execute at them time of job failed'
    }
  }
 }
