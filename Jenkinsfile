pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "run npm test"'
            }
        }
        stage('Quality Gate') {
            steps {
                // sh '/opt/sonar/bin/sonar-scanner'
                sh 'echo "run Sonarqube"'
            }
        }
        stage('Deploy to Development') {
            steps {

                // cfpush
                pushToCloudFoundry(
                    target: 'api.run.pivotal.io',
                    organization: 'mason.glenn',
                    cloudSpace: 'development',
                    credentialsId: 'mkultra_pws',
                    pluginTimeout: 240, // default value is 120
                    manifestChoice: [ // optional... defaults to manifestFile: manifest.yml
                        manifestFile: '../app/manifest.yml'
                    ]
                )

                // publish to artifactory
                sh 'npm publish'

                // notify slack channel
            }
        }
        stage('DB Migrate') {
            steps {
                // pause for input
                sh 'echo "flyway migrate"'
            }
        }
        stage('Functional Test') {
            steps {
                sh 'echo "Run Functional Tests"'
            }
        }
        stage('Deploy to QA') {
            steps {
                // pause for input
                sh 'echo "Run Functional Tests"'
            }
        }
    }
}