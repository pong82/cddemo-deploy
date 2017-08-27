 node("docker-test") {

     stage("Deploy Test"){
        def phpsample = docker.image('pong645/php-sample:latest')
        phpsample.pull()
     }
 }