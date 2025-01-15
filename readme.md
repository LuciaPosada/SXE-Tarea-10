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
[placeholder]
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

Una vez introducidos los datos de la BD y otra información pertinente habremos terminado nuestra instalación de Odoo

> Servicio en funcionamiento ↓
>
> ![Odoo_Pagina](/img/Odoo2.png)

---
</details>

<details>
 <summary>PgAdmin</summary>
<br>

Acedemos a nuestra instalación de PgAdmin mediante:

```bash
http://<ip>:<puerto_pgadmin>
```
> La página resultante deberia ser las siguiente ↓
>
> ![PgAdmin_Incio](/img/PgAdmin1.png)

Una vez introducidos los datos que especificamos en el archivo compose.yml tendremos acesso a la pagina principal de nuestra sesion, aqui podemos conectar nuestro servidor con los datos de odoo pulsando el la opcion: agregar nuevo servidor.

> Formulario de servidor ↓
>
> ![PgAdmin_Servidor](/img/PgAdmin2.png)

Tras connectar correctamente la BD con el PgAdmin podremos visualizar los datos de Odoo desde la pagina.

> Servidor connectado ↓
>
> ![PgAdmin_Odoo](/img/PgAdmin3.png)

---
</details>

