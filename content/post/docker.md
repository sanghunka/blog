Docker : docker run --net=host -it -p 8888:8888 image:version

docker run -i -t -p 8888:8888 sanghkaang/bootcamp:0.0.3

Container : jupyter notebook --ip 0.0.0.0 --no-browser



jupyter notebook --ip=0.0.0.0 --no-browser --allow-root

Host : localhost:8888/tree‌


도커 여러 쉘 실행 docker exec -it <container> bash



도커 커밋하는법
docker commit 컨테이너명 이미지명
도커 push
docker push sanghkaang/bootcamp:0.0.5

도커 카피
docker comp 로컬파일 컨테이너주소: