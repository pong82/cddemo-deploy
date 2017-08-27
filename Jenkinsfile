 node("docker-test") {

     stage("Deploy Test"){
        sh "echo 'This is the start'"
        sh "ls"
        docker.withRegistry('', 'docker-login') {
      
        }
        def phpsample = docker.image('pong645/php-sample:latest')
        phpsample.pull()
     }

    stage("Deploy"){
        sh '''
            GREENVER="53"
            BLUEVER="latest"
            ENV=$(curl -s http://cdtest.aimail.me/app1/env.html|grep "green"|wc -l)
            if [[ "$ENV" -eq 0 ]]; then
                SERVICES=$(docker service ls --filter name=app1green --quiet | wc -l)
                if [[ "$SERVICES" -eq 0 ]]; then
                    docker service create --name app1green -p81:80 pong645/php-sample:"$GREENVER"
                    sleep 2
                    CONTAINER=$(docker ps | grep app1green | cut -c 1-12)
                else
                    docker service update --image pong645/php-sample:"$GREENVER" app1green
                    sleep 2
                    CONTAINER=$(docker ps | grep app1green | cut -c 1-12)
                fi
                echo "green">env.html
                docker cp env.html "$CONTAINER":/var/www/html/
                docker service ps -f 'Desired-State'=Running app1green|grep php-sample|sed 's#  #<br>#g'>status.html
                docker cp status.html "$CONTAINER":/var/www/html/
            else
                SERVICES=$(docker service ls --filter name=app1blue --quiet | wc -l)
                if [[ "$SERVICES" -eq 0 ]]; then
                    docker service create --name app1blue -p82:80 pong645/php-sample:"$BLUEVER"
                    sleep 2
                    CONTAINER=$(docker ps | grep app1blue | cut -c 1-12)
                else
                    docker service update --image pong645/php-sample:"$BLUEVER" app1blue
                    sleep 2
                    CONTAINER=$(docker ps | grep app1blue | cut -c 1-12)
                fi
                echo "blue">env.html
                docker cp env.html "$CONTAINER":/var/www/html/
                docker service ps -f 'Desired-State'=Running app1blue|grep php-sample|sed 's#  #<br>#g'>status.html
                docker cp status.html "$CONTAINER":/var/www/html/

            fi
        '''
     }
 }