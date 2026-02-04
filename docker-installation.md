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

1. **No uses "Service" con compose pegado (Raw Compose)**  
   El compose monta el código del repo en `/workspace`. En Raw Compose Coolify no clona el repo, así que `/workspace` queda vacío y falla `init.sh`.

2. **Usa una Aplicación desde GitHub con Docker Compose**
   - Crea un **Proyecto** y dentro una **Aplicación**.
   - Origen: **GitHub** (repositorio `frappe/lms` o tu fork).
   - Build pack: **Docker Compose**.
   - Ruta del archivo compose: **`docker-compose.coolify.yml`** (está en la raíz del repo).
   - Asegúrate de que Coolify ejecute el compose desde la **raíz del repositorio**, para que el volumen `.:/workspace` sea el repo completo y exista `docker/init.sh`.

3. **Diferencias del compose para Coolify**
   - En la raíz del repo está `docker-compose.coolify.yml`.
   - Usa volumen `.:/workspace` (raíz del repo) y comando `bash /workspace/docker/init.sh`.
   - Incluye `depends_on` para que `frappe` espere a MariaDB y Redis.

4. **Dominio y puertos en Coolify**  
   Usa `docker-compose.coolify.yml` (sin mapeo de puertos al host). Así se evita el error "port is already allocated". Asigna un dominio al servicio **frappe** y en el dominio indica el puerto 8000, por ejemplo `http://tudominio.com:8000`, para que el proxy enrute al contenedor.

5. **Credenciales**  
   Tras el primer despliegue, usa Usuario: **Administrator**, Contraseña: **admin** (o las que definas por variables de entorno si las configuras).
