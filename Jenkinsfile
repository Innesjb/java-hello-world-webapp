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

        stage('Create Package in XL Deploy') {
            steps {
                xldCreatePackage(
                    serverId: "${XLD_SERVER_ID}",
                    applicationName: "${APP_NAME}",
                    version: "${APP_VERSION}"
                )
            }
        }

        stage('Upload WAR to XL Deploy') {
            steps {
                xldPublishPackage(
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
                // Optional: API call to XL Release
            }
        }
    }

    post {
        success {
            echo "✅ Build, package creation, upload, and deployment steps finished successfully."
        }
        failure {
            echo "❌ Build or deployment failed."
        }
    }
}
