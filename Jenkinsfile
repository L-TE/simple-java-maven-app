// *****************************************************************************
// Un Jenkins pipeline permet de definir un enchaînement de phases (workflow)
// qui seront executés dans une tache (Job) Jenkins.
// Le fichier contenant le pipeline doit se nommer 'Jenkinsfile' et se trouver
// dans le répertoire racine du projet concerné.
// Il sera executé par une tache Jenkins chaque fois que le projet auquel il
// appartient sera modifié.
// *****************************************************************************
pipeline {
  //
  // Définition des variables d'environnement associés au pipeline
  //
  environment {
    NOTIFICATION_MSG = '-= END OF PIPELINE =-.'
  }
  //
  // Définition du noeud Jenkins (worker) sur le lequel sera exécuté le pipeline
  //
  agent {
    // Noeud/machine installé avec 'Docker' 
    // L'execution aura lieu dans le conteneur éphémère
    // basé sur l'image et les paramètres indiqués.
    docker {
      image 'maven:3-alpine'
      args '--volume /root/.m2:/root/.m2'
    }
  }
  //
  // Définition des phases (stages) du pipeline
  //
  stages {
    // Phase de construction (build)
    stage('Build') {
      // Définition des étapes (steps) de la phase
      steps {
        // Une étape = Un Jenkins plugin
        // Ici on utilise le Jenkins plugin : 'sh' 
        // Etape de creation de l'artefact avec Maven
        sh 'mvn -B -DskipTests clean package'
      }
    }
    // Phase de tests
    stage('Test') {
      // Définition des étapes (steps) de la phase
      steps {
        // Etape de test de l'artefact avec Maven
        sh 'mvn test'
      }
      // Définition des étapes de fin de phase
      post {
        // Etapes toujours executées (finally)
        always {
          // Etape de ????
          junit {
            allowEmptyResults true
            healthScaleFactor 5.0
            testResults 'target/surefire-reports/*.xml'
          }
        }
      }
    }
    // Phase de déploiement et d'execution de l'artefact (deploy)
    stage('Deliver') {
      // Condition d'execution des étapes (steps) de la phase
      when {
        // Si la phase Jenkins de 'build' 
        // n'a pas été executée
        // ou s'est terminée avec succès 
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }
      }
      // Définition des étapes (steps) de la phase
      steps {
        // Etape d'execution de l'artefact (Script Maven et Java)
        sh './jenkins/scripts/deliver.sh'
      }
    }
    // Phase de notification
    stage('Notify') {
      // Définition des étapes (steps) de la phase
      steps {
        // Etape d'affichage d'un message sur la sortie 'console'
        echo "${env.NOTIFICATION_MSG}"
      }
    }
  }
}