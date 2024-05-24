pipeline{
    agent any
    tools {
        maven 'maven'
    }
    stages {
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
                scanner = tool 'sonar'
            }
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh "${scanner}/bin/sonar-scanner -e -Dsonar.host.url=http://172.27.1.1:9000 -Dsonar.projectKey=Backend -Dsonar.java.binaries=target -Dsonar.exclusions=src/test/**"
                }

            }
        }
        stage('QualityGate'){
            steps {
                sleep(10)
                timeout(1){
                    waitForQualityGate abortPipeline:true, credentialsId: 'token_sonar'
                }
            }
        }
        stage('BuildImage'){
            steps {
                sh 'docker build -t backend .'
            }
        }
    }
}