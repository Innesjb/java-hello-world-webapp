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
                git branch: 'main', url: 'https://github.com/your-org/java-hello-world-webapp.git'
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
                curl -u $XLD_USER:$XLD_PASS -X POST \
                  -F "file=@target/java-hello-world-webapp.war" \
                  "$XLD_SERVER/deployit/repository/import"
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
