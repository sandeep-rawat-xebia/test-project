pipeline { 
    agent any  
    stages { 
        stage('Build') { 
            steps { 
               sh "mvn clean install"
            }
        }
        stage('Run Test') { 
            steps { 
               sh "mvn test"
            }
        }
    }
}
