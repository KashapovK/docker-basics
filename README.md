# Docker for IT Pros and System Administrators

## Stage 1

### Your First Linux Containers

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

### Customizing Docker Images

docker container run -ti ubuntu bash  
apt-get update  
apt-get install -y figlet  
figlet "hello docker"  
exit  
![image](https://github.com/user-attachments/assets/e89e1aa9-fe4f-4798-b587-6ab3385bca52)  

docker container ls -a  
docker container commit CONTAINER_ID  
docker image ls  
docker image tag <IMAGE_ID> ourfiglet
docker image ls  
docker container run ourfiglet figlet hello  
![image](https://github.com/user-attachments/assets/19523afb-ee7b-4f14-a174-7fbd7089c9d7)

docker image build -t hello:v0.1 .  
![image](https://github.com/user-attachments/assets/62a30f18-7559-4021-8275-828384b81dda)

docker image ls  
docker image history 54b1  
echo "console.log(\"this is v0.2\");" >> index.js  
docker image build -t hello:v0.2 .  
![image](https://github.com/user-attachments/assets/f82510a5-0ea5-410c-8028-8d8168673522)  

docker image pull alpine  
docker image inspect alpine  
docker image inspect --format "{{ json .RootFS.Layers }}" alpine  
![image](https://github.com/user-attachments/assets/15e50267-bc41-4ad1-9a10-aec7e6b7d096)  

### Deploy and Managing Multiple Containers

docker swarm init --advertise-addr $(hostname -i)  
docker swarm join --token SWMTKN-1-1rmh2qrvympy5yebwv8ezxtg9xipyvo7ber6nfv08pw7bby4v6-796q0htdgwd120b5lx6620vj6 192.168.0.12:2377  
docker node ls  
git clone https://github.com/docker/example-voting-app  
cd example-voting-app  
docker stack deploy --compose-file=docker-stack.yml voting_stack  
docker stack ls  
![image](https://github.com/user-attachments/assets/44865d2a-1cde-45e5-b7c9-8645f9fa46d7)  

docker stack services voting_stack  
docker service ps voting_stack_vote  
docker service scale voting_stack_vote=5  
![image](https://github.com/user-attachments/assets/552bc743-82f9-4578-bf0f-e6a1a6257d47)  

## Stage 2

### Security Lab: Seccomp

git clone https://github.com/docker/labs  
cd labs/security/seccomp  
docker run --rm -it --cap-add ALL --security-opt apparmor=unconfined --security-opt seccomp=seccomp-profiles/deny.json alpine sh  
cat seccomp-profiles/deny.json  
docker run --rm -it --security-opt seccomp=unconfined debian:jessie sh  
![image](https://github.com/user-attachments/assets/b4a461b9-924a-4511-9804-f02cf645d894)  

whoami  
unshare --map-root-user --user  
whoami  
exit  
exit  
apk add --update strace  
strace -c -f -S name whoami 2>&1 1>/dev/null | tail -n +3 | head -n -2 | awk '{print $(NF)}'  
strace whoami  
![image](https://github.com/user-attachments/assets/c6a502ab-6fb2-41a8-bb5d-3cdab987cc63)  

docker run --rm -it --security-opt seccomp=./seccomp-profiles/default-no-chmod.json alpine sh  
chmod 777 / -v  
exit  
docker run --rm -it --security-opt seccomp=./seccomp-profiles/default.json alpine sh  
chmod 777 / -v  
exit  
cat ./seccomp-profiles/default.json | grep chmod  
cat ./seccomp-profiles/default-no-chmod.json | grep chmod  
![image](https://github.com/user-attachments/assets/6cbb499a-cbb5-4ea9-a032-2e701cb65419)  

### Security Lab: Capabilities

docker run --rm -it alpine chown nobody /  
docker run --rm -it --cap-drop ALL --cap-add CHOWN alpine chown nobody /  
docker run --rm -it --cap-drop CHOWN alpine chown nobody /  
docker run --rm -it --cap-add chown -u nobody alpine chown nobody /  
docker run --rm -it alpine sh -c 'apk add -U libcap; capsh --print'  
docker run --rm -it alpine sh -c 'apk add -U libcap;capsh --help'  
![image](https://github.com/user-attachments/assets/e92dff68-a9f2-4882-8849-cff98efd0c86)  

### Docker Networking Hands-on Lab

docker network  
docker network ls  
docker network inspect bridge  
docker info  
![image](https://github.com/user-attachments/assets/836c9661-bb14-41e1-9c15-5620d368506c)

docker network ls  
apk update  
apk add bridge  
brctl show  
ip a  
![image](https://github.com/user-attachments/assets/cebe4b21-b44d-464f-abb9-87064f547d5e)  

docker run -dt ubuntu sleep infinity  
docker ps  
brctl show  
docker network inspect bridge  
![image](https://github.com/user-attachments/assets/52dd5d84-b9a4-4295-9dbe-9c2dc44a1233)  

docker ps  
apt-get update && apt-get install -y iputils-ping  
![image](https://github.com/user-attachments/assets/96aea92b-59fa-4536-aae5-1e25b7070ea8)  

docker run --name web1 -d -p 8080:80 nginx  
docker ps  
curl 127.0.0.1:8080  
![image](https://github.com/user-attachments/assets/811d3dc3-ef84-4845-b0cf-66007ca9c9e8)  

docker swarm init --advertise-addr $(hostname -i)  
docker node ls  
docker network create -d overlay overnet  
docker network ls  
docker service create --name myservice \  
--network overnet \  
--replicas 2 \  
ubuntu sleep infinity  
docker service ls  
![image](https://github.com/user-attachments/assets/d94d2927-fe8c-4a6b-b466-bbf65ba93ca6)  

docker service ps myservice  
docker network ls  
docker network inspect overnet  
docker ps  
apt-get update && apt-get install -y iputils-ping  
![image](https://github.com/user-attachments/assets/2e3d7bef-fe22-4f47-afbb-5756674b318a)  

cat /etc/resolv.conf  
docker service inspect myservice  
docker service rm myservice  
docker ps  
docker swarm leave --force  
![image](https://github.com/user-attachments/assets/0377204b-c4ff-48da-9006-80fcfa30d69c)  

### Docker Orchestration Hands-on Lab

docker run -dt ubuntu sleep infinity  
docker ps  
docker swarm init --advertise-addr $(hostname -i)  
docker swarm join --token SWMTKN-1-53t7ru988l2h1j6cy2o2lt1aptkyu38rp27ggjxn1yf2sqfcyr-2c5fwwqxjfr76htvp941nwdt6 192.168.0.13:2377  
docker node ls  
![image](https://github.com/user-attachments/assets/648fe215-1812-4973-9ffd-5ce3219d3435)  

docker service create --name sleep-app ubuntu sleep infinity  
docker service ls  
docker service update --replicas 7 sleep-app  
docker service ps sleep-app  
![image](https://github.com/user-attachments/assets/660a93ef-791c-4560-96ac-7ef98956a85f)  

docker service update --replicas 4 sleep-app  
docker service ps sleep-app  
docker node ls  
![image](https://github.com/user-attachments/assets/809c25b9-1ac7-4c71-b84f-0f08620cd827)  

docker service rm sleep-app  
docker ps  
docker swarm leave --force  
![image](https://github.com/user-attachments/assets/593dbf24-dbde-4dbd-af9c-04d21bfd7d68)  





