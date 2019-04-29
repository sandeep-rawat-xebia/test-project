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
                sshagent (['sandeep-rawat-git']) {
              sh "mvn release:prepare"
                }
            }
        }
         
    }
}
