pipeline {
    agent any  // Utiliser n'importe quel agent disponible

    triggers {
        // Déclenche la pipeline lorsque des commits sont poussés sur dev1 ou dev2
        githubPush()
    }

    environment {
        // Définir des variables d'environnement si nécessaire
        GIT_BRANCH_DEV = 'dev'
    }

    stages {
        stage('Checkout') {
            steps {
                // Vérifier le code source du dépôt
                checkout scm
            }
        }

        stage('Merge to dev') {
            steps {
                script {
                    // Récupérer le nom de la branche actuelle
                    def branchName = env.BRANCH_NAME
                    
                    // Vérifier si la branche actuelle est dev1 ou dev2
                    if (branchName == 'dev1' || branchName == 'dev2') {
                        // Fusionner automatiquement la branche dev1 ou dev2 vers dev
                        echo "Merging ${branchName} into dev"

                        sh """
                            # Se déplacer sur la branche dev
                            git checkout dev

                            # Récupérer les dernières modifications de dev
                            git pull origin dev

                            # Effectuer le merge de dev1 ou dev2 vers dev
                            git merge ${branchName}

                            # Pousser les modifications sur la branche dev
                            git push origin dev
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
            echo "Merge completed successfully!"
        }
        failure {
            // Actions en cas d'échec (par exemple, notification d'erreur)
            echo "Merge failed!"
        }
    }
}
