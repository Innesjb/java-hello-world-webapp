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
                    applicationName: 'java-hello-world',
                    version: '1.0',
                    darPath: 'target/java-hello-world.war'
                )
            }
        }

        stage('Publish Package to XL Deploy') {
            steps {
                xldPublishPackage(
                    applicationName: 'java-hello-world',
                    version: '1.0',
                    darPath: 'target/java-hello-world.war',
                    serverCredentials: 'XLDeployServer'
                )
            }
        }

        stage('Deploy to Environment') {
            steps {
                xldDeploy(
                    applicationName: 'java-hello-world',
                    version: '1.0',
                    environment: 'DEV',
                    serverCredentials: 'XLDeployServer'
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

        
