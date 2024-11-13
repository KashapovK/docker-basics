# Docker for IT Pros and System Administrators

## Stage 1

docker container run hello-world  
Контейнер Привет Мир!  
![image](https://github.com/user-attachments/assets/65084460-82ad-445f-8586-fde120838463)  

docker image pull alpine  
docker image ls  
docker container run alpine ls -l  
![image](https://github.com/user-attachments/assets/1f0be407-15e5-43d3-8d6e-fb5c89e9873a)  

docker container run alpine echo "hello from alpine"  
docker container run alpine /bin/sh  
docker container run -it alpine /bin/sh  
docker container ls  
docker container ls -a  
![image](https://github.com/user-attachments/assets/03695691-7858-4289-b26d-cf917c6f6cdc)  

docker container run -it alpine /bin/ash  
echo "hello world" > hello.txt  
ls  
docker container run alpine ls  
![image](https://github.com/user-attachments/assets/698cf970-4581-41e3-aac2-8a3c40429544)  

docker container ls -a  
docker container start 95b4  
docker container ls  
docker container exec 95b4 ls  
![image](https://github.com/user-attachments/assets/59536901-5107-477c-a29d-93b20dfe5dc4)  


