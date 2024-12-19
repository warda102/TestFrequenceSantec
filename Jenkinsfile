pipeline {
    agent any

    environment {
        TARGET_BRANCH = 'dev'  // La branche principale pour le merge
    }

    triggers {
        // Déclenchement sur un push dans le dépôt GitHub
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Vérifier le code source
                checkout scm
            }
        }

        stage('Merge Branches') {
            steps {
                script {
                    // Vérifier si la branche actuelle est `dev1` ou `dev2`
                    if (env.BRANCH_NAME == 'dev1' || env.BRANCH_NAME == 'dev2') {
                        echo "Merging branch '${env.BRANCH_NAME}' into '${env.TARGET_BRANCH}'"

                        // Exécuter les commandes Git pour le merge
                        sh """
                            # Configurer Git pour Jenkins
                            git config user.name "Jenkins"
                            git config user.email "jenkins@example.com"

                            # Passer sur la branche principale cible
                            git checkout ${env.TARGET_BRANCH}

                            # Mettre à jour la branche principale avec les derniers changements
                            git pull origin ${env.TARGET_BRANCH}

                            # Fusionner la branche actuelle dans la branche cible
                            git merge ${env.BRANCH_NAME} --no-edit

                            # Pousser les modifications sur la branche principale
                            git push origin ${env.TARGET_BRANCH}
                        """
                    } else {
                        echo "No merge required for branch: '${env.BRANCH_NAME}'"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Merge to '${env.TARGET_BRANCH}' completed successfully!"
        }
        failure {
            echo "Merge failed! Please check for conflicts or errors."
        }
        always {
            // Nettoyer l'espace de travail après chaque exécution
            cleanWs()
        }
    }
}
