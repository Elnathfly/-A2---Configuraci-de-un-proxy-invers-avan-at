# Memoria de la Práctica: Proxies Inversos con Nginx

<p align="center">
  <img src="html/images/logo.svg" width="180" alt="Logo del Proyecto">
</p>

**Asignatura:** 0378 SAD - Administración de Sistemas Operativos en Red  
**Centro:** Institut El Calamot  
**Alumno:** Elnath Lavarino  
**Fecha:** 17 de abril de 2026

---

## 1. Introducción
En esta práctica he desarrollado una infraestructura de red utilizando **Docker Compose** para implementar un proxy inverso con Nginx. El objetivo es gestionar el tráfico hacia dos servidores Apache, aplicando balanceo de carga y optimizando el rendimiento mediante una memoria caché (cache).

## 2. Objetivos de la Práctica
- Desplegar contenedores de Apache y Nginx de forma coordinada.
- Implementar un volumen compartido para mantener la integridad del contenido web.
- Configurar el balanceo de carga tipo Round Robin.
- Habilitar y verificar el sistema de proxy cache de Nginx.

## 3. Desarrollo de la Infraestructura

### 3.1. Configuración del Web Server (Apache)
He utilizado dos nodos llamados `web1` y `web2`. Ambos montan la carpeta local `./html` en el path `/usr/local/apache2/htdocs/`. De esta manera, cualquier cambio en el archivo `index.html` se refleja inmediatamente en los dos servidores. El contenido incluye texto personalizado, estilos CSS, una imagen SVG creada para la ocasión y un vídeo MP4.

### 3.2. Configuración del Proxy (Nginx)
El archivo `nginx.conf` es el corazón del sistema. He definido un `upstream` que apunta a los dos nodos de Apache. Además, he configurado:
- **Proxy Cache:** He definido una zona de memoria de 10MB y un directorio de caché (`/var/cache/nginx`) vinculado a mi carpeta local `./nginx/cache`.
- **Cabeceras Personalizadas:** He añadido las cabeceras `X-Backend-IP` (para saber qué nodo responde) y `X-Cache-Status` (para ver el estado de la caché: HIT/MISS).

## 4. Evidencias y Verificación

A continuación se muestran las capturas de pantalla que demuestran el correcto funcionamiento del sistema según los requisitos:

### 4.1. Visualización de la Página Web
En esta captura se puede ver la página web cargada correctamente, con mi nombre, el logo SVG y el reproductor de vídeo preparado.

![Web Funcionando](evidencies/web_funcionant.png)

### 4.2. Verificación del Balanceo de Carga
Utilizando el comando `curl -I`, he comprobado que la cabecera `X-Backend-IP` cambia entre las direcciones internas de los nodos Apache (ej: 172.18.0.2 y 172.18.0.3), demostrando que Nginx distribuye las peticiones.

![Balanceo de Carga](evidencies/balanceig.png)

### 4.3. Funcionamiento de la Memoria Caché
Aquí se demuestra cómo, tras una primera petición (`MISS`), las siguientes son servidas directamente por Nginx desde la caché (`HIT`), mejorando drásticamente el tiempo de respuesta.

![Estado de la Caché](evidencies/cache_status.png)

## 5. Conclusiones y Problemas Encontrados
Durante el desarrollo, el reto principal ha sido la correcta configuración de los permisos del directorio de caché y la sintaxis de las cabeceras personalizadas para forzar el cacheo de contenido estático. Una vez resuelto, el sistema se ha mostrado muy estable. 

El uso de **Docker Compose** ha facilitado enormemente la replicación del entorno de producción en mi equipo local. Esta arquitectura es ideal para entornos reales donde se busca alta disponibilidad y velocidad.

---
*Firmado: Elnath Lavarino*
