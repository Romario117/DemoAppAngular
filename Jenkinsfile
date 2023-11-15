pipeline {
  agent any

  tools {
    nodejs "NodeJS"
  }

  parameters {
    string(name: 'container_name', defaultValue: 'pagina_web', description: 'Nombre del contenedor de docker.')
    string(name: 'image_name', defaultValue: 'pagina_img', description: 'Nombre de la imagen docker.')
    string(name: 'tag_image', defaultValue: 'lts', description: 'Tag de la imagen de la página.')
    string(name: 'container_port', defaultValue: '80', description: 'Puerto que usa el contenedor')
  }

  stages {
    stage('install') {
      steps {
        git branch: 'master', url: 'https://github.com/Romario117/DemoAppAngular.git'
        dir('/') {
          sh 'npm install'
        }
      }
    }

    stage('build') {
      steps {
        dir('/') {
          script {
            try {
              sh 'docker stop ${container_name}'
              sh 'docker rm ${container_name}'
              sh 'docker rmi ${image_name}:${tag_image}'
            } catch (Exception e) {
              error 'Error al detener y eliminar contenedores antiguos o imágenes. Detalles: ' + e.toString()
            }
          }
          sh 'npm run build'
          sh 'docker build -t ${image_name}:${tag_image} .'
        }
      }
    }

    stage('deploy') {
      steps {
        sh 'docker run -d -p ${container_port}:80 --name ${container_name} ${image_name}:${tag_image}'
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
