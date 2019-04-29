pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
               build "Build Project"
            }
        }
        stage('Run Test') {
            steps {
                sshagent (['my-ssh-key']) {
              sh "mvn release:prepare"
                }
            }
        }
         
    }
}
