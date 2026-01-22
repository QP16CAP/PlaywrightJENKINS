pipeline {
    agent {
        docker {
            image 'playwright/chromium:playwright-1.56.1'
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

                // Installer git
                sh 'apt-get update && apt-get install -y git'

                // Supprimer l’ancien repo
                sh 'rm -rf repo'

                // Cloner le repo
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                dir('repo') {
                    sh 'npm ci'

                    // ✅ AJOUT – Installation des navigateurs Playwright
                    //sh 'npx playwright install --with-deps'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    script {

                        // ✅ AJOUT – Nettoyage ancien rapport Allure
                        sh 'rm -rf allure-results'

                        if (params.Navigateur == 'chromium') {
                            sh 'npx playwright test --project=chromium'
                        } else {
                            error 'Navigateur non valide sélectionné'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            dir('repo') {

                // ✅ AJOUT – Publication du rapport Allure dans Jenkins
                allure(
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'allure-results']]
                )
            }
        }
    }
}
