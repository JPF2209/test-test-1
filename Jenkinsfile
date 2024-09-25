pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        jdk "OpenJDK8"
        maven "Maven3"
        nodejs "node22"
    }

    stages {
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                 git branch: "main", changelog: false, poll: false, url: "https://github.com/JPF2209/SIT223-9.2H-Test1.git"
                echo"1"
            }
        }
        stage('Build') {
            steps {
                 sh "mvn install"
            }
        }
        stage('Compile') {
            steps {
                 sh "mvn clean compile"
            }
        }

        stage('Build Docker Image') {
            steps {
                 sh "docker build -t jpf2209/test-1 ."
            }
        }

        stage('Scan') {
            steps {
                 sh "trivy jpf2209/test-1:latest"
            }
        }

        stage('Push Docker Image') {
            steps {
                 script{
                     withCredentials([string(credentialsId: 'dockerhub', variable: 'dhub')])
                     sh "docker login -u jpf2209 -p ${dhub}."
                     sh "docker push jpf2209/test-1
                 }
            }
        }
    }
}



pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        jdk "OpenJDK8"
        maven "Maven3"
        nodejs "node22"
        docker "docker"
    }

    stages {
        stage('Initialize'){
            steps{
                def dockerHome = tool 'Docker'
                env.PATH = "${dockerHome}/bin:${env.PATH}"
                echo "${env.PATH}"
            }
        }
        
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                git branch: "main", changelog: false, poll: false, url: "https://github.com/JPF2209/SIT223-9.2H-Test1.git"
                echo"1"
            }
        }
        stage('OWASP DC') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('Build') {
            steps {
                 sh "mvn clean compile"
            }
        }
        stage('Build Docker Image') {
            steps {
                 sh "docker build -t jpf2209/test-1 ."
            }
        }
    }
}

