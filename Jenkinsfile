pipeline{
    agent any
    environment{
        dockerhubcred=credentials('dockerhubcred')
        awscred=credentials('awscred')
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
                sh 'echo $dockerhubcred_PSW | docker login -u $dockerhubcred_USR --password-stdin'
                sh 'docker push aniruddhaprojectes/php-project-jenkins:latest'

            }
        }
        stage('eks configure'){
            steps{
                sh 'aws eks update-kubeconfig --region us-east-2 --name java-project-eks-cluster'
            }
        }
        stage('k8s deploy dev'){
            steps{
                sh 'kubectl apply -f deployment-dev.yaml -n dev'
            }
        }
        stage('k8s deploy staging'){
            steps{
                sh 'kubectl apply -f deployment-staging.yaml -n staging'
            }
        }
        stage('k8s deploy prod'){
            steps{
                script{
                    def approval=input id: 'Productiondeploy', message: 'productiondeploy', submitter: 'admin'}

                sh 'kubectl apply -f deployment-prod.yaml -n production'
            }
        }
    }
}