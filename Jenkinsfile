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
                bat 'if not exist "C:\\apache-jmeter-5.4.1\\bin\\PT_Report" mkdir "C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results_${env.BUILD_NUMBER}.jtl"'
            }
        }

         stage('Run JMeter Test') {
            steps {
                script {
                    // Define the path to the JMeter test file
                    def jmxFile = 'C:\\Users\\SHAKEEL\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\jmeter_jenkins_github_Pipeline\\test_plan.jmx'

                    // Use the Jenkins build number to create a unique result file name
                    def resultFile = "C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results_Build_${env.BUILD_NUMBER}.jtl"

                    // Check if the .jmx file exists before running the JMeter test
                    if (fileExists(jmxFile)) {
                        // Run the JMeter test and use the build number in the result file name
                        bat "\"C:\\apache-jmeter-5.4.1\\bin\\jmeter.bat\" -n -t \"${jmxFile}\" -l \"${resultFile}\""
                    } else {
                        error "JMeter test file ${jmxFile} does not exist"
                    }
                }
            }
        }

        stage('Publish Results') {
            steps {
                echo 'Publishing results...'
                // Archive the result file with the build number in its name
                archiveArtifacts artifacts: "C:\\apache-jmeter-5.4.1\\bin\\PT_Report\\results_Build_${env.BUILD_NUMBER}.jtl", allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
    }
}

