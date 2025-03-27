pipeline {
	agent any
	tools{
		nodejs 'NodeJS'
	}
	environment {
		DOCKER_HUB_REPO = 'benzouine1991/jenkins'
		DOCKER_HUB_CREDENTIALS_ID = 'gitops-dockerhub'
	}
	
	stages {
		stage('Checkout Github'){
			steps {
			git branch: 'main', credentialsId: 'gitops-token', url: 'https://github.com/mohamedBenzouine/Jenkins-ArgoCD-GitOps.git'
			}
		}		
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Build Docker Image'){
			steps {
				script {
					echo 'building docker image ...'
					dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
				}
			}
		}
		stage('Trivy Scan'){
			steps {
					//sh 'trivy image --severity HIGH,CRITICAL --format table -o trivy-scan-report.txt ${DOCKER_HUB_REPO}:latest'
					sh 'trivy image --severity HIGH,CRITICAL --skip-update --format table -o trivy-scan-report.txt ${DOCKER_HUB_REPO}:latest'
				}
		}
		stage('Push Image to DockerHub'){
			steps {
				script {
					echo 'pushing docker image to DockerHub...'
					docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS_ID}"){
						dockerImage.push('latest')
						}
					}
				}
			}
		stage('Install Kubectl & ArgoCD CLI'){
			steps {
				sh '''
				echo 'Install Kubectl & ArgoCD CLI'
				'''
			}
		}
		stage('Apply Kubernetes Manifests & Sync App with ArgoCD'){
			steps {
				sh '''
				echo 'Apply Kubernetes Manifests & Sync App with ArgoCD'
				'''
			}
		}
	}

	post {
		success {
			echo 'Build & Deploy completed succesfully!'
		}
		failure {
			echo 'Build & Deploy failed. Check logs.'
		}
	}
}
