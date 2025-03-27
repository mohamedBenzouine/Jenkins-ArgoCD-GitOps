pipeline {
	agent any
	tools{
		nodejs 'NodeJS'
	}
	environment {
		DOCKER_HUB_REPO = 'benzouine1991/jenkins'
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
					docker.build("${DOCKER_HUB_REPO}:latest")
				}
			}
		}
		stage('Trivy Scan'){
			steps {
				sh '''
				echo 'Trivy Scan'
				'''
			}
		}
		stage('Push Image to DockerHub'){
			steps {
				sh '''
				echo 'Push Image to DockerHub'
				'''
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
