pipeline {
    agent {
        label 'shir-build'
    }
    environment {
        SONAR_TOKEN = credentials('shir-git-hub')
    }
    stages {
        stage('Git Clone') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        stage('SAST') {
            environment {
                SONAR_SCANNER_VERSION = '4.7.0.2747'
                SONAR_SCANNER_HOME = "$HOME/.sonar/sonar-scanner-$SONAR_SCANNER_VERSION-linux"
                PATH = "$SONAR_SCANNER_HOME/bin:$PATH"
                SONAR_SCANNER_OPTS = '-server'
            }
            steps {
                sh 'curl --create-dirs -sSLo $HOME/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION-linux.zip'
                sh 'unzip -o $HOME/.sonar/sonar-scanner.zip -d $HOME/.sonar/'
                sh '''sonar-scanner \
                    -Dsonar.organization=shirlavi28 \
                    -Dsonar.projectKey=Shirlavi28_Shir-s-project- \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonarcloud.iosonar-scanner \'''
            }
        }

        stage('Build and Tag') {
            steps {
                script {
                    app = docker.build("shirlv66/snake:${env.BUILD_ID}")
                }
            }
        }
        stage('Image and Vulnerabilty Scan') {
            steps {
                script {
                    def imageId = "shirlv66/snake:${env.BUILD_ID}"
                    anchoreImageScan(imageId: imageId, failOnPolicy: true, vulnTypeFailThreshold: 10)
                }
            }
        }
        stage('Post to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','username-password-dockerhub') {
                        app.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Pull image Server') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
        stage('DAAST') {
            steps {
                arachniScanner(
                    url: 'http://63.34.64.229:8080',
                    checks: 'xss, sql_injection',
                    reportFilename: 'arachni_report.html'
                )
            }
        }
    }
}
