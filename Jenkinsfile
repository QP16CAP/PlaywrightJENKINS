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
        choice(name: 'tag', choices: ['invalide','valide'])
    }

    stages {

        stage('Préparation du projet') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'
                sh 'rm -rf repo'
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'

                dir('repo') {
                    sh 'npm ci'
                }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    // Nettoyage des anciens résultats
                    sh 'rm -rf allure-results'

                    script {
                        if (params.Navigateur == 'chromium') {
                            // Lancer les tests avec Allure
                            sh 'npx playwright test --project=chromium --reporter=allure-playwright'
                        } else {
                            error "Navigateur non valide sélectionné"
                        }
                    }
                }
            }
        }

        stage('Récupération des résultats') {
            steps {
                // Copier les résultats depuis le conteneur Docker vers l'hôte Jenkins
                sh 'cp -r repo/allure-results $WORKSPACE/allure-results'
            }
        }
    }

    post {
        always {
            // Générer le rapport Allure sur l'hôte Jenkins
            node {
                allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
            }
        }
    }
}
