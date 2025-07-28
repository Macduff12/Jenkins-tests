pipeline {
    agent any

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to build')
        choice(name: 'BROWSER', choices: ['chromium', 'firefox', 'webkit'], description: 'Choose browser')
    }

    stages {
        stage('Checkout') {
            steps {
                // Виконуємо checkout вибраної гілки
                checkout([$class: 'GitSCM',
                    branches: [[name: "refs/heads/${params.BRANCH}"]],
                    userRemoteConfigs: [[url: 'git@github.com:your-org/your-repo.git']]
                ])
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm ci'
                sh 'npx playwright install'
            }
        }

        stage('Run tests') {
            steps {
                sh "npx playwright test --project=${params.BROWSER}"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
            junit 'playwright-report/**/*.xml'
        }
    }
}
