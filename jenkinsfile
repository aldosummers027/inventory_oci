pipeline {
    agent any

    environment {
        VENV_PATH = "/home/ubuntu/ansible_venv/bin/activate"
        PLAYBOOK_NAME = "inventario_detallado.yml" 
        OCI_PROFILE = "DEFAULT" 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Obteniendo el código del repositorio...'
                checkout scm
            }
        }

        stage('Activate Environment & Run Playbook') {
            steps {
                echo "Activando entorno virtual: ${env.VENV_PATH}"
                echo "Ejecutando el playbook: ${env.PLAYBOOK_NAME}"
                
                sh """
                    source ${env.VENV_PATH}
                    OCI_CONFIG_PROFILE="${env.OCI_PROFILE}" ansible-playbook ${env.PLAYBOOK_NAME}
                """
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archivando el reporte CSV generado...'
                archiveArtifacts artifacts: 'inventario_detallado_vms.csv', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Limpiando el workspace...'
            cleanWs()
        }
    }
}