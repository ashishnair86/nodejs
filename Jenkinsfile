pipeline {
    agent any

    environment {
        OPENSHIFT_NAMESPACE = 'my-1st-project'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t docker.io/ashishnair/node:latest .'
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker', url: '']) {
                        sh 'docker push docker.io/ashishnair/node:latest'
                    }
                }
            }
        }

        stage('Deploy to OpenShift') {
            steps {
                script {
                    openshift.withCluster(OPENSHIFT_API, OPENSHIFT_TOKEN) {
                        openshift.withProject(OPENSHIFT_NAMESPACE) {
                            openshift.apply(file: 'openshift-deployment.yaml')
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'CI/CD pipeline complete!'
        }
    }
}

