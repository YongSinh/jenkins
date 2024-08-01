pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        registry_url="registry.digitalocean.com/jenkins-server"
        digitalocean_token ="dop_v1_69dd7da00c3632c9a91986170bf3473573b7e24b68044b646f33d13108f5ba61"
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