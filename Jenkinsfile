pipeline {
    agent any
 
    options {
        timestamps()
    }

    parameters{
        choice(name: 'ENTORNO', choices: ['dev', 'qa','prod'], description: 'Ambiente destino')
        string(name: 'VERSION', defaultValue: '1.0.0', description: 'Version a desplejar')
        booleanParam(name: 'EJECUTAR_TESTS', defaultValue: true, description: 'Correr los tests')
    }
               
    stages {
        stage('Instalar dependencias') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install --quiet -r requirements.txt
                '''
            }
        }
 
        stage('Lint') {
            steps {
                sh '''
                    . .venv/bin/activate
                    ruff check .
                '''
            }
        }
 
        stage('Test') {
            when {
                expression { paramms.EJECUTAR_TESTS }
            }
            steps {
                sh '''
                    . .venv/bin/activate
                    pytest --junitxml=reports/junit.xml
                '''
            }
        }
    }
 
    post {
        always {
            junit 'reports/junit.xml'
        }
    }
}
