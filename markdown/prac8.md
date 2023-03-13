# Balanceador de carga con Apache
<p align="center">
<img src="https://raw.githubusercontent.com/drain113/pictures/main/Fotos/2.png" width="350" height="220" />  
</p>
<br>   <br/>  


# Índice

### [1. Introducción](#introducción)

### [2. Estructura](#estructura)

### [3. Creación y configuración de la instancia](#creación-y-configuración-de-la-instancia)
  
### [4. Instalación del repositorio](#Instalación-del-repositorio)

### [5. Conclusión](#conclusión)

<br>   <br/>   

# Introducción
Esta práctica consiste en la instalación automatizada mediante Ansible de una aplicación web LAMP como en la práctica 7 pero con cuatro máquinas EC2 realizando un balanceo de carga entre Frontends para redirigir el tráfico.

Un balanceador de carga es un dispositivo hardware o software que se pone al frente de un conjunto de servidores y se encarga de asignar o balancear las peticiones que llegan de los clientes hacia los servidores.

Ejemplos de balanceadores de carga hardware:

    F5 BIG-IP.
    Kemp LoadMaster.

Ejemplos de balanceadoes de carga software:

    HAProxy.
    Nginx.
    Apache HTTP Server.

Estos dispositivos permiten distribuir el tráfico de red entre varios servidores o dispositivos de red, con el fin de mejorar el rendimiento y la disponibilidad de un sistema o aplicación.

Un proxy inverso es un tipo de servidor proxy que hace de intermediario entre un cliente y uno o más servidores. El cliente realiza las peticiones a los servidores a través del proxy inverso y las respuestas de los servidores hacia el cliente también se envían a través del proxy inverso.

El objetivo de esta práctica es crear una arquitectura de alta disponibilidad que sea escalable y redundante, de modo que podamos balancear la carga entre todos los frontales web.
<br>   <br/>   

# Estructura
La estructura de esta práctica se divide en cuatro máquinas (EC2).  
- Una máquina que hará de **Balanceador**.  
- Una máquina que hará de **Backend**.
- Dos máquinas que harán de **Frontend**.  

Por otro lado, necesitamos un equipo , ya sea local o máquina EC2 que será el Launcher desde el que instalaremos ansible para ejecutar los playbooks en las máquinas

Como realizaremos la instalación en Ansible, solo tendremos que tener en cuenta el archivo inventario del repositorio en el que declararemos las IP de las máquinas que actuarán como Frontend y backend.
También debemos de editar las variables para seleccionar las IP del backend y otras variables de configuración de la base de datos.  
# Creación y configuración de la instancia

Crearemos dos instancias, ambas con Ubuntu 22.04 y 4 GB de RAM.

Tendremos que fijarnos en que ambas tengan sus puertos correctamente abiertos en la configuración inicial.  

**IMPORTANTE**  
- En la instancia de Backend debemos de abrir el puerto 3306 para aceptar conexiones MySQL al servidor que mantiene la base de datos.  
- Es importante que cambiemos las variables del script (archivo variables.yml) para determinar el dominio e Ips del backend entre otras funciones.
- Por último, se debe cambiar las Ip del inventario.

Descargaremos la vockey de nuestras máquinas (vockey.pem) en nuestro Launcher o equipo local para declararla en nuestro inventario de conexiones de Ansible.

En caso de usar máquina como Launcher de ansible, podremos pasar la vockey con la siguiente sintáxis:
``` shell
scp username@b:/path/to/file /path/to/destination
```


# Instalación del repositorio

Una vez tengamos nuestra máquina correctamente enlazada mediante Ansible (podemos hacer la prueba mediante ) 
``` ansible
ansible all -m ping
```

Solo hará falta ejecutar el playbook main.yml que instalará todo en el siguiente orden:
1. Instalaremos primero **deploy_backend.yml** que instalará las herramientas necesarias (MySQL junto a Python y sus módulos) y creará el usuario de la base de datos.
2. Luego instalará **deploy_loadbalancer.yml** que hará el despliegue del balanceador de carga y el certificado de Certbot.
3. Instalaremos **deploy_frontend.yml** qué despliega la aplicación web del repositorio de Jose Juan instalando todo lo necesario para ello y además, exportando la base de datos al backend.  

<br>   <br/> 

# Conclusión

Ansible es realmente útil para manejar múltiples máquinas y configurar e instalar herramientas conjuntas.
Esta práctica es una manera de acercarnos a la arquitectura en partes de las páginas webs más avanzadas para  asegurarnos de no tener fallos de rendimiento o posibles caídas en frontends. 
<break>   </break>  
-Guille  
<break>   </break>  
 [![](https://preview.redd.it/enr7hhg3zku81.png?auto=webp&s=fc017e6a82f91cc81ab3dd7d0388ef57bfd72c30)](https://github.com/drain113)
