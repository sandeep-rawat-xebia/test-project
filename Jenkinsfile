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
               sh "mvn test"
            }
        }
        
        stage('Run Test2') {
            steps {
               sh "pwd"
               sh "mvn test"
            }
        }
    }
}
