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
        stage('Run JMeter Tests') {
            steps {
                bat """
                    jmeter -n -t src\\test\\jmeter\\test-plan.jmx -l src\\test\\jmeter\\results.jtl -e -o src\\test\\jmeter\\report -Jjmeter.save.saveservice.output_format=csv
                    """
            }
        }
        stage('Publish Results') {
            steps {
                publishHTML(target: [
                    reportDir: 'src\\test\\jmeter\\report',
                    reportFiles: 'index.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true
                ])
            }
        }
    }
}