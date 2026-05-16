# ==========================================
# ETAPA 1: BUILDER (Construcción)
# ==========================================
# Usamos una imagen de Node.js ligera (alpine) para instalar dependencias
FROM node:18-alpine AS builder

# Directorio de trabajo temporal
WORKDIR /app

# Copiamos el package.json para instalar las dependencias
COPY package.json ./

# Instalamos solo las dependencias necesarias para producción (limpieza de capas)
RUN npm install --omit=dev

# Copiamos el código fuente de la aplicación
COPY server.js ./

# ==========================================
# ETAPA 2: RUNNER (Ejecución - Imagen Final)
# ==========================================
# Iniciamos una nueva imagen desde cero, abandonando toda la basura de la etapa anterior
FROM node:18-alpine AS runner

# Definimos el directorio de trabajo final
WORKDIR /app

# Por seguridad (mínimo privilegio), le decimos a Docker que use el usuario 'node' 
# Este usuario ya viene creado por defecto en las imágenes oficiales de Node.js, no es root.
USER node

# Copiamos SOLO lo estrictamente necesario desde la etapa 'builder'
# Usamos --chown=node:node para que el usuario no root tenga los permisos correctos
COPY --from=builder --chown=node:node /app/node_modules ./node_modules
COPY --from=builder --chown=node:node /app/package.json ./
COPY --from=builder --chown=node:node /app/server.js ./

# Exponemos el puerto 3001. 
# ¿Por qué el 3001? Porque en el default.conf del Frontend vimos que busca al backend en ese puerto.
EXPOSE 3001

# Comando para levantar el servidor Node.js
CMD ["node", "server.js"]