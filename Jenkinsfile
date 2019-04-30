pipeline {
    agent any

    parameters {
    	string ( defaultValue: "SNAPSHOT",description: 'Release type SNAPSHOT or Release',name : 'RELEASE_TYPE')
        string ( defaultValue: "DEFAULT" ,description: 'Version Number to use?',name : 'VERSION_NUMBER')
      }

      environment {
      		PATH = '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Wireshark.app/Contents/MacOS:/Users/sandeep/.rvm/bin'
      	}

    stages {

        stage('Run Test Cases') {
            steps {
               sh "mvn test"
            }
        }

        stage('Set Version') {
            steps {
              script {
                      MY_VERSION_NUMBER = env.VERSION_NUMBER
                      if(MY_VERSION_NUMBER == 'DEFAULT'){
                        MY_VERSION_NUMBER = new Date().format("yyyyMMdd.HHmmss")
                      }
                      if (env.RELEASE_TYPE == 'SNAPSHOT') {
                       sh "mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev}-SNAPSHOT versions:commit"
                       } else {
                        sh "mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=\\\${git.commit.time}.\\\${git.commit.id.abbrev} versions:commit"
                       }
                      sh "echo ${VERSION_NUMBER}"
               }
            }
        }

        stage('Package') {
                    steps {
                       sh "zip -r dbscripts.zip SQLScripts"
                       sh "mvn clean install -DskipTests"
                    }
                }

         stage('Push to Repository') {
                             steps {
                             sh "echo configure settings.xml"
                             //   sh "mvn deploy -DskipTests"
                }
          }


          stage('Push to XL Deploy') {
                                       steps {
                                       sh "rm -rf build && mkdir build"
                                       sh "cp target/SampleWe* build/test-project.ear"
                                       sh "cp dbscripts.zip build/sqlscripts.zip"
                                       sh "cp deployit-manifest.xml build/deployit-manifest.xml"
                                       xldCreatePackage artifactsPath: '.', manifestPath: 'deployit-manifest.xml', darPath: 'build'
                                       xldPublishPackage serverCredentials: 'xldeploy', darPath: 'build/test.dar'
                          }
                    }


    }
    
   
    
}
