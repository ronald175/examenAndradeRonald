Instalación de la imagen
Ronald Andrade


Nos descargamos la imagen desde docker hub:
docker pull ronaldandrap17/exameninterciclo_andraderonald
Levantamos el contenedor con el siguiente comando:
docker run -d -p ${PORT}:80 -v ${PATH}:/app/front --name ${CONTAINER_NAME} ronaldandrap17/exameninterciclo_andraderonald
Donde:

${PORT}: es el puerto en donde deseamos levantar nuestra aplicación. En caso de no colocarlo se levantará en el puerto 80.
${PATH}: ruta donde tenemos nuestro frontend en Angular.
${CONTAINER_NAME}: nombre de nuestro contenedor.
Nota: en caso de realizar modificaciones en el frontend, es necesario reiniciar el contenedor. Para ello usamos el comando: docker restart ${CONTAINER_NAME}
Dockerfile

Este proyecto utiliza la última imagen disponible de ubuntu en docker hub.

Funcionamiento

Partimos de la imagen de ubuntu
FROM ubuntu:latest
Dentro de la imagen actualizamos los paquetes de ubuntu y realizamos la instalación de los paquetes curl y gnupg para poder realizar la instalación de NodeJS en los pasos siguientes.
RUN apt-get update
RUN apt-get install -y curl gnupg
Descargamos NodeJS en su versión 22.x y procedemos a instalarlo mediante bash
RUN curl -sL https://deb.nodesource.com/setup_22.x | bash -
RUN apt-get -y install nodejs
Realizamos la instalación de nginx con el manejador de paquetes de ubuntu
RUN apt-get install -y nginx
Instalamos el CLI de Angular mediante npm
RUN npm install -g @angular/cli
Cargamos la configuración de nginx para servir nuestra aplicación de Angular. Esta configuración la copiamos desde el repositorio y la colocamos en la carpeta de configuración de nginx. Además, eliminamos el sitio por defecto que se genera al instalar nginx.
COPY nginx.conf /etc/nginx/sites-available/default
RUN rm -rf /var/www/html/*
Creamos el volumen para nuestro front en la ruta /app/front y nos ubicamos en esa ruta.
VOLUME /ExamenInterciclo_AndradeRonald/appRonald
WORKDIR /ExamenInterciclo_AndradeRonald/appRonald
Ejecutamos los comandos para instalar las dependencias de nuestro proyecto, compilar y copiar los archivos generados a la carpeta de nginx. Finalmente, levantamos nginx en modo silencioso y servimos nuestra aplicación.
CMD ["bash", "-c", "npm install --force && ng build && cp -r /ExamenInterciclo_AndradeRonald/appRonald/dist/app-ronald/* /var/www/html && nginx -g 'daemon off;'"]
EXPOSE 80
Creación de imagen

Para construir nuestra imagen basándonos en el Dockerfile nos situamos en la raíz del repositorio y ejecutamos el siguiente comando:

docker build -t ronaldandrap17/exameninterciclo_andradeRonald .
