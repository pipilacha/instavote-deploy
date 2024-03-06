pipeline {
    agent any
    stages{
        stage('Update Git') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-mutlibranch-pipe01', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email pipilacha@email.com"
                            sh "git config user.name pipilacha"
                            sh "cat vote-ui-deployment.yaml"
                            sh "sed -i 's+pipilacha/vote.*+pipilacha/vote:${DOCKERTAG}+g' vote-ui-deployment.yaml"
                            sh "cat vote-ui-deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job deployment: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/instavote-deploy.git HEAD:main"
                } 
            }
        }
    }
    post{
        failure{
            slackSend (message: "Deploy failed: ${env.JOB_NAME} ${BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
        }
        success{
            slackSend (message: "Deploy sucess: ${env.JOB_NAME} ${BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
        }
    }    
}