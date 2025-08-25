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
                // Create a package from WAR (DAR not needed for this example)
                xldCreatePackage(
                     packageId: 'java-hello-world:1.0',
                     darPath: 'target/java-hello-world.war'
                )
            }
        }

        stage('Upload WAR to XL Deploy') {
            steps {
                // Publish the package to XL Deploy server
                xldPublishPackage(
                    packageId: 'java-hello-world:1.0',
                    darPath: 'target/java-hello-world.war'
                )
            }
        }

        stage('Trigger Deployment in XL Release') {
            steps {
                // Deploy the package to DEV environment
                xldDeploy(
                    packageId: 'java-hello-world:1.0',
                    environmentId: 'DEV'

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
