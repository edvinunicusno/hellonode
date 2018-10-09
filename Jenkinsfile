node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Check Node Status') {
        sh 'echo "Heisan Hoppsan"'
        sh '/usr/bin/hostname '
    }
    
    stage('Check Docker') {
        sh '/usr/bin/hostname '
        sh 'which docker'
        sh 'docker version'
    }

    
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        
        app = docker.build("edvinunicusno/hellonode")
    }  

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials-edvin') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
