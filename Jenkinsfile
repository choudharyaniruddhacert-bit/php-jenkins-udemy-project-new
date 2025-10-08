pipeline{
    agent any
    environment{
        dockerhubcred=credentials('dockerhubcred')
    }
    stages{
        stage('code checkout'){
            steps{
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/choudharyaniruddhacert-bit/php-jenkins-udemy-project-new.git']])
            }
        }
        stage('run composer install'){
            steps{
                sh 'composer install'
                sh 'composer show'
                sh 'ls -a'
            }
        }
        stage('docker build stage'){
            steps{
                sh 'docker build -t aniruddhaprojectes/php-project-jenkins .'
                sh 'docker images'
            }
        }
        stage('docker push stage'){
            steps{
                sh 'echo %dockerhubcred_PSW | docker login -u $dockerhubcred_USR --password-stdin'
                sh 'docker push aniruddhaprojectes/php-project-jenkins:latest'

            }
        }
    }
}