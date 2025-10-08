pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                branch pattern: "release/.*", comparator: "REGEXP"
            }
            steps {
                sh 'echo "Simulating deploy from branch ${env.BRANCH_NAME}"'
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on ${env.BRANCH_NAME}\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425371020990353439/YE8JvbLU0G_K744mybErm_c7QGLFUhZ-_RZyWL_pA6NTvdI7e8Tf0g5olhc_YxyDdEQt',
                    validResponseCodes: '200'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on ${env.BRANCH_NAME}\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425371020990353439/YE8JvbLU0G_K744mybErm_c7QGLFUhZ-_RZyWL_pA6NTvdI7e8Tf0g5olhc_YxyDdEQt',
                    validResponseCodes: '200'
                )
            }
        }
    }
}