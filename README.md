Aquí tienes el contenido del `README.md` basado en la información que proporcionaste:

```markdown
# Examen Interciclo - Ronald Andrade

## Instalación de la imagen

1. **Descarga la imagen desde Docker Hub:**

   Ejecuta el siguiente comando para descargar la imagen del contenedor:

   ```bash
   docker pull ronaldandrap17/exameninterciclo_andraderonald
   ```

2. **Levanta el contenedor:**

   Una vez descargada la imagen, ejecuta el siguiente comando para levantar el contenedor:

   ```bash
   docker run -d -p ${PORT}:80 -v ${PATH}:/app/front --name ${CONTAINER_NAME} ronaldandrap17/exameninterciclo_andraderonald
   ```

   **Explicación de los parámetros:**

   - `${PORT}`: Es el puerto en el que deseas levantar la aplicación. Si no especificas un puerto, se levantará en el puerto `80` por defecto.
   - `${PATH}`: La ruta donde se encuentra tu frontend en Angular.
   - `${CONTAINER_NAME}`: El nombre que deseas asignar al contenedor.

   **Nota:** Si realizas modificaciones en el frontend, es necesario reiniciar el contenedor. Puedes hacerlo con el siguiente comando:

   ```bash
   docker restart ${CONTAINER_NAME}
   ```

## Dockerfile

Este proyecto utiliza la última imagen disponible de **Ubuntu** en Docker Hub.

```dockerfile
# Partimos de la imagen base de Ubuntu
FROM ubuntu:latest

# Actualizamos los paquetes de Ubuntu e instalamos curl y gnupg
RUN apt-get update
RUN apt-get install -y curl gnupg

# Instalamos NodeJS versión 22.x
RUN curl -sL https://deb.nodesource.com/setup_22.x | bash -
RUN apt-get -y install nodejs

# Instalamos nginx
RUN apt-get install -y nginx

# Instalamos Angular CLI
RUN npm install -g @angular/cli

# Configuramos nginx para servir nuestra aplicación Angular
COPY nginx.conf /etc/nginx/sites-available/default
RUN rm -rf /var/www/html/*

# Creamos un volumen para nuestro frontend
VOLUME /ExamenInterciclo_AndradeRonald/appRonald

# Establecemos el directorio de trabajo
WORKDIR /ExamenInterciclo_AndradeRonald/appRonald

# Instalamos dependencias, compilamos la app y copiamos los archivos generados a nginx
CMD ["bash", "-c", "npm install --force && ng build && cp -r /ExamenInterciclo_AndradeRonald/appRonald/dist/app-ronald/* /var/www/html && nginx -g 'daemon off;'"]

# Exponemos el puerto 80
EXPOSE 80
```

## Creación de la imagen Docker

Para construir la imagen a partir del `Dockerfile`, sigue estos pasos:

1. Sitúate en la raíz del repositorio donde se encuentra el `Dockerfile`.
2. Ejecuta el siguiente comando:

   ```bash
   docker build -t ronaldandrap17/exameninterciclo_andradeRonald .
   ```

Esto creará la imagen personalizada de tu aplicación, que podrás usar para levantar un contenedor.

## Notas adicionales

- Asegúrate de tener configurado correctamente tu archivo `nginx.conf` y las rutas de tu frontend.
- Si necesitas realizar cambios en el frontend, recuerda reiniciar el contenedor con el comando `docker restart ${CONTAINER_NAME}`.
```

Este archivo `README.md` contiene la información estructurada de manera clara para la instalación, configuración y uso del contenedor. Puedes personalizar cualquier detalle según lo necesites.