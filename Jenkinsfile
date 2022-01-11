def modules = ['all-post-service', 'edit-post-service'];
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    modules.each { item -> 
                        dir("${item}"){
                            print("Building Code for $item")
                            bat "npm install"
                            // bat "npm run build"
                        }
                    }
                }
            }
        }
        
         stage('Test') {
            steps {
                script {
                     modules.each { item -> 
                        dir("${item}"){
                        print("Testing Code for $item")
                        // bat "npm run sonar" 
                        }
                    }
                }
            }
        }
        stage('Dockerizing microservices'){
            steps {
                script {
                     modules.each { item -> 
                        dir("${item}"){
                        print("Dockerizing microservices for $item")
                        bat "docker build . -t heyshraddha/$item"    
                    
                        }
                    }
                }
            }
        }
        stage('Pushing Docker Images to Registery'){
            steps {
                script {
                    bat "docker login -u heyshraddha -p QAZwsx@#123"
                     modules.each { item -> 
                        dir("${item}"){
                        print("Pushing Docker Images to Registery for $item")
                        bat "docker push heyshraddha/$item  "    
                    
                        }
                    }
                }
            }
        }
        stage('Deploying Micro Services to Kubernetes') {
            steps {
                script {
                     modules.each { item -> 
                        dir("${item}"){
                        print("Deploying Micro Services to Kubernetes $item")
                        bat "kubectl create deployment $item-cluster --image heyshraddha/$item" 
                        bat "kubectl scale deployment $item-cluster --replicas 2"
                        bat "kubectl expose deployment $item-cluster --type=NodePort --port 3000"
                        }
                    }
                    sleep(60);
                    bat "kubectl get services"
                    bat "kubectl get deployment"
                }
            }
        }

    }
}