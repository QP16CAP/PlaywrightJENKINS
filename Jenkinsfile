pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.57.0-noble'
            args '--user=root --entrypoint=""'
        }
    }

    parameters {
        choice(name: 'Navigateur', choices: ['chromium', 'webkit', 'firefox'], description: 'Sélectionner un navigateur')
        choice(name: 'tag', choices:['invalide','valide'])
    }

    stages {
        stage('Préparation du projet') {
            steps {
                sh 'node -v'
                sh 'npx playwright --version'
                sh 'rm -rf repo'
                sh 'git clone https://github.com/QP16CAP/PlaywrightJENKINS.git repo'
                dir('repo') { sh 'npm ci' }
            }
        }

        stage('Exécution des tests Playwright') {
            steps {
                dir('repo') {
                    sh 'rm -rf allure-results'
                    script {
                        if (params.Navigateur == 'chromium') {
                            sh 'npx playwright test --project=chromium --reporter=allure-playwright'
                        } else {
                            error 'Navigateur non valide sélectionné'
                        }
                    }
                }
            }
        }

        stage('Récupération des résultats') {
            steps {
                // Copier les résultats depuis le conteneur vers l'hôte
                sh 'cp -r repo/allure-results $WORKSPACE/allure-results'
            }
        }
    }

    post {
        always {
            // Génération du rapport Allure sur l'hôte Jenkins
            node {
                // Cette étape se fait sur l'hôte, pas dans le conteneur
                allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
            }
        }
    }
}
