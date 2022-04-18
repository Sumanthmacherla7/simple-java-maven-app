pipeline {
    agent {
        label 'maven'
    }
    options {
        timeout(10)
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
            }
        }
        stage('Deploy to Dev') {
            parallel {
                stage('target1'){
                    environment {
                        target_user = "ubuntu"
                        target_server = "172.31.11.162"
                    }
                    steps {
                        echo "Deploying to Dev Environment"
                        //sshagent(['maven-cd-key']) {
                        //    sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ubuntu"
                        //}
                    }
                }
                stage('target2'){
                    environment {
                        target_user = "ec2-user"
                        target_server = "172.31.51.74"
                    }
                    steps {
                        echo "Deploying to Dev Environment"
                        //sshagent(['maven-cd-key']) {
                        //    sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ec2-user"
                        //}
                    }
                }
            }
        }
        stage('Deploy to UAT') {
            input {
                message 'Do you want me to deploy to UAT?'
            }
            environment {
                target_user = "ec2-user"
                target_server = ""
            }
            steps {
                echo "Awaiting"
            }
        }
    }
    post {
        always {
            deleteDir()
        }
        success {
            echo "The job is successful"
        }
    }
}

/*
        stage('Deploy') {
            steps {
                echo "Deploying to Dev Environment"
                sshagent(['maven-cd-key']) {
                    sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ec2-user"
                }
            }
        }
*/
