pipeline {
    agent any
    stages {
        stage('Run Python') {
            steps {
                bat 'docker run --rm python:3.10 python --version'
            }
        }
    }
}


    stages {
        stage('Install Dependencies') {
            steps {
                // Gunakan 'sh' karena di dalam container Python berbasis Linux
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
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425371020990353439/YE8JvbLU0G_K744mybErm_c7QGLFUhZ-_RZyWL_pA6NTvdI7e8Tf0g5olhc_YxyDdEQt'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425371020990353439/YE8JvbLU0G_K744mybErm_c7QGLFUhZ-_RZyWL_pA6NTvdI7e8Tf0g5olhc_YxyDdEQt'
                )
            }
        }
    }
}
