pipeline {
    agent any

    options {
        disableConcurrentBuilds() // Permite abortar los builds anteriores si se lanza uno nuevo
    }

    environment {
        SONARQUBE_SERVER = 'Sonar 9.9.8'    // El nombre del servidor configurado en Jenkins > Configure System > SonarQube servers
        SONAR_TOKEN = credentials('sonar-token-3')    // Token configurado en Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // variable automática que representa la configuración del repositorio definida en la interfaz del job de Jenkins. clona el repositorio con base en esa configuración
            }
        }
		
        stage('DP Check') {
            steps {
                dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'DP-Check'
            }
            post {
                always {
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                    archiveArtifacts artifacts: '**/dependency-check-report.html', onlyIfSuccessful: true
                }
            }
        }


        stage('Test en múltiples versiones Node.js') {
            parallel {
                stage('Node 20') {
                    agent any
                    tools { nodejs 'node20' }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                }

                stage('Node 22') {
                    agent any
                    tools { nodejs 'node22' }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                }

                stage('Node 24') {
                    agent any
                    tools { nodejs 'node24' }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Análisis SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=realestate-api \
                        -Dsonar.projectName="RealEstate API" \
                        -Dsonar.sources=src \
                        -Dsonar.language=js \
                        -Dsonar.sourceEncoding=UTF-8 \
                        -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completado correctamente"
        }
        failure {
            echo "💥 El pipeline falló"
        }
    }
}