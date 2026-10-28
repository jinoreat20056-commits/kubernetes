
pipeline {
    agent any

    environment {
        DOCKER_EXE = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        KUBECTL_EXE = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe'
        KUBE_CONFIG = 'C:\\Users\\HP\\.kube\\config'
        IMAGE_NAME = 'simple-web-app:1.0'
    }

    stages {
        stage('Verify Kubernetes Connection') {
            steps {
                bat '''
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" config current-context
                    if errorlevel 1 exit /b 1
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" get nodes
                    if errorlevel 1 exit /b 1
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" cluster-info
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Build Local Image') {
            steps {
                bat '''
                    "%DOCKER_EXE%" build -t %IMAGE_NAME% .
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                bat '''
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" apply -f deployment.yaml
                    if errorlevel 1 exit /b 1
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" apply -f service.yaml
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '''
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" rollout status deployment/simple-web-app --timeout=180s
                    if errorlevel 1 exit /b 1
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" get pods
                    if errorlevel 1 exit /b 1
                    "%KUBECTL_EXE%" --kubeconfig="%KUBE_CONFIG%" get services
                    if errorlevel 1 exit /b 1
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Kubernetes connection verified and application deployed.'
        }
        failure {
            echo 'FAILED: Check the first failing stage in Console Output.'
        }
    }
}
