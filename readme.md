# Instalación Odoo Community v.17

En esta guia se muestran los pasos a seguir para intalar tres servicios: Odoo, PostgreSQL y PgAdmin

## Creación del archivo compose

<details>
 <summary>Crear carpeta y archivo</summary>
<br>

```bash
# Montar una carpeta para almacenar el archivo compose.yml
mkdir compose_Odoo

# Colocarse en la carpeta recien creada
cd compose_Odoo

# Creación del archivo compose.yml
nano docker-compose.yml
```
---
</details>

<details>
 <summary>Contenido del archivo</summary>
<br>

```bash
services:
  odoo:                                    # Define el servicio Odoo
    image: odoo:17.0                       # Especifica la imagen de Docker para Odoo, versión 17.0.
    depends_on:                            # Asegura que el servicio de base de datos se inicie antes que el servicio Odoo
      - db
    restart: unless-stopped                # El contenedor se reiniciara automáticamente a menos que se detenga explícitamente
    ports:
      - "8055:8069"                        # Asigna el puerto predeterminado de Odoo al puerto 8055
    environment:
      HOST: db                             # El nombre de host para el servicio de base de datos
      USER: admin                          # El nombre de usuario para la base de datos
      PASSWORD: pswd                       # La contraseña para la base de datos

  db:                                      # Define el servicio de base de datos PostgreSQL
    image: postgres:latest                 # Especifica la imagen de Docker para PostgreSQL, ultima versión
    restart: unless-stopped                # El contenedor se reiniciara automáticamente a menos que se detenga explícitamente
    environment:                 
      POSTGRES_DB: odoo                    # El nombre de la base de datos predeterminada
      POSTGRES_PASSWORD: pswd              # La contraseña para el usuario de PostgreSQL
      POSTGRES_USER: admin                 # El nombre de usuario de PostgreSQL
      PGDATA: /var/lib/postgresql/data/pgdata      # Especifica la ruta dónde PostgreSQL almacena los datos dentro del contenedor
    volumes:                               # Monta un volumen para conservar los datos de la base de datos
      - odoo-db-data:/var/lib/postgresql/data/pgdata

  pgadmin:                                 # Define el servicio pgAdmin
    image: dpage/pgadmin4                  # Especifica la imagen de Docker para pgAdmin
    restart: unless-stopped                # El contenedor se reiniciara automáticamente a menos que se detenga explícitamente
    ports:                                 # Asigna el puerto predeterminado de interfaz web pgAdmin al puerto 8065
      - "8065:80"      
    environment:        
      PGADMIN_DEFAULT_EMAIL: admin@example.com     # El correo electrónico de inicio de sesión para pgAdmin
      PGADMIN_DEFAULT_PASSWORD: admin      # La contraseña para el usuario pgAdmin
    depends_on:                            # Asegura que el servicio de base de datos se inicie antes que el servicio pgAdmin
      - db
    volumes:                               # Monta un volumen para conservar los datos de pgAdmin
      - pgadmin-data:/var/lib/pgadmin

volumes:                                   # Define los volúmenes declarados en los servicios
  odoo-db-data:
  pgadmin-data:

```
---
</details>

<details>
 <summary>Puesta en funcionamiento</summary>
<br>

```bash
# Ejecución en segundo plano (deja libre la terminal)
docker compose up -d

# Ejecución en primer plano  (muestra el log de los contenedores)
docker compose up 
```
---
</details>

## Acceso e instalación a los servicios

<details>
 <summary>Odoo</summary>
<br>

Acedemos a nuestra instalación de Odoo mediante:

```bash
http://<ip>:<puerto_odoo>
```
> La página resultante deberia ser las siguiente ↓
>
> ![Odoo_Incio](/img/Odoo1.png)

Una vez introducidos los datos de la BD y otra información pertinente, habremos completado la instalación de Odoo.

> Servicio en funcionamiento ↓
>
> ![Odoo_Pagina](/img/Odoo2.png)

---
</details>

<details>
 <summary>pgAdmin</summary>
<br>

Acedemos a nuestra instalación de pgAdmin mediante:

```bash
http://<ip>:<puerto_pgadmin>
```
> La página resultante deberia ser las siguiente ↓
>
> ![PgAdmin_Incio](/img/PgAdmin1.png)

Una vez introducidos los datos que especificamos en el archivo compose.yml, tendremos acceso a la página principal de nuestra sesión. Aquí podemos conectar nuestro servidor con los datos de Odoo seleccionando la opción: **Agregar nuevo servidor**.

> Formulario de servidor ↓
>
> ![PgAdmin_Servidor](/img/PgAdmin2.png)

Tras conectar correctamente la base de datos con pgAdmin, podremos visualizar los datos de Odoo desde la página.

> Servidor connectado ↓
>
> ![PgAdmin_Odoo](/img/PgAdmin3.png)

---
</details>

# Solucionar puertos ocupados

**¿Que ocurre si en el ordenador local el puerto 5432 está ocupado? ¿Y si lo estuviese el 8069? ¿Como puedes solucionarlo?**

1) 5432 - Puerto predeterminado de PostgreSQL
2) 8069 - Puerto predeterminado de Odoo para interfaz web

Un conflicto de puertos imposibilitara la iniciación del contenedor Docker que contenga un servicio con un puerto conflictivo.

Para solucionar problemas de conflictos entre puertos, la solución, es cambiar el puerto que esta utilizando uno de los servicios causantes del problema por otro distinto.
