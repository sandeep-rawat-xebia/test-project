pipeline { 
    agent any  
    tools{
        maven "maven3.3.9"
        jdk "java8"
    }
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
