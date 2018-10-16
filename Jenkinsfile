node("AppServer") {
        stage("remove old containers"){
          sh "docker ps -a -q --filter=ancestor=devopsevd/nginx_img --filter=ancestor=10.144.2.110:8123/sboot:v2.0-red --filter=ancestor=10.144.2.110:8123/sboot:v1.1-green --filter=ancestor=devopsevd/sboot:v1.1-green --filter=ancestor=devopsevd/sboot:v1.0-blue --filter=ancestor=devopsevd/sboot_demo_pg | xargs -r docker rm -f"      
        }
        stage("Start containers - Blue"){   
          sh "sudo docker run -d --env=POSTGRES_PASSWORD=demo_pass  -p 5432:5432  --name sboot_demo_pg devopsevd/sboot_demo_pg"
          sh "sudo docker run -d --name v1.0-blue1 -p 8180:8180 -e app.version='v1.0-blue-node1'  --link sboot_demo_pg:sboot_demo_pg devopsevd/sboot:v1.0-blue"
          sh "sudo docker run -d --name v1.0-blue2 -p 8280:8180 -e app.version='v1.0-blue-node2'  --link sboot_demo_pg:sboot_demo_pg devopsevd/sboot:v1.0-blue"
          sh "sudo docker run -P -d --name ng -p 80:80 devopsevd/nginx_img"
        }       
       
}
