pipeline {
    agent any
    environment {
          packageVersion = ''
          NEXUS_URL = '3.90.153.181:8081'
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
                            nexusVersion: NEXUS3,
                            protocol: HTTP,
                            nexusUrl: ${NEXUS_URL},
                            groupId: com,
                            version: ${packageVersion},
                            repository: roboshop,
                            credentialsId: nexus_id,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: catalogue.zip,
                                classifier: '',
                                file: catalogue.zip,
                                type: zip],

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
