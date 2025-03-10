pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage("Pull SRC") {
            steps {
                git branch: 'main', url: 'https://github.com/sanjana2580/ansible-terraform-alb.git'
            }
        }
        stage("move the target") {
            steps {
                sh 'mv target/bhoomika.war .'
            }
        }
        stage("Prepare Build") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("build docker image"){
            steps{
                sh 'docker build -t app .'
            }
        }
        stage("tag image"){
            steps{
                sh 'docker tag app sanjana255/alb-terra:latest'
            }
        }
        stage("push image"){
            steps{
               sh 'echo "Biradar@24"| docker login -u sanjana255 --password-stdin'
                sh 'docker push sanjana255/alb-terra:latest'
            }
        }
        stage("remove images locally"){
            steps{
                sh 'docker rmi app'
            }
        }
        
        stage("run ansible playbook") {
            steps {
                sshPublisher(
                    continueOnError: false, 
                    failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                            configName: "remote",
                            transfers: [
                                sshTransfer(sourceFiles: 'play.yml'),
                                sshTransfer(execCommand: "ansible-playbook play.yml")
                            ],
                            verbose: true
                        )
                    ]
                )
            }
        }
    }
}