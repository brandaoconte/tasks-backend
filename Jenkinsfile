pipeline{
    agent any
    tools {
        maven 'maven'
    }
        stages{
            stage('Build'){
                steps{
                    sh '''
                    mvn clean package -DskipTests
                    '''   
            }        
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }
        stage('sonar'){
            environment{
                scanner = tools 'sonar_scanner'
            }
            steps{
                withSonarQubeEnvi('sonarqube'){
                sh "${scanner}/bin/sonar-scanner -e -Dsonar.host.url=http://http://172.27.1.1:9000 -Dsonar.projectKey=Backend -Dsonar.java.binaries=target -Dsonar.exclusions=src/test/**"

            }
            
            }
        } 
    }
}