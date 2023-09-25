pipeline {
    
    agent any
    
      stages {
        stage ('Git-Checkout') {
            
            steps {
                
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/VardhanLearn/Jenkins-KubernetesCluster.git']])
                
            }
        }  
        
         stage ('Maven-Build') {
             
             steps {
                
                sh 'mvn install -DskipTests'   
             }
             
             post {
	           success {
	              echo 'Now Archiving it...'
	              archiveArtifacts artifacts: '*/**.jar'
	           }
	        }
        }
	        
         stage('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
       } 
         stage('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
       }
        stage ('Docker Build & Push') {
            
            steps {
                
                script {
                    
                   withDockerRegistry(credentialsId: 'vardhan8928-docker') {
                        
                        sh "docker build -t vardhan8928/kubernetes-deploy:tag130 ."
                        sh "docker push vardhan8928/kubernetes-deploy:tag130"
                    }
                    
                }
                
            }
            
            post {
                
                success {
                    
                   echo 'Now Archiving it...'
	               archiveArtifacts artifacts: '*/**.jar'
                    
                }
                
            }
        }
        stage('Deploy to K8s') {
          steps {
            script {
                
                kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
            }
          }
        }
      }    
    }
