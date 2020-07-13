# Introduction

This project is a template for 'dockerized' web app project using Angular for the front-end server, NodeJS for back-end and MySql for database. Therefore, not all the contents in this template are necessary/relevant and should be deleted/replaced based on the new project requirements, _**however**_  the file-structure cannot be changed in order for the docker environment to launch correctly. Therefore, please adhere to the instructions per folder.

## File Structure Requirements   

- The _client_ and _server_ folders must include a package.json file with all the client/server's dependenicies, directly under the app folder, ie. client/app/package.json. 
-  The _docker_ folder and all its contents must remain **AS IS**. 
- Any SQL configurations that are needed at start up, for instance databases, tables, initialization data, must be included in the _mysql_ folder. The file that is there curentl y can be edited or removed.
- The _setup_ folder and all its contents must remain **AS IS**.
- All the files directly under the project root must remain **AS IS**.

How to launch a docker developer environment
--
Assuming docker is installed on your local machine, run the following command in your terminal:

`docker-compose -f docker-compose-dev.yml up -d`

If docker launched successfully, your angular server can be found at [http://localhost:4200](), and your NodeJS server at [http://localhost:8080](), and mysql server at [http://localhost:3306]().

**Please note, like in all docker projects, all internal api calls, i.e, server to server, must be referenced by their docker service name followed by their port number, for example http://server-node:8080. The service names are the names under 'services' in the docker-compose-dev.yml file.
This is unlike browser calls to the front-end server which will to the localhost, not the service.

## How to launch a docker production environment - _locally_.

- Make sure all the services in the dev environment are down by running
 
    `docker-compose -f docker-compose-dev.yml down`.
- Run `docker-compose -f docker-compose-prod.yml up -d`
- Check your app is running at your [localhost](http://localhost), which is default port 80.
 
## How to launch a docker production environment -on a _remote server_.
- Uncomment all lines in the nginx\nginx-prod\ng-prod.conf file.
- Comment out lines 11 and 19.
- Edit the domain in setup/prod/api-config.json to the correct remote vm domain.
- Do the same in lines 14,15,18 and 46  in the docker/nginx-prod/ng-prod.conf file.
- On the production server, from the project root run 
    
    `python3 setup/prod/setup.py`
- In a browser verify the project is accessible with HTTP and HTTPS.