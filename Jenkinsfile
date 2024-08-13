pipeline {
    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.4.1' // Path to JMeter installation
        RESULTS_DIR = 'C:\\apache-jmeter-5.4.1\\bin\\PT_Report' // Directory to save results
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the repository
                git branch: 'master', url: 'https://github.com/learnngrow731/jmeter_jenkins_github.git'
            }
        }

        stage('Prepare Results Directory') {
            steps {
                // Create results directory if it does not exist
                bat 'if not exist "%RESULTS_DIR%" mkdir "%RESULTS_DIR%"'
            }
        }

        stage('Run JMeter Tests') {
            steps {
                script {
                    def jmxFile = bat(script: 'dir /b /s *.jmx', returnStdout: true).trim()
                    def resultFile = "C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results.jtl"
                    bat "\"C:\\apache-jmeter-5.4.1\\bin\\jmeter.bat\" -n -t \"${jmxFile}\" -l \"${resultFile}\""
                }
            }
        }

        stage('Publish Results') {
            steps {
                publishHTML(target: [
                    reportDir: "${env.RESULTS_DIR}",
                    reportFiles: 'index.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                ])
            }
        }
    }

    post {
        always {
            echo "Build completed"
        }
    }
}
