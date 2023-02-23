pipeline {
    agent any 
    stages {
        stage('build') {
            steps {
                    withCredentials([usernamePassword(credentialsId: 'dockercredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t haidy/project-app:${BUILD_NUMBER} .
                            docker push haidy/project-app:${BUILD_NUMBER}
                            echo ${BUILD_NUMBER} > ../bakehouse-build-number.txt
                        """
                    }
                    
                
            }
        }
        stage('deploy') {
            steps {
            
                    withCredentials([file(credentialsId: 'kubernetes_kubeconfig', variable: 'config')]) {
                    sh """
                        export BUILD_NUMBER=\$(cat ../bakehouse-build-number.txt)
                        mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                        cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                        rm -f Deployment/deploy.yaml.tmp
                        kubectl apply -f Deployment --kubeconfig=${config}
                    """
                }
            
                
            }
        }
    }
}
