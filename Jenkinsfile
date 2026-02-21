pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'Java21'
    }

    environment {
       APP_NAME = "devops-application"
       RELEASE = "1.0.${env.BUILD_NUMBER}"
       DOCKER_USER = "ayselguliyeva2025"
       DOCKER_PASS = "TOKEN_DOCKER_ID"
       IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
       IMAGE_TAG = "${IMAGE_NAME}:${RELEASE}"
    }
    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo "Build url: ${BUILD_URL}"
                echo "BUILD_TAG: ${BUILD_TAG}"
                echo "BUILD_ID: ${BUILD_ID}"
            }
        }

        stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ayselguliyeva2025/devops_004_pipeline']])
            }
        }

        stage('Build Maven') {
            steps {
                script {
                    if (isUnix()) {
                        sh "mvn clean install"
                    } else {
                        bat "mvn clean install"
                    }
                }
            }
        }

        stage('Test Maven') {
            steps {
                script {
                    if (isUnix()) {
                        sh "mvn test"   // Linux ve MacOS için
                    } else {
                        bat "mvn test"  // Windows için
                    }
                }
            }
        }

        stage('SonarQube') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'TOKEN_SONARQUBE_ID') {
                        if (isUnix()) {
                            sh "mvn sonar:sonar"
                        } else {
                            bat "mvn sonar:sonar"
                        }
                    }
                }
            }
        }

         stage('Docker Image Build & Push') {
                    steps {
                         script {
                         docker.withRegistry('', DOCKER_PASS) {
                         dockerImage = docker.build(IMAGE_TAG)
                         dockerImage.push(IMAGE_TAG)
                            }
                        }
                    }
              }


        /*
        stage('Docker Image') {
            steps {
                 script {
                    if (isUnix()) {
                        sh 'docker build  -t  ayselguliyeva2025/devops-application:latest  .'
                    } else {
                        bat 'docker build  -t  ayselguliyeva2025/devops-application:latest  .'
                    }
                }
            }
        }

        stage('DockerHub') {
            steps {
                echo "Image DockerHub'a gönder."
                 script {
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {


                            if (isUnix()) {
                             //   sh 'docker login    -u ayselguliyeva2025     -p   %dockerhub%'
                                sh 'docker push ayselguliyeva2025/devops-application:latest'
                            } else {
                             //    bat 'docker login    -u ayselguliyeva2025     -p   %dockerhub%'
                                 bat 'docker push ayselguliyeva2025/devops-application:latest'
                            }
                        }
                 }

            }
        }

        stage('Kubernetes (K8s)') {
            steps {
                 script {
                      kubernetesDeploy (configs: 'deployment-service.yaml',  kubeconfigId: 'kubernetes')
                     echo "K8s içinde image'ı çalıştır."
                 }

            }
        }

       stage('Clean') {
            steps {

                script {
                    if (isUnix()) {
                        sh "docker image prune -f"
                    } else {
                        bat "docker image prune -f"
                    }
                }
                echo "Makinemdeki fazlalık imageları temizle."
            }
        }

     */
    }
}