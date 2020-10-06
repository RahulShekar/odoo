branch=$(echo "$GIT_BRANCH" | sed 's/origin\///g')
tag=$(echo "$GIT_BRANCH" | sed 's/[^0-9]*//g')
echo "branch=$branch, tag=$tag"
git clone  https://https://github.com/RahulShekar/odoo.git/http://172.30.11.224:8069/ $tag
if [ $? -eq 0 ]; then
    cd $tag
    sudo touch .env
    sudo echo "tag=$tag" > .env
    cd frontend
    sudo touch .env
    sudo echo -e  "DANGEROUSLY_DISABLE_HOST_CHECK=true\nREACT_APP_TAG=$tag" > .env
    cd ..
    sudo docker container inspect proxy > /dev/null 2>&1 &&  sudo docker rm -f proxy
   [[ $(sudo docker ps -f "name=proxy" --format '{{.Names}}') == "proxy" ]] || sudo docker run --network='nginx-proxy' --name proxy -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
   [[ $(sudo docker ps -f "name=db" --format '{{.Names}}') == "db" ]] || sudo docker run --network='nginx-proxy' --name db -d -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres postgres:10
    sudo docker-compose build
    sudo docker-compose up -d
else
    cd $tag
    git pull
    sudo docker-compose down
    sudo docker-compose build
    sudo docker container inspect proxy > /dev/null 2>&1 &&  sudo docker rm -f proxy
   [[ $(sudo docker ps -f "name=proxy" --format '{{.Names}}') == "proxy" ]] || sudo docker run --network='nginx-proxy' --name proxy -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
   [[ $(sudo docker ps -f "name=db" --format '{{.Names}}') == "db" ]] || sudo docker run --network='nginx-proxy' --name db -d -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres postgres:10
    sudo docker-compose up -d
fi
