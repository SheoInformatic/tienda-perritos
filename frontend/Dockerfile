# 1. Cambiamos a la versión "unprivileged" (sin privilegios) para cumplir la regla de "usuario no root"
FROM nginxinc/nginx-unprivileged:alpine

# 2. Definimos el directorio de trabajo estándar
WORKDIR /usr/share/nginx/html

# 3. Copiamos los archivos del frontend y les asignamos el dueño correcto (nginx) en vez de root
COPY --chown=nginx:nginx index.html app.js ./

# 4. Copiamos la configuración custom de Nginx
COPY --chown=nginx:nginx default.conf /etc/nginx/conf.d/default.conf

# 5. Cambiamos el puerto al 8080. Los usuarios no root no pueden usar el puerto 80.
EXPOSE 8080