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
                    sh "/opt/sonar-scanner/bin/sonar-scanner -Dsonar.host.url=http://192.168.112.128:9000 -Dsonar.projectName=vignesh-git-project -Dsonar.projectVersion=1.0 -Dsonar.projectKey=vignesh-git-project:app -Dsonar.sources=. -Dsonar.projectBaseDir=/var/lib/jenkins/workspace/onlinebookstore_J2EE"
                }
            }
        }
    }
}

