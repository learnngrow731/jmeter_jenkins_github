pipeline {
    agent any
    stages {
        stage('Prepare Results Directory') {
            steps {
                bat 'if not exist "C:\\apache-jmeter-5.4.1\\bin\\PT_Report" mkdir "C:\\apache-jmeter-5.4.1\\bin\\PT_Report"'
            }
        }
        stage('Run JMeter Tests') {
            steps {
                script {
                    // Find the .jmx file
                    def jmxFile = bat(script: 'dir /b /s *.jmx', returnStdout: true).trim()
                    // Ensure the .jmx file exists
                    if (jmxFile) {
                        // Define the result file path
                        def resultFile = "C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results.jtl"
                        // Run the JMeter test
                        bat "\"C:\\apache-jmeter-5.4.1\\bin\\jmeter.bat\" -n -t \"${jmxFile}\" -l \"${resultFile}\""
                    } else {
                        error 'No .jmx file found'
                    }
                }
            }
        }
        stage('Publish Results') {
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Publishing results...'
                // Add your result publishing steps here
            }
        }
    }
    post {
        always {
            echo 'Build completed'
        }
    }
}
