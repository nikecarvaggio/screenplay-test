pipeline {
    agent {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm ci' 
            }
        }
    stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    stage('Deliver') {
            steps {
                sh 'npm start'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Generar evidencia'){
            steps{
                 script{
                     try{
                          sh  "mv \"${WORKSPACE}/target\" serenity_${timestamp}"
                          echo 'Backup de evidencias realizado con exito'
                          publishHTML([
                                allowMissing: false,
                                alwaysLinkToLastBuild: true,
                                keepAll: true,
                                reportDir: "${WORKSPACE}//serenity_${timestamp}",
                                reportFiles: 'index.html',
                                reportName: 'Evidencias Automatizacion WEB Screenplay',
                                reportTitles: 'Reto tecnico web'
                          ])
                                echo 'Reporte Serenity realizado con exito'
                                archiveArtifacts "**/cucumber.json"
                                cucumber '**/cucumber.json'
                                echo 'Reporte Cucumber realizado con exito'
                          }
                          catch(e){
                                echo 'No se realizo el Backup de evidencias'
                                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 
"${WORKSPACE}//target/serenity_${timestamp}", reportFiles: 'index.html', reportName: 'Evidencias Automatizacion WEB Screenplay', 
reportTitles: 'Proyecto Mobiletec Screenplay'])
                                echo 'Reporte Html realizado con exito'
                                currentBuild.result='SUCCESS'
                          }
                 }
            }
        }
    }
}
