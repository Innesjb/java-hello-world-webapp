pipeline {
    agent any

    environment {
        XLD_SERVER_ID = 'XLDeployServer' // XL Deploy server configured in Jenkins
        APP_NAME      = 'java-hello-world'
        APP_VERSION   = '1.0'
        WAR_FILE      = 'target/java-hello-world.war'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Innesjb/java-hello-world-webapp.git',
                    credentialsId: 'git-credentials'
            }
        }

        stage('Build WAR with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Create CI Artifact in XL Deploy') {
            steps {
                xldeployArtifactCreate(
                    serverId: "${XLD_SERVER_ID}",
                    applicationName: "${APP_NAME}",
                    version: "${APP_VERSION}",
                    description: "CI artifact created via Jenkins"
                )
            }
        }

        stage('Upload WAR to XL Deploy') {
            steps {
                xldeployArtifactUpload(
                    serverId: "${XLD_SERVER_ID}",
                    applicationName: "${APP_NAME}",
                    version: "${APP_VERSION}",
                    artifactPath: "${WAR_FILE}"
                )
            }
        }

        stage('Trigger Deployment in XL Release') {
            steps {
                echo "Triggering XL Release pipeline for ${APP_NAME} version ${APP_VERSION}"
                // Optional: API call to XL Release can go here
            }
        }
    }

    post {
        success {
            echo "✅ Build, CI artifact creation, upload, and deployment steps finished successfully."
        }
        failure {
            echo "❌ Build or deployment failed."
        }
    }
}

      
       
       
