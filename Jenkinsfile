def AGENT_LABEL = null
def DOCKER_COMPOSE_NAME = "docker-compose -f ${env.COMPOSE_NAME}"
def GITLAB_NAME = "-----change to project name from Gitlab-----"

node('master') {
  stage('Checkout and set agent'){
     checkout scm
     if (env.ENVIRONMENT == 'dev') {
        AGENT_LABEL = "${env.JOB_NAME}-dev-slave"
        sh "echo ${AGENT_LABEL}"
    }

     if (env.ENVIRONMENT == "prod"){
         AGENT_LABEL = "${env.JOB_NAME}-prod-slave"
         sh "echo ${AGENT_LABEL}"
     }

    //  if(env.ENVIRONMENT == "testing"){
    //      AGENT_LABEL = "${env.JOB_NAME}-testing-slave"
    //      sh "echo ${AGENT_LABEL}"
    //  }
   }
}

pipeline { 
    agent {label "${AGENT_LABEL}"}
   
    stages {   
        stage('Docker Build') {
            steps {
                script {
                    if (env.PULLONLY == 'false') {
                        if (env.SERVICE == 'all') {
                            sh 'whoami'
                            sh 'git branch'
                            sh "echo ${env.SERVICE}"
                            sh "echo ${DOCKER_COMPOSE_NAME}"
                            sh "docker image prune -a -f"
                            sh "${DOCKER_COMPOSE_NAME} ps"
                            sh "${DOCKER_COMPOSE_NAME} build --no-cache"
                            sh "${DOCKER_COMPOSE_NAME} stop"
                            sh "${DOCKER_COMPOSE_NAME} up -d"
                        } else {
                            sh "echo ${env.SERVICE}"
                            sh "echo ${DOCKER_COMPOSE_NAME} 1"
                            sh "docker image prune -a -f"
                            sh "${DOCKER_COMPOSE_NAME} ps ${env.SERVICE}"
                            sh "${DOCKER_COMPOSE_NAME} build --no-cache ${env.SERVICE}"
                            sh "${DOCKER_COMPOSE_NAME} stop ${env.SERVICE}"
                            sh "${DOCKER_COMPOSE_NAME} up -d ${env.SERVICE}"
                        }
                    }
                } 

            }
     
        }
//Stage 2 

        // stage('Test2') {
        //     steps {
        //         sh 'ls -l'
        //         sh 'pwd'
        //     }
        // }

    } 


    

//Post Stage  
 
    post {
        
    

        success {
            updateGitlabCommitStatus name: "${GITLAB_NAME}", state: 'success'
            emailext attachLog: true, body: "Job ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL}) success", recipientProviders: [developers()], subject: 'Pipeline success'
            //step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: emailextrecipients([[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']])])

        }

        failure {
            updateGitlabCommitStatus name: "${GITLAB_NAME}", state: 'failed'
            emailext attachLog: true, body: "Job ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL}) failed", recipientProviders: [developers()], subject: 'Pipeline failed', to: 'ifriedland@ravtech.co.il'
            mail to: 'devops@ravtech.co.il', subject: 'Pipeline failed', body: "Job ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL}) failed"

        }

        unstable {
            updateGitlabCommitStatus name: "${GITLAB_NAME}", state: 'success'
            emailext attachLog: true, body: "Job ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL}) unstable", recipientProviders: [developers()], subject: 'Pipeline unstable'
        }
    }
}

