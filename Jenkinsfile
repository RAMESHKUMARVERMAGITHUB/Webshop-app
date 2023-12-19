pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        // nodejs 'node16'
        
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/RAMESHKUMARVERMAGITHUB/Webshop-app.git'
            }
        }
        stage('CLJ-HOMES-SCAN') {
            steps {
                sh '''
                clj-holmes fetch-rules
                clj-holmes scan -p ./
                '''
            }
        }
        // stage('OWASP FS SCAN') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
        //             dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        
        // stage('TRIVY FS SCAN') {
        //     steps {
        //         sh "trivy fs ."
        //     }
        // }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=webshop -Dsonar.projectKey=webshop "
                }
            }
        }
        
        
        //  stage('Install Dependencies') {
        //     steps {
        //         sh "npm install"
        //     }
        // }
        
        // stage('Backend') {
        //     steps {
        //         dir('/var/lib/jenkins/workspace/bank/app/backend') {
        //             sh "npm install"
        //         }
        //     }
        // }
        
        // stage('frontend') {
        //     steps {
        //         dir('/var/lib/jenkins/workspace/bank/app/frontend') {
        //             sh "npm install"
        //         }
        //     }
        // }
        
        stage('Deploy to Container') {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
