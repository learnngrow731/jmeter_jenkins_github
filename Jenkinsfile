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
                    // Find all .jmx files in the repository
                    def testPlans = bat(script: 'dir /b /s *.jmx', returnStdout: true).trim().split('\r?\n')

                    // Loop through each .jmx file and execute
                    for (testPlan in testPlans) {
                        def testPlanFile = testPlan.trim() // Clean up any extra whitespace
                        if (testPlanFile) {
                            // Create a safe filename for results
                            def safeTestPlan = testPlanFile.replaceAll('[<>:"/\\|?*]', '_') // Sanitize filename
                            def resultFile = "${env.RESULTS_DIR}\\${safeTestPlan}.jtl"

                            // Run JMeter test
                            echo "Running test plan: ${testPlanFile}"
                            bat """
                            "${env.JMETER_HOME}\\bin\\jmeter.bat" -n -t "${testPlanFile}" -l "${resultFile}"
                            """
                        }
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
