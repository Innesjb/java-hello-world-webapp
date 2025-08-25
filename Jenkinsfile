pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven3', type: 'maven'
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
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
                    serverId: 'XLDeployServer', // Jenkins XL Deploy server ID
                    applicationName: 'java-hello-world',
                    version: '1.0'
                )
            }
        }

        stage('Upload WAR to XL Deploy') {
            steps {
                xldPublishPackage(
                    serverId: 'XLDeployServer', // Jenkins XL Deploy server ID
                    applicationName: 'java-hello-world',
                    version: '1.0',
                    artifactPath: 'target/java-hello-world.war'
                )
            }
        }

        stage('Trigger Deployment in XL Release') {
            steps {
                xldDeploy(
                    serverId: 'XLDeployServer',
                    environment: 'DEV',
                    applicationName: 'java-hello-world',
                    version: '1.0'
                )
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment succeeded!'
        }
        failure {
            echo '❌ Build or deployment failed.'
        }
    }
}

                   
