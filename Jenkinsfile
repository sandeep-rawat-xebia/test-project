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
                 
              sh "mvn release:prepare"
                
            }
        }
         
    }
}
