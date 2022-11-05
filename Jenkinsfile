pipeline {
    agent any
    tools {
        maven 'M2_HOME'

    }

    stages {
        stage('Checkout GIT') {
            steps {
                echo 'Pulling...';
                git branch: 'main',
                url: 'https://github.com/mohamedjihedriahi/Devops_Project2.git'
                

                
            }
          
        }
        
        stage('prepare project') {
        steps {
            // The -a option is an improved recursive option, that preserve all file attributes, and also preserve symlinks.
        // The . at end of the source path is a specific cp syntax that allow to copy all files and folders, included hidden ones.
        sh "cp -a ./Backend/. ."
        sh "rm -rf ./Backend"
        // List the final content
        sh "ls -la"
        }
    }
    
        stage("Build & Tests") {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
                    
        }
        
       stage('Docker') {
            
            steps {
                
                sh 'docker-compose up --detach'
                
            }
        }
        
        stage('Nexus') {
          steps {
            sh 'mvn deploy -Dmaven.test.skip=true -e'
          }
        }
        
        stage('mv clean & compile') {
          steps {
            sh 'mvn clean'
            sh 'mvn compile'
          }
        }
        
        stage('Sonarqube') {
          steps {
            sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=sonar'
          }
        }
        
    }
}
