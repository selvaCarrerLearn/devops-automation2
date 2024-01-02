pipeline {
    agent any
    tools{
        maven 'maven-3.8.4'
    }
    stages{
        stage('Build Maven'){
            steps{
                  checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/selvaCarrerLearn/devops-automation2']]])
                   bat 'mvn clean install'
                  }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t selvakumarsmddocker/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   bat 'docker login -u selvakumarsmddocker -p ${dockerhubpwd}'

}
                   bat 'docker push selvakumarsmddocker/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}