<h1 align = center>
 Manual de confección de playbooks en Ansible
</h1>
<p align = center >
Amanda Rodríguez Jiménez
<br> Actualizado el 28 de octubre, 2025
</p>


## Tabla de contenidos 

1. [Secciones usuales en un playbook](#Secciones)
2. [Ejemplos de playbooks](#Ejemplos)
3. [Ejecución de un playbook](#Ejecucion)


<h2 id= "Secciones"> 
  Secciones usuales en un playbook
</h2>

<h3> Nombre del playbook </h3>

`name`: nombre indicado para la tarea que se va a realizar.

<h3> Target Hosts </h3>

`hosts`: nombre de los hosts a los que se les aplican las tareas. Suele ser un archivo `.yml` que contiene el conjunto de hosts. En el comando de terminal se debe añadir el inventario donde estos grupos estén localizados.

<h3> Variables </h3>

`vars`: sección para especificar valores de variables dadas, como el puerto http, una cantidad máxima de usuarios, etcétera.

<h3> Orden de los Hosts</h3>

`order`: permite definir el orden en el que serán ejecutados los hosts para aplicarles las tareas. Los valores pueden ser:
* inventory (default): ejecutados a como se encuentran en el inventario.
* reverse_inventory: invierte el orden del inventario, empezando desde el final.
* sorted: ordenados alfabéticamente por nombre.
* reverse_sorted: ordenados alfabéticamente por nombre, iniciando desde la Z.
* shuffle: orden aleatorio.

<h3> Cuenta del Remote User</h3> 

`remote-user`: nombre de la cuenta de usuario

<h3> Concesión de privilegios </h3>

`become`: en caso de que se requiere utilizar el super usuario (sudo), permite elevar privilegios de acceso.

<h3> Segmento de tasks </h3>


`tasks`: directriz que indica el inicio de la lista de la(s) tarea(s) a realizar. Cada task requiere como mínimo de un nombre (`name`) y un [módulo de ansible](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html) para ejecutarse.

<h4> Módulos de Ansible </h4> 

Entre los más comunes se pueden mencionar:
- *apt*: para paquetes en Debian/Ubuntu
- *service*: servicios del sistema
- *file*: archivos y directorios.
- *copy*: copiar archivos locales al host remoto
- *template*: plantillas Jinja2

Algunos parámetros recurrentes de los módulos son:
- `name`: nombre del módulo seleccionado
- `state`: estado o versión del recurso (present, absent, started, stopped, latest).
- `enabled`: si el recurso está habilitado (yes/no)
- `src`: archivo de origen
- `dest`: ruta destino en el host remoto.
- `mode`: se refiere a los permisos del archivo (Unix)
- `when`: para realizarse al cumplir dada condición
- `notify`: informa cuando la tarea realiza un cambio en el sistema (mensaje de notificación). Si hay una lista agregada, todos sus elementos serán denominados `handlers`. 
    - `handler`: notificados por el parámetro de _notify_, siendo invocados por el nombre en el que escuchan. Podría decirse que son como sub-tasks que se llaman de uno principal.

<h2 id = "Ejemplos" > Ejemplos de playbooks </h2>

_Importante_: La identación y los guiones son indispensables para el formato de los playbooks:

```yml
--- 
- name: Update web servers
  hosts: webservers
  become: true  # asignar privilegios de sudo 
 
  tasks:
    - name: Ensure apache is at the latest version
      ansible.builtin.yum: # módulo de ansible
        name: httpd # nombre del paquete de yum
        state: latest # última versión
    - name: Write the apache config file
      ansible.builtin.template:
        src: /srv/httpd.j2
        dest: /etc/httpd.conf
        mode: "0644"
```

```yml
---
- name: Restart machines
  hosts: linux_hosts
  
  handlers:
    - name: restart memcached
      service:
        name: memcached
        state: restarted
      listen: "restart web services" # donde el handler escuchará hasta ser invocado
    - name: restart apache
      service:
        name: apache
        state: restarted
      listen: "restart web services"

  tasks:
    - name: restart everything
      command: echo "this task will restart the web services" # módulo de comando / shell
      notify: "restart web services" # invocación del handler
```

<h2 id = "Ejecucion"> Ejecución de un playbook</h2>

Estando en uno de los nodos master, se puede ejecutar el comando `sudo ansible-playbook -i inventory_name route/of/playbook.yml` para ejecutar dado playbook en dado inventario. Se puede omitir el parámetro del inventario (indicado por la flag -i) si se tiene por defecto una ruta que señale qué nodos esclavos se quieren levantar. 

<h3> Programación de playbooks</h3> 

_Queda pendiente revisarlos en el master corriendo_

En el nodo master se encuentra la carpeta _cron jobs_, en donde se pueden definir archivos que programen la ejecución de dado playbook en los nodos esclavos. Se utiliza la opción `ansible-connection:local`. La idea es que el playbook se ejecute localmente en el nodo central de Ansible y que en el momento dado se ejecute en los esclavos. 
En el cron job, al inicio del archivo se encuentra la sección que indica la hora y día en que se ejecutarán los playbooks, siguiendo el formato `# # # # #`, es decir, una tupla de 5 campos, respectivamente, minuto (0-59), hora(0-23), día del mes (1-31), mes (1-12, jan-dec) y el día de la semana (0-7). Entonces, si por ejemplo se desea programar una tarea para cada domingo a las 10 am, se introduce:

```
0 10 * * 0 ansible-playbook /ruta/al/playbook_programado.yml --connection=local
```
donde los asteriscos (*) indican que se ejecutarán en todos los domingos de todos los meses.

Otros ejemplos recurrentes de cronogramas serían:
- 0 0 * * *: Todos los días a medianoche
- 30 6 * * 1-5: Lunes a viernes a las 6:30 am
- 0 */2 * * *: Cada dos horas de todos los días.
