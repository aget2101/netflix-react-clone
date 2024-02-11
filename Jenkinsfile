pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("335871625378.dkr.ecr.us-west-2.amazonaws.com/netflix-jan:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 335871625378.dkr.ecr.us-west-2.amazonaws.com/netflix-jan:latest'
            }
       }
        stage('Push to ECR') {
        steps {
            script {
                // Correct registry URL without specifying the repository
                docker.withRegistry('https://335871625378.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:abreham-netflix') {
                    // Assuming docker.build was successful in a previous step and myImage is available
                    def myImage = docker.image("335871625378.dkr.ecr.us-west-2.amazonaws.com/netflix-jan:latest")
                    // Push image
                    myImage.push()
            }
        }
    }
}

        
    }
}
