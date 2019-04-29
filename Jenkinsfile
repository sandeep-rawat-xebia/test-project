def TAG_SELECTOR = "UNINTIALIZED"
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
              script { if (env.RELEASE_TYPE == 'SNAPSHOT') {
                       sh "mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev}-SNAPSHOT versions:commit"
                       } else {
                        sh "mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev} versions:commit"
                       }
               }
            }
        }

        stage('Package') {
                    steps {
                       sh "zip -r dbscripts.zip SQLScripts"
                       sh "mvn clean install -DskipTests"
                          
                       script {
                             TAG_SELECTOR = readMavenPom().getVersion()
                        }
                       echo("TAG_SELECTOR=${TAG_SELECTOR}") 
                        
                        
                    }
                }

         stage('Push to Repository') {
                             steps {
                             sh "echo configure settings.xml"
                             //   sh "mvn deploy -DskipTests"
                }
          }  
    }
    
    post { 
        always { 
            MY_POM_VERSION=$(mvn -q -Dexec.executable="echo" -Dexec.args='${projects.version}' --non-recursive org.codehaus.mojo:exec-maven-plugin:1.3.1:exec)
        }
    }
    
}
