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
                 
              sh "mvn clean -DskipTests -Darguments=-DskipTests release:clean release:prepare release:perform"
                
            }
        }
         
    }
}
