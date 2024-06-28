apipeline {
    agent any

    parameters {
        string(name: 'Pergunte_aqui', description: 'Insira a pergunta no campo abaixo.')
    }

    environment {
        PATH = "C:\\Windows\\System32;C:\\windows;C:\\windows\\Scripts;${env.PATH}"
    }

    stages {
        stage('Preparação do Ambiente') {
            steps {
                echo 'Instalando...'
            }
        }

        stage('Execução do Teste Levenshtein') {
            steps {
                bat 'python levenshtein_teste.py'
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
                    def pergunta = params.Pergunte_aqui
                    bat "python chat_bot.py \"${pergunta}\" > resposta.txt"
                }
            }
        }

        stage('Enviar Email') {
            steps {
                script {
                    def pergunta = params.Pergunte_aqui
                    def resposta = readFile('resposta.txt').trim()

                    emailext(
                        subject: "Pergunta e Resposta do Chatbot",
                        body: """<p>Pergunta: ${pergunta}</p>
                                 <p>Resposta: ${resposta}</p>""",
                        to: 'brendalauramarini@gmail.com',
                        mimeType: 'text/html'
                    )
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
