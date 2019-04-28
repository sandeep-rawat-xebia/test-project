pipeline { 
    agent any  
    tools {
    maven "apache-maven-3.1.0"
    jdk "default"
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
