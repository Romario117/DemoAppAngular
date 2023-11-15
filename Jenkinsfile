pipeline {
    agent {
        docker {
            // Utiliza una imagen de Node.js como base
            image 'node:18'
        }
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                // Instala las dependencias del proyecto Angular
                sh 'npm install'
            }
        }

        stage('Construir la aplicación') {
            steps {
                // Compila la aplicación Angular
                sh 'npm run build --prod'
            }
        }

        stage('Construir la imagen Docker') {
            steps {
                // Construye la imagen Docker
                script {
                    docker.build('my-angular-app')
                }
            }
        }

        stage('Desplegar en Docker') {
            steps {
                // Despliega la aplicación en un contenedor Docker
                script {
                    docker.image('my-angular-app').withRun('-p 8080:80')
                }
            }
        }
    }

    post {
        success {
            echo 'La aplicación se construyó y desplegó correctamente.'
        }

        failure {
            echo 'Hubo un error en la construcción o despliegue de la aplicación.'
        }
    }
}
