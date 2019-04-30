def VERSION_NUMBER = new Date().format("yyyyMMdd.HHmmss")

pipeline {
    agent any

    parameters {
    	string ( defaultValue: "SNAPSHOT",description: 'Upload this version to repository?',name : 'RELEASE_TYPE')
        string ( defaultValue: VERSION_NUMBER ,description: 'Upload this version to repository?',name : 'VERSION_NUMBER')
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
                                       sh "cp dbscripts.zip build/dbscripts.zip"
                                       sh "cp deployit-manifest.xml build/deployit-manifest.xml"
                                       xldCreatePackage artifactsPath: '.', manifestPath: 'deployit-manifest.xml', darPath: '$JOB_NAME-$BUILD_NUMBER.0.dar'
                          }
                    }


    }
    
   
    
}
