pipeline {
    agent any
    stages{
        stage("PROCESANDO APP NODE"){
            agent { 
                docker {
                    image "node:22"
                }
            }
            stages{
                stage('INICIO PIPELINE'){
                    steps {
                        sh 'echo "** INICIO PIPELINE **"'
                    }
                }
                stage('INSTALANDO DEPENDENCIAS'){
                    steps {
                        sh 'echo "** Instalando dependencias **"'
                        sh 'npm install'
                    }
                }
                stage('EJECUTAR LINT'){
                    steps {
                        sh 'echo "** Ejecutando lint **"'
                        sh 'npm run lint'
                    }
                }
                stage('EJECUTAR TESTING'){
                    steps {
                        sh 'echo "** Ejecutando test **"'
                        sh 'npm run test'
                    }
                }
                stage('EJECUTAR TESTING COVERAGE'){
                    steps {
                        sh 'echo "** Ejecutando coverage **"'
                        sh 'npm run test:cov'
                    }
                }
                stage('BUILD'){
                    steps {
                        sh 'echo "** Ejecutando build del codigo **"'
                        sh 'npm run build'
                    }
                }
            }
        }
        stage('BUILD DE IMAGEN DOCKER'){
            steps {
                sh 'docker build -t backend-node .'
                script {
                    docker.withRegistry("https://index.docker.io/v1/","credencial-dh"){
                        sh 'docker tag backend-node hugovaldes1995/curso-devops-lab-final'
                        sh "docker tag backend-node hugovaldes1995/curso-devops-lab-final:${env.BUILD_NUMBER}"
                        sh 'docker push hugovaldes1995/curso-devops-lab-final'
                        sh "docker push hugovaldes1995/curso-devops-lab-final:${env.BUILD_NUMBER}"
                    }
                    docker.withRegistry("https://ghcr.io","credencial-gh"){
                        sh 'docker tag backend-node ghcr.io/hugovaldes1993/backend-test'
                        sh "docker tag backend-node ghcr.io/hugovaldes1993/backend-test:${env.BUILD_NUMBER}"
                        sh 'docker push ghcr.io/hugovaldes1993/backend-test'
                        sh "docker push ghcr.io/hugovaldes1993/backend-test:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('DESPLIEGUE') {
            agent{
                docker{
                    image 'alpine/k8s:1.32.2'
                    reuseNode true
                }
            }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-docker']){
                    sh "kubectl -n hvaldes set image deployments taller-final-dp backend=ghcr.io/hugovaldes1993/backend-test:${env.BUILD_NUMBER}"
                }
            }
        }
        stage('FIN PIPILINE'){
            steps {
                echo '** Finalizando pipeline **'
            }
        }
    }
}