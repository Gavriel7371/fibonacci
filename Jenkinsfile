pipeline {
    agent any

    parameters {
        string(name: 'number', defaultValue: '0', description: 'A numeric parameter')
    }

    environment {
        OUTPUT_FILE = 'output.html'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Gavriel7371/fibonacci.git'  // Replace with your repository URL
            }
        }

        stage('Run Shell Script') {
            steps {
                script {
                    def output = sh(script: "bash fibonacci.txt ${params.number}", returnStdout: true).trim()
                    writeFile file: OUTPUT_FILE, text: "<html><body><h1>Output</h1><p>${output}</p></body></html>"
                }
            }
        }

        stage('Display Parameter') {
            steps {
                script {
                    currentBuild.description = "Numeric parameter is ${params.number}"
                }
            }
        }

        stage('Verify Parameter on Web Page') {
            steps {
                script {
                    def description = currentBuild.description
                    if (description.contains("${params.number}")) {
                        echo "Parameter ${params.number} exists on the web page."
                    } else {
                        error "Parameter ${params.number} does not exist on the web page."
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: OUTPUT_FILE, fingerprint: true
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: OUTPUT_FILE,
                reportName: 'Shell Script Output'
            ])
        }
    }
}
