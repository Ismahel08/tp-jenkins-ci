pipeline {
 agent any
 options {
 // Arrête vite si ça coince, limite la verbosité
 ansiColor('xterm')
 timestamps()
 }
 stages {
 stage('Checkout') {
 steps {
 // Si votre projet est dans un SCM connecté à Jenkins, ceci suffit :
 checkout scm
 // Sinon, commentez checkout scm et utilisez un "Pipeline script" inline
 // en collant ce Jenkinsfile directement dans le job.
 }
 }
 stage('Setup Python venv') {
 steps {
 sh '''
 set -e
 if ! command -v python3 >/dev/null 2>&1; then
 echo "Python3 manquant sur l9agent Jenkins. Installez Python 3.10+."
 exit 1
 fi
 python3 -m venv venv
 . venv/bin/activate
 python -m pip install --upgrade pip
 '''
 }
 }
 stage('Tests') {
 steps {
 sh '''
 set -e
 . venv/bin/activate
 # Si vous utilisez pytest :
 # python -m pip install pytest
 # pytest -q
 # Ici on utilise unittest (intégré) :
 python -m unittest -v
 '''
 }
 }
 }
 post {
 always {
 archiveArtifacts artifacts: '**/*.py', onlyIfSuccessful: false
 }
 success {
 echo ' Pipeline OK'
 }
 failure {
 echo ' Pipeline en échec 4 vérifiez les logs de la stage "Tests"'
 }
 }
}