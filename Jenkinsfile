pipeline {
    agent any
    environment{
        PATH = "C:\\Windows\\System32;C:\\Users\\Marlon\\AppData\\Local\\Programs\\Python\\Python312;C:\\Users\\Marlom\\AppData\\Local\\Progras\\Python\\Python312\\Scripts;${env.PATH}"
    }
    stages {
        stage('Preparação do Ambiente') {
            steps {
                sh 'pip install -r requisitos.txt'
            }
        }

        stage('Execução do Teste Levenshtein') {
            steps {
                sh 'python levenshtein_teste.py'
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
                sh 'python chat_bot.py'
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
