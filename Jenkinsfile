pipeline {
    agent { label "jenkins-agent" }
    environment {
              APP_NAME = "app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/hamzaBKR/gitops_app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "hamzabaker"
                   git config --global user.email "hamzabaker03101998@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated My Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]) {
                  sh "git push https://github.com/hamzaBKR/gitops_app.gi"
                }
            }
        }
      
    }
}
