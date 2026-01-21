pipeline {
    agent {
        docker {
            image 'playwright/chromium:playwright-1.56.1'
            args '--user=root --entrypoint=""'
        }
    }

    stages {
        stage('Préparation') {
            steps {
                // Vérifier les versions Node et Playwright
                sh 'node -v'
                sh 'npx playwright --version'
            }
        }

        stage('Installation des dépendances') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh 'npm install'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh 'npx playwright test --project=chromium'
                }
            }
        }
    }
}
