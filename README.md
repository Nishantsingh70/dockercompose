# dockercompose

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.Docker-compose is like an infrastructure as code.

Using Compose is basically a three-step process:

1)Define your app’s environment with a Dockerfile so it can be reproduced anywhere.

2)Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.

3)Run docker-compose up and Compose starts and runs your entire app.

This is my Dockerfile which contain all the isolated environment for mysql:5.6 and Joomla.
      
     
    version: '3'
    services:
        joomladbos:
               image: mysql:5.6          
               restart: always     
               environment:
                       MYSQL_ROOT_PASSWORD: rootpass
                       MYSQL_USER: 123
                       MYSQL_PASSWORD: 56677
                       MYSQL_DATABASE: mydb     
               volumes:
                       - mysql_storage_new:/var/lib/mysql   
        joomlaos:
               image: joomla
               restart: always
               depends_on: 
                   - joomladbos
               links:
                   - joomladbos:mysql      
               ports:
                   - 8080:80    
               environment:
                       JOOMLA_DB_HOST: joomladbos
                       JOOMLA_DB_USER: 123
                       JOOMLA_DB_PASSWORD: 56677
                       JOOMLA_DB_NAME: mydb    
               volumes:
                       - joomla_storage_new:/var/www/html
    volumes:
        joomla_storage_new:
        mysql_storage_new:

         
                  
By launching the complete setup of Joomla and mysql:5.6  type the command:
    
    docker-compose up
     
This command do the following task:

* Creaing the volume(persistent) for mysql as well as Joomla
* Launching the containers(One container launch by the mysql image and second container launch by the Joomla image)
* After this they mounting the storage.
* They do patting also for accessing the Joomla from outside world.
