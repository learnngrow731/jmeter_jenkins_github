pipeline {
    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.4.1\\bin' // Update this path to your JMeter installation
        RESULTS_DIR = 'C:\\apache-jmeter-5.4.1\\bin\\PT Report' // Update this path to where you want to store results
        TEST_PLANS_DIR = '' // Leave empty to search entire repository or specify directory
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the repository
                git branch: 'master', url: 'https://github.com/learnngrow731/jmeter_jenkins_github.git'
            }
        }

        stage('Run JMeter Tests') {
            steps {
                script {
                    // Create results directory if it does not exist
                    bat "if not exist \"${env.RESULTS_DIR}\" mkdir \"${env.RESULTS_DIR}\""
                    
                    // If TEST_PLANS_DIR is not set, search the entire repository
                    def searchDir = env.TEST_PLANS_DIR ? env.TEST_PLANS_DIR : '.'
                    
                    // Find all .jmx files in the repository
                    def testPlans = bat(script: "dir /b /s ${searchDir}\\*.jmx", returnStdout: true).trim().split('\n')
                    
                    // Loop through each .jmx file and execute
                    for (testPlan in testPlans) {
                        def safeTestPlan = testPlan.replaceAll('[<>:"/\\|?*]', '_') // Sanitize filename
                        def resultFile = "${env.RESULTS_DIR}\\${safeTestPlan}.jtl"
                        
                        echo "Running test plan: ${testPlan}"
                        bat "\"${env.JMETER_HOME}\\bin\\jmeter\" -n -t \"${testPlan}\" -l \"${resultFile}\""
                    }
                }
            }
        }

        stage('Publish Results') {
            steps {
                // Publish JMeter test results if available
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
            // Any post-build actions can go here
            echo "Build completed"
        }
    }
}