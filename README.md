# Git repository for the Kubernetes 

пушим к себе на докерхаб образ веб-сервера Node.js 
docker login
docker build . -t peter91peter91/k8s-web-hello-ru:latest -t peter91peter91/k8s-web-hello-ru:1.0.0
docker push peter91peter91/k8s-web-hello-ru --all-tags

echo 'alias k=kubectl' >> ~/.bashrc; source ~/.bashrc
поднимаем миникуб-кластер 
minikube start 
Поднимаем деплоймент из моего образа с докерхаба
k create deploy k8s-web-hello --image=peter91peter91/k8s-web-hello-ru
 k get pods
k describe pod  k8s-web-hello-744588c698-drrjd    или...    k describe pod  k8s-web-hello
смотрим айпи пода
k get pods -o wide

входим в кластер 
minikube ssh

шлем запрос в наш под на порт веб сервера  - curl сформирует HTTP GET запрос
 curl http://10.244.0.11:3000/

открываем туннель 
 minikube tunnel

в другой вкладке запускаем создение СЕРВИСА типа LoadBalancer
k expose deploy k8s-web-hello --type=LoadBalancer --port=3333 --target-port=3000

смотрим наши сервисы-создался сервис с ТИПОМ LoadBalancer
k get svc

меняем число реплик- кол-во подов подстраивается,создается больше эндпоинтов
k scale deploy k8s-web-hello --replicas=7

заходим в браузере,отключаем кеширование,смотрим что балансировщик подставляет разные эндпоинты деплоймента
http://10.100.53.76:3333/
-------------------------------------------------------------------------------
меняем index.mjs и строим новые образы и пушим в мой докерхаб
docker build . -t peter91peter91/k8s-web-hello-ru:latest -t peter91peter91/k8s-web-hello-ru:2.0.0
docker push peter91peter91/k8s-web-hello-ru --all-tags
 
 подменяем образ у нашего деплоймента
 k set image deploy k8s-web-hello k8s-web-hello-ru=peter91peter91/k8s-web-hello-ru:2.0.0

смотрим как происходит обновление 
 k rollout status deploy k8s-web-hello

k describe deploy k8s-web-hello 

--------------------------------------------------------------------
--------------------------------------------------------------------
создать деплоймент из файла
k apply -f deployment.yaml
удалить 
k delete  deploy k8s-web-hello

поднимаем сервис из файла
kubectl apply -f service.yaml

поднимаем дашборд-веб-сервис отобразит графически данные те же что и в терминале
minikube addons enable dashboard
minikube dashboard --url

удаляем все ресурсы поднятые с  файлов
k delete -f deployment.yaml -f service.yaml




--------------------------------------------------------------------
--------------------------------------------------------------------
поднимаем миникуб-кластер 
minikube start 

открываем туннель 
 minikube tunnel

!!!!!!!!!!!!!!!создать 2 деплоймента и 2 сервиса  из файлов
k apply -f k8s-web-to-nginx.yaml -f nginx.yaml
---------
удалить
 k delete -f k8s-web-to-nginx.yaml -f nginx.yaml
узнать почему не поднялось 
kubectl describe deploy k8s-web-to-nginx
kubectl get deploy k8s-web-to-nginx
kubectl get pods -l app=k8s-web-to-nginx
---------










--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
--------------------------------------------------------------------
некоторые из команд для кубера
---------
minikube start 
minikube status

minikube addons enable dashboard
minikube dashboard --url
http://127.0.0.1:35647/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/workloads?namespace=default

alias k = kubectl
k get nodes
k get pods
minikube ssh
k run my-nginx-pod --image=nginx  -создать под нджинкс
k describe pod  my-nginx-pod    -смотреть инфу о поде
k get pods -o wide     -покажет айпи пода и ноду его
k delete pod my-nginx-pod

k scale deploy my-nginx-deploy --replicas=3
k get services - смотреть айпи для доступа по единому айпи к приложению

создать сервис  
k expose deploy my-nginx-deploy --port=8080 —target-port=80