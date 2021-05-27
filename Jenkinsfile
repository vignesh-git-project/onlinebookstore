pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn clean compile'
                    
                }
            }
        }
                
        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn test'
                }
            }
        }


        stage ('Install Stage') {
            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn install'
                }
            }
        }
    
       stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar"
                }
            }
        }
        
        
       stage ('upload') {
    gitlabCommitStatus("upload") {
      def server = Artifactory.server "admin@jfrog"
      def buildInfo = Artifactory.newBuildInfo()
      buildInfo.env.capture = true
      buildInfo.env.collect()

      def uploadSpec = """{
        "files": [
          {
            "pattern": "**/target/*.jar",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.pom",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.war",
            "target": "libs-snapshot-local"
          }
                ]
                         }"""
      // Upload to Artifactory.
      server.upload spec: uploadSpec, buildInfo: buildInfo

      buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
      // Publish build info.
      server.publishBuildInfo buildInfo
                                }
                        }



        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: jfrog
                )
                  }
                                     }
        
        
        
    }
}

