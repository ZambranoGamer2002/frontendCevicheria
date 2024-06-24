pipeline {
    agent any

    stages {
        stage('Preparacion') {
            steps {
                git branch: 'main', url: 'https://github.com/ZambranoGamer2002/frontendCevicheria.git'
                echo 'Pulled from GitHub successfully'
            }
        }

        stage('Verifica version php') {
            steps {
                sh 'php --version'
            }
        }

        stage('Unit Test php') {
            steps {
                sh 'chmod +x vendor/bin/phpunit'
                sh 'vendor/bin/phpunit'
            }
        }

        stage('Integration Test') {
            steps {
                echo 'Running Integration Tests...'
                // Aquí puedes agregar los comandos necesarios para ejecutar las pruebas de integración
                // Por ejemplo, si utilizas un script de shell para las pruebas de integración, podrías usar:
                // sh './scripts/run-integration-tests.sh'
                
                // Si utilizas una herramienta específica, asegúrate de tenerla instalada y configurada
                // Ejemplo con PHPUnit para pruebas de integración:
                sh 'vendor/bin/phpunit --configuration phpunit-integration.xml.dist'
            }
        }

        // Revisa la calidad de código con SonarQube
        // stage('Sonarqube') {
        //     steps {
        //         script {
        //             def scannerHome = tool name: 'sonarscanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
        //             echo "scannerHome = $scannerHome ...."
        //             withSonarQubeEnv() {
        //                 sh "$scannerHome/bin/sonar-scanner"
        //             }
        //         }
        //     }
        // }

        stage('Docker Build') {
            steps {
                sh 'docker build -t frontendcevicheria .'
            }
        }

        stage('Deploy php') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/logs/**', allowEmptyArchive: true
        }
        success {
            echo 'La construcción y las pruebas han sido exitosas.'
        }
        failure {
            echo 'La construcción o las pruebas han fallado.'
        }
    }
}
