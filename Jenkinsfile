pipeline {
    agent any
    environment {
                scannerHome = tool 'sonar-scanner' 
            }
    

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "https://github.com/Arun2089/jenkins-task2.git"
            }
        }

        stage('Sonar Analysis') {
           
            steps {
                withSonarQubeEnv('sonar-server') { 
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Deployment-Docker-image"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qualityGate = waitForQualityGate()

                    if (qualityGate.status == 'OK') {
                        
                        slackSend(channel: 'U07CGJ071V2', color: 'good', message: 'Quality gate passed View report at http://43.204.110.177:9000/dashboard?id=Sonar_qube')
                        build job: 'Deployment', wait: true
                        
                    } else {
                        
                        
                        slackSend(channel: 'U07CGJ071V2', color: 'danger', message: 'Quality gate failed View report at http://43.204.110.177:9000/dashboard?id=Sonar_qube')
                        error "Quality gate failed, aborting pipeline."
                    }
                }
            }
        }
    }
}
