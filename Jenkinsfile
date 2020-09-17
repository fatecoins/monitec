pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        skipStagesAfterUnstable()
        disableConcurrentBuilds()
    }

    stages {
        stage('start') {
            steps {
                sh '''
                    ansible-playbook -i /etc/ansible/hosts -t start deploy.yml
                '''
            }
        }

        stage('pre-build') {
            steps {
                sh '''
                    ansible-playbook -i /etc/ansible/hosts -t prebuild deploy.yml
                '''
            }
        }

        stage('transfer') {
            steps {
                sh '''
                    ansible-playbook -i /etc/ansible/hosts -t transfer deploy.yml
                '''
            }
        }

        stage('install') {
            steps {
                sh '''
                    ansible-playbook -i /etc/ansible/hosts -t install deploy.yml
                '''
            }
        }

        stage('enable') {
            steps {
                sh '''
                    ansible-playbook -i /etc/ansible/hosts -t enable deploy.yml
                '''
            }
        }
    }

    post {
        always {
            emailext attachLog: false,
                     compressLog: false,
                     subject: "${currentBuild.currentResult}: ${currentBuild.fullDisplayName}",
                     body: "",
                     to: "groupmonitec@gmail.com"
        }
        success {
            deleteDir()
        }
    }
}