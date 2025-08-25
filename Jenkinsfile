pipeline {
    agent any

    environment {
        XLD_SERVER = 'http://192.168.127.131:4516'
        XLD_USER   = 'admin'
        XLD_PASS   = '1234'
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

        stage('Upload WAR to XL Deploy') {
            steps {
                sh '''
                    curl -u $XLD_USER:$XLD_PASS \
                         -X PUT \
                         -F "file=@target/java-hello-world.war" \
                         "$XLD_SERVER/deployit/repository/ci/com/example/java-hello-world.war/1.0"
                '''
            }
        }

        stage('Trigger Deployment in XL Release') {
            steps {
                echo 'Triggering XL Release pipeline (can add API call here)...'
            }
        }
    }

    post {
        success { echo "✅ Build and deployment steps finished." }
        failure { echo "❌ Build failed." }
    }
}

       
       
