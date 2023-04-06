pipeline{
    agent any

    tools {
        maven 'Maven_3.9'       
    }

    stages{
        stage('SCM Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git_Token', url: 'https://github.com/sasi0616/java-maven-app.git']])
            }
            
        }
        stage('mavenbuild'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('sonarscanner'){
               steps{
                    withSonarQubeEnv('SonarQube') {
                         sh "${tool("SonarQube")}/bin/sonar-scanner \
                        -Dsonar.projectKey=java-maven-app \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://52.3.226.23:9000 \
                        -Dsonar.login=sqp_0391bfb3db9b19f71c46191427c6267a90dab838"
                    }
               }
              
        }
        stage('nexus-upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }

}