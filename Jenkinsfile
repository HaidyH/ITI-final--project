pipeline {
    agent any


    stages {
        stage('ci') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                sh "docker build . -t ${USERNAME}/haidy-web-app-v1 -f Dockerfile"
                sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                sh "docker push ${USERNAME}/haidy-web-app-v1"}

            }


        }
        stage('cd'){
            steps{
                  sh """
                  kubectl create ns app
                  kubectl apply -f deploy.yml 
                  kubectl apply -f service.yml 
                  """

            }

        }
    }
}


