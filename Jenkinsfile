pipeline {
    agent any


    stages {
        stage('ci') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh "sudo apt install docker.io"
                sh "docker build . -t ${USERNAME}/haidy-web-app-v1 -f Dockerfile"
                sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                sh "docker push ${USERNAME}/haidy-web-app-v1"}

            }


        }
        stage('cd'){
            steps{
                  sh """
                  kubectl apply -f deploy.yaml 
                  kubectl apply -f service.yaml 
                  """

            }

        }
    }
}


