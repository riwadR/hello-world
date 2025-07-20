pipeline {
  agent any

  environment {
    SONARQUBE_SERVER = 'SonarQube' // Nom défini dans Jenkins > Global Tool Config
  }

  stages {
    stage('1. Checkout') {
      steps {
        git 'https://github.com/ton-utilisateur/hello-world.git'
      }
    }

    stage('2. Analyse SonarQube') {
      steps {
        withSonarQubeEnv("${SONARQUBE_SERVER}") {
          sh 'sonar-scanner'
        }
      }
    }

    stage('3. Déploiement avec Ansible') {
      steps {
        sh '''
          cd ansible
          ansible-playbook -i inventory.ini playbook.yml
        '''
      }
    }

    stage('4. Test de la page déployée') {
      steps {
        sh 'curl http://127.0.0.1:2222 || echo "Vérifier que nginx tourne"'
      }
    }

    stage('5. Nettoyage') {
      steps {
        sh '''
          docker stop ssh-host
          docker rm ssh-host
        '''
      }
    }
  }
}
