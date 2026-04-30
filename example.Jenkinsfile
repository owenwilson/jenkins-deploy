pipeline {
    // configure node
    agent any
    tools {
        maven 'maven-3-9-15'
        jdk 'jdk-17'
    }
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
        stage('Scan sonarqube') {
            steps {
                echo 'Scan'
                script {
                    def microservices = ['folder-microservice-1', 'folder-microservice-2', 'folder-microservice-3']
                    microservices.each { service ->
                        dir(service) {
                            writeFile file: 'sonar-project.properties', text: """
                            sonar.projectKey=example-1-microservices.${service}
                            sonar.projectName=example-1 - ${service}
                            sonar.projectVersion=1.0

                            sonar.sources=src/main/java
                            sonar.tests=src/test/java
                            sonar.java.binaries=target/classes

                            sonar.sourceEncoding=UTF-8
                            sonar.language=java

                            sonar.exclusions=**/generated/**/*.*, **/model/**/*.java
                            """

                            def javaHome = '/opt/java/openjdk'
                            withSonarQubeEnv(installationName: 'sonarqube-server') {
                                withEnv(["JAVA_HOME=${javaHome}", "PATH+JAVA=${javaHome}/bin"]) {
                                    sh'''
                                    export JAVA_HOME=$JAVA_HOME
                                    '''
                                    sh 'mvn clean compile'
                                    sh 'sonar-scanner'
                                }
                            }
                        }
                    }
                }
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
