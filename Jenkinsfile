pipeline {
    agent any
    stages{
        stage("Procesando aplicacion NodeJS"){
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
        // stage('Build docker image'){
        //     steps {
        //         sh 'docker build -t backend-node .'
        //         script {
        //             docker.withRegistry("https://index.docker.io/v1/","credencial-dh"){
        //                 sh 'docker tag backend-node carlosmarind/backend-node'
        //                 sh "docker tag backend-node carlosmarind/backend-node:${env.BUILD_NUMBER}"
        //                 sh 'docker push carlosmarind/backend-node'
        //                 sh "docker push carlosmarind/backend-node:${env.BUILD_NUMBER}"
        //             }
        //             docker.withRegistry("https://ghcr.io","credencial-gh"){
        //                 sh 'docker tag backend-node ghcr.io/carlosmarind/backend-node'
        //                 sh "docker tag backend-node ghcr.io/carlosmarind/backend-node:${env.BUILD_NUMBER}"
        //                 sh 'docker push ghcr.io/carlosmarind/backend-node'
        //                 sh "docker push ghcr.io/carlosmarind/backend-node:${env.BUILD_NUMBER}"
        //             }
        //         }
        //     }
        // }
        // stage('Despliegue continuo') {
        //     agent{
        //         docker{
        //             image 'alpine/k8s:1.32.2'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         withKubeConfig([credentialsId: 'kubeconfig-docker']){
        //              sh "kubectl -n devops set image deployments backend-dp backend=carlosmarind/backend-node:${env.BUILD_NUMBER}"
        //         }
        //     }
        // }
        stage('fin pipeline'){
            steps {
                echo 'finalizando pipeline'
            }
        }
    }
}