pipeline {
    agent any

    tools {
        // Ensure this NodeJS tool is configured in your Jenkins "Global Tool Configuration"
        nodejs 'NodeJS' 
    }

    environment {
        // Using the same credentials ID as the Maven project
        ARTIFACTORY_CREDS = credentials('7e59b761-7e86-402e-bc24-a194c787a656')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Generate Base64 auth token from username and password
                    // This is required for .npmrc _auth
                    def auth = java.util.Base64.getEncoder().encodeToString("${ARTIFACTORY_CREDS_USR}:${ARTIFACTORY_CREDS_PSW}".toString().getBytes('UTF-8'))
                    
                    // Inject the auth token into the environment for .npmrc to use
                    withEnv(["NPM_AUTH_TOKEN=${auth}"]) {
                        sh 'npm install'
                        // Verify that packages were resolved from JFrog
                        sh 'grep "resolved" package-lock.json || true'
                        sh 'npm start'
                    }
                }
            }
        }
    }
}
