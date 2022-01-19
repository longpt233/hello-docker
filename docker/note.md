# 1. Why docker 
- devops
- Solves Dependency Conflicts : 1 server cần nhiều môi trường 
- Scaling Up
- update, backup 

# 2. Install 

- test : ```docker run hello-world```
- note for win : Docker Desktop requires a Professional, Home, Enterprise or Education 64-bit edition of Windows 10, because it is based on Hyper-V.

# 3. Basic concept 

- container : cái đang chạy : chạy trên 1 server. A container is what Docker hosts. 
- images : cái để từ nó chạy ra container. An image is a template used for creating Docker containers
- registries: nơi lưu images 

# 4. Use Docker Image 
- management: 

```bash 
docker ps: lists the containers that are still running. Add the -a switch in order to see containers that have stopped
docker logs: retrieves the logs of a container, even when it has stopped
docker inspect: gets detailed information about a running or stopped container
docker stop: stops a container that is still running
docker rm: deletes a container
```

- docker run: chạy 2 lần câu lệnh ```docker run alpine printenv``` sẽ chạy 2 container khác nhau (lệnh print in ra khác nhau)
- long run : ```docker run -d -p 8085:80 nginx```
> -   -d : detach -> docker logs 84 (id container)
> -   -p map port từ ngoài 8085 vào trong 80 
> -   image nginx
> - nếu image k có câu lệnh thực thi thì sẽ stop ngay container
> - docker run alpine -> thoát ngay 
> - docker run alpine ping www.docker.com -> oke 

- using volume : ```docker run -v /your/dir:/var/lib/mysql -d mysql:5.7```
- registries : public docker hub : <repository_name>/<name>:<tag>
- quiz 

> - docker run = create + run 
> - docker ps -a : liệt kê cả container stop 
> - docker container prune -f : Deletes all the containers which are not in use without prompting for confirmation

# 5. Create 
- simple file : Dockerfile

```
FROM debian:8
CMD ["echo", "Hello world"]
```
- command build ```docker build -t hello:1.0.0 . ```. *note* . context -> or -f Dockerfile.debian-hello

- trong Dockerfile:
> - ADD . . : copy tất cả trong thư mục cùng cấp chạy docker build vào trong image 
> - COPY . . : Same as 'ADD', but without the tar and remote URL handling.
> - ENV host=www.google.com : thiết lập biến môi trường. cách lấy ra env theo từng ngôn ngữ
> - VOLUME /path/to/directory-inside-container : nếu k chỉ định thì khi xóa mất hết. khi chạy thì cần thêm -v 
> - EXPOSE 80 : mở port 80, là hình thức ghi cho vui chứ lúc chạy bắt buộc phải thêm -p 

# 6. Publish Docker Images

- docker build -> docker login -> docker push
- image layer: RUN
- cache layer : pull, push, build 

# 7. Forget SDK Installs
- chạy ngay trên những base image 
- Multi-Stage Dockerfiles: có trò tách nơi build và nơi chạy. ví dụ khi build java thì mình cần môi trường mvn chẳng hạn, với cả đống pack liên quan -> nặng, trong khi đó khi chạy chỉ cần môi trường java 
ví dụ 
```
FROM fat-image AS builder
...

FROM small-image
COPY --from=builder /result .
…
```

- java 

```
# Use an image with the SDK for compilation
FROM openjdk:8-jdk-alpine AS builder
WORKDIR /out
# Get the source code inside the image 
COPY *.java .
# Compile source code
RUN javac Hello.java

# Create a lightweight image 
FROM openjdk:8-jre-alpine
# Copy compiled artifacts from previous image
COPY --from=builder /out/*.class .
CMD ["java", "Hello"]
```


- python 
```
FROM python:3.7-stretch

# Install modules
RUN pip install Flask

# Needed by the Flask module
ENV FLASK_APP=server.py

# Copy source files into the image
COPY templates ./templates
COPY server.py .

EXPOSE 5000

CMD ["flask", "run", "--host=0.0.0.0"]
```
- .net core, php, node 

# 9. More 
- monitor : ```docker stats```
- xóa mấy cái k dùng ```docker container prune -f```, ```docker volume prune -f```, ```docker image prune -f```
- chạy tự động, chạy nhiều  Docker Swarm or Kubernetes





 
