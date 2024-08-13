pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/learnngrow731/jmeter_jenkins_github.git']]])
            }
        }

        stage('Prepare Results Directory') {
            steps {
                // Create the directory for JMeter results if it does not exist
                bat 'if not exist "C:\\apache-jmeter-5.4.1\\bin\\PT_Report" mkdir "C:\\apache-jmeter-5.4.1\\bin\\PT_Report"'
            }
        }

        stage('Run JMeter Test') {
            steps {
                script {
                    // Define the path to the JMeter test file and result file
                    def jmxFile = 'C:\\Users\\SHAKEEL\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\jmeter_jenkins_github_Pipeline\\test_plan.jmx'
                    def resultFile = 'C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results.jtl'

                    // Verify if the .jmx file exists
                    if (fileExists(jmxFile)) {
                        // Run the JMeter test
                        bat "\"C:\\apache-jmeter-5.4.1\\bin\\jmeter.bat\" -n -t \"${jmxFile}\" -l \"${resultFile}\""
                    } else {
                        error "JMeter test file ${jmxFile} does not exist"
                    }
                }
            }
        }

        stage('Publish Results') {
            steps {
                // Publish the results if required (e.g., archive results, generate reports)
                echo 'Publishing results...'
                // Example: archive the JMeter results
                archiveArtifacts artifacts: 'C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results.jtl', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Actions that should always run after the pipeline completes
            echo 'Build completed'
        }
    }
}
