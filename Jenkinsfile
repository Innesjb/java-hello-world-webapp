stage('Create Package in XL Deploy') {
    steps {
        xldCreatePackage(
            applicationName: 'java-hello-world',  // mandatory
            version: '1.0',                        // mandatory
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
            serverCredentials: 'XLDeployServer'   // reference your Jenkins XL Deploy credentials
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
