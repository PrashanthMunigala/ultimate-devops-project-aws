pipeline {
    agent any
    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Select the action')
    }
    stages {
        stage('git clone') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PrashanthMunigala/ultimate-devops-project-aws.git']])
            }
        }
        stage('build backend') {
            steps {
                dir('eksinstall/backend') {
                    sh 'terraform init'
                }
            }
        }
        stage('backend terraform apply or destroy') {
            steps {
                script {
                    if (params.ACTION == 'apply') {
                        dir('eksinstall/backend') {
                            sh 'terraform apply'
                        }
                    } else if (params.ACTION == 'destroy') {
                        dir('eksinstall/backend') {
                            sh 'terraform destroy'
                        }
                    }
                }
            }
        }
    }
}
