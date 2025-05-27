pipeline{
    agent any
    stages{
        stage('Clone'){
            steps{
                git branch:'main', url:'https://github.com/ThakurSahilSingh/FlaskApp-with-pv.git'
            }
        }
        //stage('Adding Files'){
        //    steps{
        //        sh '''
        //        cp /root/pv/Dockerfile /root/.jenkins/workspace/flask-pv/
        //        cp -r /root/pv/k8s /root/.jenkins/workspace/flask-pv/
        //        '''
        //    }
        //}
        stage('Build'){
            steps{
                sh '''
                docker build -t sahil0824/flask-pvc:latest .
                '''
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'docker-pass', usernameVariable: 'docker-user')]){
                    sh '''
                    docker login -u "$docker-user" -p "$docker-pass"
                    docker push sahil0824/flask-pvc:latest
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                withCredentials([file(credentialsId: 'k8-cred', variable: 'KUBECONFIG')]) {
                    sh '''
                    kubectl apply -f k8s/. --name-space=default
                    '''
                }   
            }
        }
    }
}
