pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK_CREATOR = "/home/ec2-user/ansible/create-infra.yaml"
        ANSIBLE_PLAYBOOK_TESTING = "/home/ec2-user/ansible/run-tests.yaml"
        ANSIBLE_PLAYBOOK_DESTROY = "/home/ec2-user/ansible/destroy-infra.yaml"
        VAULT_PASS = "/home/ec2-user/ansible/vault_pass.txt"
        
    }

    stages {
        stage('Build') {
            agent { label 'Built-In Node' }
            steps {
                script {
                    echo "Running Ansible to create testing EC2 instance"
                    sh "ansible-playbook ${ANSIBLE_PLAYBOOK_CREATOR} --vault-password-file ${VAULT_PASS}"
                }
            }
        }

        stage('Test') {
            agent { label 'Built-In Node' }
            steps {
                script {
                    echo "Testing the Flask app"
                    sh "ansible-playbook ${ANSIBLE_PLAYBOOK_TESTING} --vault-password-file ${VAULT_PASS}"
                }
            }
        }

        stage('Deploy') {
            steps {
                agent { label 'Built-In Node' }
                script {
                    echo "Deploy to production - Ensure you're logged into DockerHub"
                    
                    // stop the previous countiner
                    sh "docker ps -q --filter 'name=flaskapp' | grep -q . && docker stop flaskapp || true"
                    
                    // push and run the new image
                    sh "docker pull meitaltal92/flaskapp:latest"
                    sh "docker run -d -p 80:80 --name flaskapp meitaltal92/flaskapp:latest"
                }
            }
        }
    }

    post { 
        always 
        node('Built-In Node')
        { 
            echo "Destroying EC2 instance"
            sh "ansible-playbook ${ANSIBLE_PLAYBOOK_DESTROY} --vault-password-file ${VAULT_PASS}"
        }
    }
}
