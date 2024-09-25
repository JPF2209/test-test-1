pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        jdk "OpenJDK8"
        maven "Maven3"
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
                 sh "docker build -t jpf2209/6_2hd:$BUILD_NUMBER ."
            }
        }
        
        stage('OWASP DC') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Dockerhub Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh "docker build -t jpf2209/6_2hd:$BUILD_NUMBER ."
                        sh "docker push jpf2209/6_2hd:$BUILD_NUMBER"
                    }
                }
            }
        }
    }
}
