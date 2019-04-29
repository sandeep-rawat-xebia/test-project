pipeline {
    agent any

    parameters {
    	string (
    	  defaultValue: "SNAPSHOT",description: 'Upload this version to repository?',name : 'RELEASE_TYPE')
      }

    stages {
        stage('Run Test Cases') {
            steps {
               sh "mvn test"
            }
        }
        stage('Set Version') {
            steps {

              script { if (RELEASE_TYPE == 'SNAPSHOT') {
                       sh 'mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev}-SNAPSHOT versions:commit'
                       } else {
                        sh 'mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev} versions:commit'
                                  }
                      }

                
            }
        }

        stage('Build') {
                    steps {
                       sh "mvn clean install"
                    }
                }
         
    }
}
