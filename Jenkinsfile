pipeline {
    agent {
        docker {
            image 'python:3.9'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Preparação do Ambiente') {
            steps {
                sh 'pip install -r requisitos.txt'
            }
        }

        stage('Execução do Teste Levenshtein') {
            steps {
                sh 'python3 levenshtein_teste.py'
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
                sh 'python3 chat_bot.py'
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
