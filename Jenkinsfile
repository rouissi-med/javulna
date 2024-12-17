pipeline { // Defines a pipeline
  agent any // Specifies that the pipeline can be run on any available agent

  tools { // Configures the tools used in the pipeline
    maven 'maven' // Specifies the Maven tool that should be used in the pipeline
  }

  stages { // Defines the different stages of the pipeline
    stage('Checkout Source') { // Defines the 'Checkout Source' stage
      steps { // Specifies the steps to be executed within this stage
        git 'https://github.com/MarwenSoula/javulna.git' // Retrieves the source code from the specified GitHub repository
      }
    }

    stage('SonarQube Analysis') {
      steps {
        sh "mvn  verify sonar:sonar \
             -Dsonar.projectKey=TEST1 \
             -Dsonar.host.url=http://192.168.27.128:9002 \
             -Dsonar.login=sqp_24c9c23056ca58b9baf086f13aa08e6a32981f53"
      }
    }
    stage ('Unit Test') { // Defines the 'Unit Test' stage
      steps { // Specifies the steps to be executed within this stage
        sh 'mvn test' // Runs the Maven command to execute the unit tests
      }   
    }

    stage ('Build') { // Defines the 'Build' stage
      steps { // Specifies the steps to be executed within this stage
        sh 'mvn clean install' // Runs the Maven command to clean and build the project
      }   
    }

    stage ('docker build') { // Defines the 'docker build' stage
      steps { // Specifies the steps to be executed within this stage
        sh 'docker build -t javulna-0.1 .' // Builds a Docker image with the specified tag
      }   
    }
    stage ('docker run container') { // Defines the 'docker run container' stage
      steps { // Specifies the steps to be executed within this stage
        sh 'docker stop app || true' // Stops any running container with the name 'app'
        sh 'docker rm app || true' // Removes the container with the name 'app' if it exists
        sh 'docker run --name app -it -d -p 9001:8080 javulna-0.1' // Runs a new Docker container named 'app' based on the 'javulna-0.1' image, with port mapping from 8080 to 9000, in detached mode (-d), and allocates a pseudo-TTY (-it)
     }
   }
 }
}
 
