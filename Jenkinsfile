pipeline{
    agent any
     parameters {
       { choice(name: 'ACTION', choices: ['paln', 'apply', 'destroy'], description: '') }
       { string(name: 'build backend', defaultValue: 'init', description: 'terraform initialize') }
       { string(name: 'backend terraform apply', defaultValue: 'apply', description: 'terraform apply') }
       { string(name: 'backend terraform destroy', defaultValue: 'destroy', description: 'terraform destroy') }
    }
    stages{
        stage('git clone') { 
            steps {
                checkout scmGit(branches: [[name: '*/main]], extensions: [], userRemoteConfigs: [['url: 'https://github.com/PrashanthMunigala/ultimate-devops-project-aws.git']])
            } 
            }
        }
        stage('build backend') {
            steps {
                dir('eksinstall/backend')
                sh 'terraform init'
            }
        }
        stage('backend terraform apply') {
            when {ACTION 'apply'}
            steps {
                dir('eksinstall/backend')
                sh 'terraform apply'
            }
        }
         stage('backend terraform destroy') {
            when {ACTION 'destroy'}
            steps {
                dir('eksinstall/backend')
                sh 'terraform destroy'
            }
        }
        stage('Wait') {
            steps {
                script {
                    echo 'Waiting for 4 minutes before starting the next stage...'
                    sleep(time: 4, unit: 'MINUTES')
                }
            }
        }
        stage('Terraform Init') {
           steps {
              dir('eksinstall') {
                 sh 'terraform init'
              }

           }
        }
        stage('terraform plan') {
            when {ACTION 'plan'}
            steps {
                dir('eksinstall') {
                    sh 'terraform plan'
                }
            }
        }
        stage('terraform validate') {
            steps {
                dir('eksinstall') {
                    sh 'terraform validate'
                }
            }
        }
        stage('Terraform apply') {
            when {ACTION 'apply'}
            steps {
                dir ('eksinstall') {
                  sh 'terraform apply -auto-approve'
                }
            }
        }
        stage('terraform destroy') {
            when {ACTION 'destroy'}
            steps {
                dir ('eksinstall') {
                    sh 'terraform destroy -auto-approve'
                }
            }
        }

}
