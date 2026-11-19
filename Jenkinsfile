pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java21'
        maven 'Maven3'
    }
    stages {
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/YOUSSEFBLYO/register-app'
            }
        }

        /* Étapes commentées temporairement pour gagner du temps :
        stage("Build Application") {
            steps { sh "mvn clean package" }
        }
        stage("Test Application") {
            steps { sh "mvn test" }
        }
        */

                stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn compile org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                          -Dsonar.projectKey=register-app \
                          -Dsonar.projectName=register-app
                    '''
                }
            }
        }


        stage("Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
