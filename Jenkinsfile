pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        registry_url="registry.digitalocean.com/jenkins-server"
        digitalocean_token ="dop_v1_cf007382e7ccf1b095e457897991cdfc54e48ca4e8d3a90ed085dbefb3e5b41a"
    }
    parameters {

        text(name: 'Release note', defaultValue: 'Hello', description: 'Enter some information about the person')

        choice(name: 'APP_ENV', choices: ['unt', 'preparerod', 'prod'], description: 'Pick something')
    }
    stages {
        stage('config') {
            steps {
                sh'''
                    rm -rf employee-api
                    git clone ${git_repo}
                    pwd
                '''
            }
        }
        stage('Build') {
            steps {
                 sh'''
                    cd employee-api
                    docker build . -t ${registry_url}:${APP_ENV}${BUILD_NUMBER}
                    docker login registry.digitalocean.com -u ${digitalocean_token} -p ${digitalocean_token} 
                    docker push  ${registry_url}:${APP_ENV}-${BUILD_NUMBER}
                '''
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
}