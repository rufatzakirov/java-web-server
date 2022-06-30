pipeline{
    agent any
    stages{
        stage("build jar package"){
            steps{
                sh "mvn package"
            }
        }
        stage("build image"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh "docker build -t zakirovrufat/$JOB_NAME:$BUILD_NUMBER ."
                    sh "docker login -u $username -p $password"
                    sh "docker push zakirovrufat/$JOB_NAME:$BUILD_NUMBER"
                }
            }
        }
        stage("deploy container"){
            steps{
                sh "docker run -d --name web$BUILD_NUMBER -p 80$BUILD_NUMBER:8080 zakirovrufat/$JOB_NAME:$BUILD_NUMBER"
            }
        }
    }
}
