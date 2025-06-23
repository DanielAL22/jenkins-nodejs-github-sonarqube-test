pipeline {
    agent any

    options {
        // Permite abortar los builds anteriores si se lanza uno nuevo
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // variable automática que representa la configuración del repositorio definida en la interfaz del job de Jenkins. clona el repositorio con base en esa configuración
            }
        }

        stage('Test en múltiples versiones Node.js') {
            parallel {
                stage('Node 20') {
                    agent any
                    tools {
                        nodejs 'node20'  // Este nombre debe coincidir con el configurado en Jenkins
                    }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                    post {
                        success {
                            echo "✅ API funcional en Node 20"
                        }
                        failure {
                            echo "❌ Falla detectada en Node 20"
                        }
                    }
                }

                stage('Node 22') {
                    agent any
                    tools {
                        nodejs 'node22'
                    }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                    post {
                        success {
                            echo "✅ API funcional en Node 22"
                        }
                        failure {
                            echo "❌ Falla detectada en Node 22"
                        }
                    }
                }

                stage('Node 24') {
                    agent any
                    tools {
                        nodejs 'node24'
                    }
                    steps {
                        sh 'node -v'
                        sh 'npm ci'
                        sh 'npm test'
                    }
                    post {
                        success {
                            echo "✅ API funcional en Node 24"
                        }
                        failure {
                            echo "❌ Falla detectada en Node 24"
                        }
                    }
                }
            }
        }
    }
}