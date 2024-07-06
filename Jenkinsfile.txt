pipeline{
    agent any
    environment {
    FIREBASE_DEPLOY_TOKEN = credentials('firebase-token')
    }

 stages{
        stage('Building'){
            steps{
           // sh 'npm install -g firebase-tools'
                echo 'Biulding...'
            }
        } 
        stage('Testing Environment'){
            steps{
            sh 'firebase deploy -P testing-env-devnet --token "$FIREBASE_DEPLOY_TOKEN"'
            input message: 'After testing. Do you want to continue with Staging Environment? (Click "Proceed" to continue)'
            }
        } 
        stage('Staging Environment'){
            steps{
             sh 'firebase deploy -P staging-env-devnet-5c91b --token "$FIREBASE_DEPLOY_TOKEN"'
            }
        } 
        stage('Production Environment'){
            steps{
            sh 'firebase deploy -P production-env-devnet-97478 --token "$FIREBASE_DEPLOY_TOKEN"'
            }
        } 
    }

}
