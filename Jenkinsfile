pipeline {
    agent { label 'Jenkins-Agent' }
    
    environment {
        APP_NAME = "spring-pet-2"
        IMAGE_TAG = "1.0.${BUILD_NUMBER}"
    }
    
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/tobaaalo/spring-pet-2.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i "s|tobaalo/spring-petclinic-project:.*|tobaalo/spring-petclinic-project:${IMAGE_TAG}|g" deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "tobaaalo"
                   git config --global user.email "tobanehemiah@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/tobaaalo/spring-pet-2.git main"
                }
            }
        }
    }
}
