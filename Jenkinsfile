pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        nodejs 'node16'
        
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/fullstack-bank.git'
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
                }
            }
        }
        
        
         stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        
        stage('Backend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/backend') {
                    sh "npm install"
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                sh "npm run compose:up -d"
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
    }

    stages {
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
    }
}
