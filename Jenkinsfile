 node("docker-test") {

     stage("Deploy Test"){
        sh "echo 'This is the start'"
        sh "ls"
        docker.withRegistry('', 'docker-login') {
      
        }
        def phpsample = docker.image('pong645/php-sample:latest')
        phpsample.pull()
     }
 }