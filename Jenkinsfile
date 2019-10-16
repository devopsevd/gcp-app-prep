node("AppServer") {
        stage("remove old containers"){
          sh "sudo docker ps -a -q --filter=ancestor=devopsevd/nginx_img --filter=ancestor=gcr.io/techlab-235821/sboot:v2.0-orange --filter=ancestor=devopsevd/sboot:v1.0-blue --filter=ancestor=devopsevd/sboot_demo_pg --filter=ancestor=redis | xargs -r sudo docker rm -f" 
        }
        stage("Start containers - Orange"){   
sh "sudo docker run -d --restart always --env=POSTGRES_PASSWORD=demo_pass  -p 5432:5432  --name sboot_demo_pg devopsevd/sboot_demo_pg"
sh "sudo docker run -d --restart always -p 6379:6379 --name redis_db redis"
sh "sudo docker run -d --name v1.0-blue1 -p 8180:8180 -e app.version='v1.0-blue-node1'  --link sboot_demo_pg:sboot_demo_pg devopsevd/sboot:v1.0-blue"
sh "sudo docker run -d --name v1.0-blue2 -p 8280:8180 -e app.version='v1.0-blue-node2'  --link sboot_demo_pg:sboot_demo_pg devopsevd/sboot:v1.0-blue"
sh "sudo docker rm -f v1.0-blue1"
sh "sudo docker run -d --restart always --name v2.0-orange1 -p 8180:8180 -e app.version='v2.0-orange-node1' --link sboot_demo_pg:sboot_demo_pg --link redis_db:redis_db gcr.io/techlab-235821/sboot:v2.0-orange"
sh "sudo docker rm -f v1.0-blue2"
sh "sudo docker run -d --restart always --name v2.0-orange2 -p 8280:8180 -e app.version='v2.0-orange-node2'  -e liquibase.parameters.cleanupRun='true' --link sboot_demo_pg:sboot_demo_pg --link redis_db:redis_db gcr.io/techlab-235821/sboot:v2.0-orange"
sh "sudo docker run -P -d --restart always --name ng -p 80:80 devopsevd/nginx_img"
        }       
       
}
