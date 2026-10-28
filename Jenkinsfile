
pipeline {
    agent any

    environment {
        KUBECTL = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe'
        KUBECONFIG_PATH = 'C:\\Users\\HP\\.kube\\config'
    }

    stages {
        stage('Verify Kubernetes Context') {
            steps {
                bat '''
                    echo ===== CURRENT CONTEXT =====
                    "%KUBECTL%" --kubeconfig="%KUBECONFIG_PATH%" config current-context
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Verify Cluster Connection') {
            steps {
                bat '''
                    echo ===== KUBERNETES NODES =====
                    "%KUBECTL%" --kubeconfig="%KUBECONFIG_PATH%" get nodes
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Verify Cluster Information') {
            steps {
                bat '''
                    echo ===== CLUSTER INFORMATION =====
                    "%KUBECTL%" --kubeconfig="%KUBECONFIG_PATH%" cluster-info
                    if errorlevel 1 exit /b 1
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Jenkins connected to Kubernetes and verified the cluster.'
        }
        failure {
            echo 'FAILED: Check the first error in Console Output.'
        }
    }
}
