
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
        
        
    

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: jfrog
                )
            }
        }
        
        
        
    }
}
