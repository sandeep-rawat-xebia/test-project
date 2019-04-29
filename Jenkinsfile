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
                sh "pwd"
               sh "mvn release:prepare"
            }
        }
         
    }
}
