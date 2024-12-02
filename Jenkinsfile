pipeline {
    agent {
        label 'WindowsAgent'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Clean Report Directory') {
            steps {
                script {
                    def reportDir = 'src\\test\\jmeter\\report'
                    bat "if exist ${reportDir} rmdir /s /q ${reportDir}"
                    bat "mkdir ${reportDir}"
                }
            }
        }

        stage('Clean Results File') {
            steps {
                script {
                    def resultsFile = 'src\\test\\jmeter\\results.jtl'
                    bat "if exist ${resultsFile} del /f /q ${resultsFile}"
                }
            }
        }

        stage('Run JMeter Tests') {
            steps {
                bat """
                    jmeter -n -t src\\test\\jmeter\\test-plan.jmx -l src\\test\\jmeter\\results.jtl -e -o src\\test\\jmeter\\report -Jjmeter.save.saveservice.output_format=csv
                    """
            }
        }
        stage('Publish Results') {
            steps {
                script {
                    bat 'dir src\\test\\jmeter\\report'
                }
                publishHTML(target: [
                    reportDir: 'C:\\jenkins_agent\\workspace\\JMeter_performance\\src\\test\\jmeter\\report',
                    reportFiles: 'index.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true
                ])
            }
        }
    }
}