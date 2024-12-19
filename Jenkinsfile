pipeline {
    agent any  // Utiliser n'importe quel agent disponible

    triggers {
        // Déclenche la pipeline lorsque des commits sont poussés sur dev1 ou dev2
        githubPush()
    }

    environment {
        // Définir la branche principale comme cible pour les fusions
        MAIN_BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                // Vérifier le code source du dépôt
                checkout scm
            }
        }

        stage('Merge to main') {
            steps {
                script {
                    // Récupérer le nom de la branche actuelle
                    def branchName = env.BRANCH_NAME
                    echo "Branch name: ${branchName}"

                    // Vérifier si la branche actuelle est dev1 ou dev2
                    if (branchName == 'dev1' || branchName == 'dev2') {
                        // Fusionner automatiquement la branche dev1 ou dev2 vers main
                        echo "Merging ${branchName} into main"

                        sh """
                            # Se déplacer sur la branche main
                            git checkout ${MAIN_BRANCH}

                            # Récupérer les dernières modifications de main
                            git pull origin ${MAIN_BRANCH}

                            # Effectuer le merge de dev1 ou dev2 vers main
                            git merge ${branchName}

                            # Pousser les modifications sur la branche main
                            git push origin ${MAIN_BRANCH}
                        """
                    } else {
                        echo "No merge needed for this branch: ${branchName}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Nettoyer l'espace de travail après chaque exécution
            cleanWs()
        }
        success {
            // Actions en cas de succès (par exemple, notification)
            echo "Merge to main completed successfully!"
        }
        failure {
            // Actions en cas d'échec (par exemple, notification d'erreur)
            echo "Merge to main failed!"
        }
    }
}
