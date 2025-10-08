pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-v /var/jenkins_home:/var/jenkins_home'  
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'python --version'
            }
        }
    }

    post {
        success {
            script {
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    url: 'https://discord.com/api/webhooks/...',
                    requestBody: '{"content": "Build Success ✅"}'
                )
            }
        }
    }
}
