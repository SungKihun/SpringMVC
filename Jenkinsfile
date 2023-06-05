//
pipeline {
    environment {
        imageName = "tjdrlgns3293/springlegacy-mvc"
        registryCredential = 'eastshine-token'
        kubeconfig = '/home/sungkihun/.kube/config'
        dockerImage = ''
    }
    agent any
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('1. Docker Build') {
            steps {
                script {
                    echo "1. Docker Building..."
                    dockerImage = docker.build(imageName)
                }
            }
        }
        
        stage('2. Dockder Push') {
            steps {
                script {
                    echo "2. Docker Pushing..."
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                            dockerImage.push("latest")
                    }
                }
            }
        }
        stage('3. K8s Deploy') {
            steps {
                script {
                    echo "3. K8s Deploy..."
                    //sh "/usr/local/bin/kubectl --kubeconfig=${kubeconfig} delete -f ./deploy-jwt.yaml -n khsung"
                    sh "/usr/local/bin/kubectl --kubeconfig=${kubeconfig} apply -f ./deploy-springmvc.yaml -n khsung"
                }
            }
        }
    }
}
