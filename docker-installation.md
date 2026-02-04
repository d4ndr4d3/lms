**Step 1:** Clone the repo

```
$ git clone https://github.com/frappe/lms.git

$ cd lms

$ cd docker
```

**Step 2:** Run docker-compose

```
$ docker-compose up
```

**Step 3:** Visit the website at http://localhost:8000/

You'll have to go through the setup wizard to setup the website for the first time you access it. Login using the following credentiasl to complete the setup wizard.

```
Username: Administrator
password: admin
```

TODO: Explain how to load sample data

## Stopping the server

Press `ctrl+c` in the terminal to stop the server. You can also run `docker-compose down` in another terminal to stop it.

To completely reset the instance, do the following:

```
$ docker-compose down --volumes
$ docker-compose up
```

## Despliegue en Coolify

Para lograr el mismo resultado que en Docker Desktop usando Coolify:

1. **Usa una Aplicación desde GitHub con Docker Compose**
   - Crea un **Proyecto** y dentro una **Aplicación**.
   - Origen: **GitHub** (repositorio `frappe/lms` o tu fork).
   - Build pack: **Docker Compose**.
   - Ruta del archivo compose: **`docker-compose.coolify.yml`** (en la raíz del repo).

2. **Por qué funciona aunque Coolify reemplace volúmenes**
   - Coolify suele cambiar `.:/workspace` por un volumen nombrado (vacío), por eso fallaba `init.sh`.
   - En `docker-compose.coolify.yml` el servicio **frappe** se **construye** con `docker/Dockerfile`: el repo se copia **dentro de la imagen** en `/workspace`. No hay bind mount: `/workspace` viene de la imagen y siempre tiene `docker/init.sh` y el código del app.
   - Sin mapeo de puertos al host se evita "port is already allocated"; el proxy de Coolify enruta por dominio.

3. **Dominio en Coolify**  
   Asigna un dominio al servicio **frappe** y en la configuración del dominio indica el puerto **8000** (p. ej. `http://tudominio.com:8000`) para que el proxy enrute al contenedor.

4. **Credenciales**  
   Tras el primer despliegue: Usuario **Administrator**, Contraseña **admin** (o las que definas por variables de entorno).
