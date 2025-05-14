pipeline {
    agent any  // Use any available agent
    

    tools {
        maven 'MAVEN'  // Ensure this matches the name configured in Jenkins
    }
    
    environment {
        ANSIBLE_PASSWORD = credentials('ansible_sudo_password')  // Use Jenkins credentials
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/vsshubh/MavenAnsible.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'  // Run Maven build
            }
        }

     stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint:true
            }
        }
        stage('Deploy') {
            steps {
               sh 'mvn clean package'  
               sh 'ansible-playbook ansible/playbook.yml -i ansible/hosts.ini --ask-become-pass --extra-vars ansible_become_password=${env.ANSIBLE_PASSWORD}'
            }
        }

                  
    }

   }
