pipeline {
    agent any
    environment {
        IMAGE_NAME = "patitas-potosinas"
        CONTAINER_NAME = "app-patitas-prod"
    }
    stages {
        stage('1. Obtener Codigo Fuente') {
            steps {
                echo 'Descargando codigo actualizado desde GitHub...'
                checkout scm
            }
        }
        stage('2. Verificar y Probar Aplicacion') {
            steps {
                echo 'Verificando integridad de archivos web...'
                sh 'test -f index.html && test -f Dockerfile'
            }
        }
        stage('3. Construir Imagen Docker') {
            steps {
                echo 'Construyendo imagen Docker...'
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .'
            }
        }
        stage('4. Desplegar Contenedor') {
            steps {
                echo 'Desplegando nueva version del contenedor...'
                sh 'docker rm -f ${CONTAINER_NAME} || true'
                sh 'docker run -d -p 8082:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest'
            }
        }
    }
}
