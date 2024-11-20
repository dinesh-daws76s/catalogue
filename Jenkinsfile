pipeline {
    agent any
    environment {
          packageVersion = ''
          NEXUS_URL = '18.215.235.108:8081'
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
        stage('Pushing Artifacts to Nexus') {
             steps {
               nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: "${NEXUS_URL}",
                            groupId: 'com.roboshop',
                            version: "${packageVersion}",
                            repository: 'catalogue',
                            credentialsId: 'nexus_agent',
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: 'catalogue',
                                classifier: '',
                                file: 'catalogue.zip',
                                type: 'zip'],
                        ]
                  )
            }
        }
        stage ('Deploy-artifact') {
            steps {
                build(job: 'catalogue-Deploy',  wait: true, parameters: [
                    
                    string(name: 'version', value: "${packageVersion}"),
                    string(name: 'environment', value: "test")
                
                    ]
               )
            }
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
