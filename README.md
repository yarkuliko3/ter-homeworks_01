# ter-homeworks_01
ter-homeworks/01

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте. 
2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?
3. Выполните код проекта. Найдите  в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.
6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. В качестве ответа дополнительно приложите вывод команды ```docker ps```.
8. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**. 
9. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **обязательно** подкрепите строчкой из документации [**terraform провайдера docker**](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs).  (ищите в классификаторе resource docker_image )
### Ответ

2. personal.auto.tfvars
                  
![2](https://github.com/yarkuliko3/ter-homeworks_01/blob/main/Screenshot%20from%202023-10-12%2014-43-23.png?raw=true)

3. "result": "KJ7gJkuASP9s9Ni6"

4. В блоке "docker_image" отстутствовал один label;
   в блоке "docker_container" второй label начинался с цифры, а должен с буквы или подчеркивания, также в строках были лишние слова.

5.
```
yaroslav@yar-linx:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
04b74811b8b2   nginx:latest   "/docker-entrypoint.…"   8 seconds ago   Up 7 seconds   0.0.0.0:8000->80/tcp   example_KJ7gJkuASP9s9Ni6
```
```
resource "docker_image" "nginx" {
  name = "nginx:latest"
  keep_locally = true
}


resource "docker_container" "nginx" {
  image = "nginx:latest"
  name = "example_${random_password.random_string.result}"
    ports {
      internal = 80
      external = 8000
    }
}
```
6.
```
yaroslav@yar-linx:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
5af91496b0c1   nginx:latest   "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:8000->80/tcp   hello_world
```
Использование -auto approve автоматически применяет конфиг без ознакомления с планом. Такая опция может быть использована только в некритической инфраструктуре, так как может привести к непредвиденным ошибкам, поломкам и простою сервиса.

7.
```
yaroslav@yar-linx:~$ cat terraform.tfstate
{
  "version": 4,
  "terraform_version": "1.5.5",
  "serial": 13,
  "lineage": "a23bf77b-d7e7-36d2-17c2-d6faba6944aa",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```

8. Образ не был удален, так как была использована опция -keep_locally
   
   `keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.`

------

## Дополнительное задание (со звёздочкой*)

**Настоятельно рекомендуем выполнять все задания со звёздочкой.** Они помогут глубже разобраться в материале.   
Задания со звёздочкой дополнительные, не обязательные к выполнению и никак не повлияют на получение вами зачёта по этому домашнему заданию. 

### Задание 2*

1. Изучите в документации provider [**Virtualbox**](https://docs.comcloud.xyz/providers/shekeriev/virtualbox/latest/docs) от 
shekeriev.
2. Создайте с его помощью любую виртуальную машину. Чтобы не использовать VPN, советуем выбрать любой образ с расположением в GitHub из [**списка**](https://www.vagrantbox.es/).

В качестве ответа приложите plan для создаваемого ресурса и скриншот созданного в VB ресурса. 

------
