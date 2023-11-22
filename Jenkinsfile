pipeline {
    agent any
    
    stages {
        stage("checkout") {
            steps {
              sh "ls"
              git branch: 'main', url: 'https://github.com/patrickdiligentforgerock/course3-jenkins-gs-spring-petclinic.git'
              sh "ls"
            }
        }
        
        stage("build") {
            steps {
                sh "./mvnw package"
            }
        }
        
        stage("parallel") {
            parallel {
                stage("A") {
                    steps {
                        sh "echo test set A"
                        sleep 2
                        sleep 3
                    }
                }
                stage("B") {
                     steps {
                        sh "echo test set B"
                        sleep 4
                     }
                }
                stage("C") {
                    steps {
                        sleep 1
                        //sh "false"
                    }
                }
            }
            failFast true
        }
        
        stage("capture") {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
                junit '**/target/surefire-reports/TEST*.xml'
            }
        }
    }

    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
                to: 'patrick@example.com',
                recipientProviders: [previous()], 
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.Build_NUMBER}]"
        }
    }
}