pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.57.0-noble'
            args '--user=root --entrypoint=""'
        }
    }

    parameters {
        choice(
            name: 'Navigateur',
            choices: ['chromium', 'webkit', 'firefox'],
            description: 'Sélectionner un navigateur pour le test'
        )
    }

    stages {

        stage('Préparation du projet') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'

                sh 'apt-get update && apt-get install -y git'
                sh 'rm -rf repo'
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                dir('repo') {
                    sh 'npm ci'
                    // ⚠️ inutile avec l’image Playwright officielle
                    // sh 'npx playwright install --with-deps'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    sh 'rm -rf allure-results'

                    script {
                        if (params.Navigateur == 'chromium') {
                            sh 'npx playwright test --project=chromium'
                        } else if (params.Navigateur == 'firefox') {
                            sh 'npx playwright test --project=firefox'
                        } else if (params.Navigateur == 'webkit') {
                            sh 'npx playwright test --project=webkit'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            dir('repo') {
                allure(
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'allure-results']]
                )
            }
        }
    }
}
