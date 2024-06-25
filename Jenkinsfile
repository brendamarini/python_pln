pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'python:3.9'
    }

    stages {
        stage('Preparação do Ambiente') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE).inside {
                        sh 'pip install -r requisitos.txt'
                    }
                }
            }
        }

        stage('Execução do Teste Levenshtein') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE).inside {
                        sh 'python3 levenshtein_teste.py'
                    }
                }
            }
        }

        stage('Verificação do Arquivo de Perguntas') {
            steps {
                script {
                    if (fileExists('perguntas.txt')) {
                        echo 'Arquivo perguntas.txt encontrado!'
                    } else {
                        error('Arquivo perguntas.txt não encontrado. Interrompendo o pipeline.')
                    }
                }
            }
        }

        stage('Execução do Chatbot') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE).inside {
                        sh 'python3 chat_bot.py'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline concluído.'
        }
        success {
            echo 'Pipeline executado com sucesso!'
        }
        failure {
            echo 'Pipeline falhou. Verificar logs para mais detalhes.'
        }
    }
}
