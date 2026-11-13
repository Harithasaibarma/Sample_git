pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 10, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        stage('Setup') {
            steps {
                sh '''
                  python3 -m venv venv
                  . venv/bin/activate
                  pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                sh '. venv/bin/activate && pytest --junitxml=report.xml'
            }
            post {
                always { junit 'report.xml' }
            }
        }
        stage('Package') {
            steps {
                sh 'tar -czf app.tar.gz app.py'
                archiveArtifacts artifacts: 'app.tar.gz', fingerprint: true
            }
        }
        stage('Deploy') {
            when { branch 'main' }
            steps { echo 'Pretend deploying...' }
        }
    }

    post {
        success { echo 'Pipeline passed' }
        failure { echo 'Pipeline failed' }
        always  { cleanWs() }
    }
}
