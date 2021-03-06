pipeline {
    agent any

    parameters {
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
                      MAVEN_VERSION = env.VERSION_NUMBER
                      if(MAVEN_VERSION == 'DEFAULT'){
                        MAVEN_VERSION = new Date().format("yyyyMMdd.HHmmss")+"-SNAPSHOT"
                      }
                      sh "mvn pl.project13.maven:git-commit-id-plugin:2.2.4:revision -DdateFormat=yyyyMMdd-HHmmss  versions:set -DnewVersion=${MAVEN_VERSION} versions:commit"
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
                             // sh "mvn deploy -DskipTests"
                }
          }


          stage('Push to XL Deploy') {
                                       steps {
                                       script {
                                        MAVEN_VERSION = readMavenPom().getVersion()
                                       }
                                       sh "sed -i -e 's/PACKAGE_VERSION/${MAVEN_VERSION}/g' deployit-manifest.xml"
                                       sh "cp target/SampleWe* target/test-project.ear"
                                       sh "cp dbscripts.zip target/sqlscripts.zip"
                                       xldCreatePackage artifactsPath: 'target', manifestPath: 'deployit-manifest.xml', darPath: 'target/xldeploy.dar'
                                       xldPublishPackage serverCredentials: 'xldeploy', darPath: 'target/xldeploy.dar'
                          }
                    }


    }
    
   
    
}
