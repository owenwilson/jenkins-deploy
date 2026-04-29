pipeline {
    // configure node
    agent any

    options {
        buildDiscarder(logRotator(
            numToKeepStr: '5',
            artifactNumToKeepStr: '5'
        ))
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                // check ssh agent plugin
                sh 'which ssh-agent || echo "ssh-agent not found"'
                // check verify ssh git connection
                sshagent(['github_key_name']) {
                    sh 'ssh -T -o StrictHostKeyChecking=no git@github.com || true'
                }
                // clone repository
                git(
                    url: 'git@github.com:nameUser/repositoryNameProject.git',
                    branch: 'main',
                    credentialsId: 'github_key_name'
                )
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        always {
            // cleanWs()
            // clear workspace
            cleanWs deleteDirs: true
        }
    }
}
