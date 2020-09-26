# Инструкция
1) Клонируем проект - git clone https://github.com/spring-projects/spring-petclinic.git
2) Устанавливаем и запускаем Docker. Устанавливаем привелегии с помощью команды net localgroup docker-users DOMAIN\USERNAME /add
3) Устанавливаем gcloud и входим как пользователь, который будет запускать команды Docker с помощью команды gcloud auth login
4) Создаем платежный google аккаунт, затем создаем проект в GCR и включаем Container Registry API.
5) Указываем gcloud на созданный проект c помощью команды gcloud config set inspired-frame-290418
6) Создаем сервисный аккаунт и идентификатор ключа, затем загружаем ключ и вводим команду gcloud auth activate-service-account servacc@inspired-frame-290418.iam.gserviceaccount.com --key-file=C:\Users\User\Desktop\jsonkey\inspired-frame-290418-a4cc049cd0d3.json
7) Настраиваем Docker с помощью команды gcloud auth configure-docker
8) Создаем диск в GCR Compute engine, a затем образ.
9) Указываем в pom-файле google jib плагин и образ GCR проекта в тэге image:
    ```
    <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <version>2.5.2</version>
        <configuration>
          <from>
             <image>openjdk:alpine</image>
          </from>
          <to>
            <image>gcr.io/inspired-frame-290418/image1</image>
          </to>
        </configuration>

        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>build</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      ```
10) Чтобы изменить версию JDK и/или образ (по умолчанию стоит gcr.io/distroless/java) нужно найти тэг образа image в pom-файле 
``` <image>openjdk:alpine</image>```  и заменить ее на ```<image>bellsoft/liberica-openjdk-debian</image>``` или на любой другой образ/JDK, который можно найти в DockerHub.
11) Компилируем проект - mvn compile jib:build
12) Скачиваем образ из хранилища по тегу, путем комманды - docker pull gcr.io/inspired-frame-290418/image1:latest (???)
13) Проверяем, работает ли проект? Запускаем контейнер Docker с помощью команды docker run -d -p8080:8080 gcr.io/inspired-frame-290418/image1
