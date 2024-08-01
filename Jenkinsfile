pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        digital_ocean ='dop_v1_4427c1107c962366d0a6ba8c79a763db732347280d80d9c728a4a6290c3037b0'
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
                    docker build . -t registry.digitalocean.com/jenkins-server:${APP_ENV}${BUILD_NUMBER}
                    docker login registry.digitalocean.com -u ${digital_ocean} -p ${digital_ocean} 
                    docker push  registry.digitalocean.com/jenkins-server:${APP_ENV}-${BUILD_NUMBER}
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