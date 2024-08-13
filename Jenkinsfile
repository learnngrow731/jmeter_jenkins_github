pipeline {
    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.4.1' // Adjust this path as necessary
        RESULTS_DIR = 'C:\\apache-jmeter-5.4.1\\bin\\PT Report' // Adjust this path as necessary
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
                    // Find all .jmx files in the repository
                    def testPlans = bat(script: 'dir /b /s *.jmx', returnStdout: true).trim().split('\n')
                    
                    // Loop through each .jmx file and execute
                    for (testPlan in testPlans) {
                        def safeTestPlan = testPlan.replaceAll('[<>:"/\\|?*]', '_') // Sanitize filename
                        def resultFile = "${env.RESULTS_DIR}\\${safeTestPlan}.jtl"
                        
                        echo "Running test plan: ${testPlan}"
                        bat "\"${env.JMETER_HOME}\\bin\\jmeter.bat\" -n -t \"${testPlan}\" -l \"${resultFile}\""
                    }
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
