Problem Statement

- Environment Issue : Environment is not same like - node, mongodb, redis version is   may be different for different developers laptops

- If we have multiple environment , then we are facing issue in replicating the environment

eg: Diet_dashboard uses Node 20.0.1
    Jaam-E-Insan uses Node 22.0.1

    so if the user using 20.0.1 in local system then they may face issues to run Jaam-E-Insan    


How Docker solve this problem ? 

 - It create seperate containers for specific projects   

 -          : A container can have environment for particular one (HARDWARE)
            : Images is like software which runs the container (SOFTWARE)

       NOTE : We can run single image in different containers like single os in multiple laptops     

Difference between Docker vs Virtual Machine

  - Docker is Kernal level virtualization (Low level virtualization)
  - Virtual Machine is Operating System Level virtualization (High level virtualization)

Difference between Docker daemon vs Docker Desktop

  - Docker daemon is core or heart of Docker concept (Actual all dockerazation is decided by daemon)
  - Docker Desktop is the GUI


Steps to install any image in particular container eg. ubuntu image 

        docker run -it ubuntu

            - initally ye local machine me find karega agar nahi milega to install hoga 'hub.docker.com' se,  if it exist to usse use karke new container bana dega



        docker container ls 

            - to see the total running container in a system

        docker container ls -a 

            - all the containers ( running+disabled)

        docker start <Container_Name>

            - to start the container

        docker stop <Container_Name>

            - to stop the container 

        docker exec -it <Container_Name> ls

            - to enter in the container   

        docker images

            - to see the images


    NOTE:  Organizations like Amazon can create image for particular project and they can share with the developers and developer can use that images to run the project   



    Port Mapping in Container : exposing the port

        - Container me run ho rahe project ke port ko expose karne hote hai , kyoki use sirf hum container ke andar hi bas access kar paate hai , globally(outside the container) access karne ke liye expose karte hai


        - Exposing the Port of container in outside the container

            docker run -it -p 1025 : 1025

                    (globally jiss port me chalane hai) : (container ke andar ka port)  

    How we Pass Env in docker container 

            docker run -it -p 1025:1025 -e key=value -e key=value



    How to dockerise any application 

        Step 1 : Dockerfile create karenge without any extension to creat a image for particular app

                  Dockerfile  
    
    Inside Dockerfile
       
       Step 2 :  -  base image choose karenge  like : ubuntu  then uske andar 
                    nodejs install kar sakte hai 

                        FROM ubuntu

                Step 2.1(optional) :  update check kar lenge

                        RUN apt-get update

                Step 2.2(optional) : curl install karenge

                        RUN apt-get install -y curl

                Step 2.3 : Nodejs particular version install karenge on top of ubuntu 

                    RUN curl -sL https://deb.nodesource.com/setup_18.X | bash -   

                Step 2.4(optional) : update karenge

                          RUN apt-get upgrade -y

                Step 2.5 : Nodejs run karenge

                          RUN apt-get install -y nodejs 

                Step 2.6 : code copy karenge
                          COPY <source> <destination> like

                                COPY  package.json package.json
                                COPY  package-lock.json package-lock.json
                                COPY  index.js index.js  

                Step 2.7 : npm install taaki node modules generate ho

                            RUN npm install 

                Step 2.8 : entry point de denge 

                            ENTRYPOINT ["node", "index.js"]

                Step 2.9(Finally) : Finally docker image build karenge iss particular app ke liye

                       terminal me 

                            docker build -t    testing-node.js  .

                Step 2.10 : Run karenge image ko to run the app

                      terminal me

                            docker run -it testing-node.js   


                PROBLEM :  app is running but not accessable because port mapping is not done

                Step 2.11 : port mapping karenge access karne ke liye outside the container

                            docker run -it -p 8000:8000 testing-node.js

                Step 2.12 : agar container ke andar kya hai dekhne hai to

                        dusre terminal me

                            docker exec -it <container_id> bash

                        then ls kar sakte hai dekhne ke liye    

                                

                 - Directly nodejs image bhi de sakte hai (Mostly we prefer it)


                    Dockerfile me

                        FROM node
                        COPY package.json package.json
                        COPY package-lock.json package-lock.json
                        COPY index.js index.js
                        RUN npm install
                        ENTRYPOINT ["node","index.js"]

    Caching in Docker

        - agar hum same Dockerfile fir se run karenge and agar sirf index.js file me change hai to baaki previous wali already cache se utha lega, which saves our time


What is DockerCompose

    - It helps to run multiple container, setup and run the containers 

    eg. Mongodb run in different port, Reddis run in different, node in different


    Step 1 : docker-compose.yml file create karenge folder directory me

    Step 2 : Inside docker-compose.yml

                version: '3.8'

                services : 
                    postgres:
                        image: postgres   # ye hub.docker.com se image laayega

                        ports:
                            - '5432:5432'

                        environment : 
                            POSTGRES_USER: postgres
                            POSTGRES_DB: review
                            POSTGRES_PASSWORD: password    

                    redis:
                        image:redis

                        ports:
                            - '6379:6379'   

        Step 3 : Terminal se run  karenge containers ko

                    docker compose up   


                NOTE : agar kabhi error aaye "Permission denied" then use another command in terminal

                    sudo docker compose up
                    
                                              






                    






                    
                     







































