# live-deployment-for-node-application
this document helps you deploy a node application with github action to digitalOcean or aws  


clone the code to your server through git 
navigate to the desired directory and clone with 
git clone #link to your replo#>
configur git credentioals globally through your public access key and user name 
# to store git credentials run the following command 

git config --global credential.helper store
after running the commang you can run git clone, git fetch, get pull once the command is executed it will ask for the credentials please provide your
github username
and personal access token
the personal access token can b found in your github web account go to settings/ developer settings / personal access token / tokens 

      create a git hub action and add an yml file with the configration to help git hub action in deployment 


      make sure you have installed node npm and nginx cetbot and pm2 servive 
      mongodb or mysql if the application is using a local database 


      for the first time it is recomended to deploy the application manually once deployed and runing it will be easier to hander the deployment through github actions

to install nginx
```sudo apt update
sudo apt install nginx
sudo ufw app list
sudo ufw allow 'Nginx Full'
sudo ufw allow 'OpenSSH'
sudo ufw status
sudo nginx -t
sudo nginx -s reload```


create a .js file for nginx and upload the file to ../etc/nginix/siteavailible
example content for a node application 
server {
    listen 80;
    server_name {{domain or subdomain}} ;
    root /var/www/yourapp (path to for code in server);
    location / {
        proxy_pass http://localhost:8022;  //port can be changed to a desired one 
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Original-For $server_addr;
        proxy_cache_bypass $http_upgrade;
    }
}

after uploading the yourapp.js file to ../etc/nginix/siteavailible 
sudo ln -s /etc/nginx/sites-available/{the name of the nginx file} /etc/nginx/sites-enabled/
sudo nginx -t
sudo nginx -s reload


# to update the nginx 
sudo rm /etc/nginx/sites-enabled/api.propverse
# appling certibot
sudo certbot --nginx -d {{domain or sub-domain}}
# auto renew
sudo certbot renew --dry-run
sudo chmod 777 -R /var/www/yourapp

after checking the all the above instrctions configured properly go to the project directory form putty and run 
# to add the service in pm2 process
pm2 start npm --name "{{name of your app}}" -- start


# to install node to a spacific version 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm list-remote
nvm install v16.14.0
nvm list
node -v
# removing node form server 
sudo apt remove nodejs
sudo apt purge nodejs
nvm current
nvm uninstall node_version
nvm deactivate



