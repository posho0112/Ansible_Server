
<h1 align = center>
 Ansible Server: Manual de componentes para el uso de Ansible 2025
</h1>

Este repositorio reúne una serie de recursos, ejemplos y configuraciones para utilizar Ansible. 
La idea es poner a disposición algunas guías de confección de pruebas para un servidor de Ansible. Algunas explicaciones fueron tomadas de la bitácora realizada por Axel Padilla y Jorge Sagot, o bien, directamente de la documentación de Ansible.

## Contenidos
- [Introducción a Ansible](#Introduccion)
- [Comandos Básicos de Ansible](#Comandos)
- [Ansible Vault](#AnsibleVault)
- [Ansible Playbooks](#Playbooks)

<h2 id= "Introduccion"> 
  Introducción a Ansible
</h2>

Ansible es una herramienta de TI que automatiza aprovisionamiento en la nube, administración de configuraciones, implementación de aplicaciones y orquestación de tareas complejas desde un nodo master hacia nodos gestionados (servidores remotos). Ansible utliza un lenguaje simple basado en YAML para describir las tareas de automatización (para más detalles, consultar la [documentación oficial](https://docs.ansible.com/projects/ansible/latest/dev_guide/overview_architecture.html)).

<h2 id= "Comandos"> 
  Comandos Básicos de Ansible
</h2>

Algunos comandos básicos para utilizar Ansible desde la terminal son:
- `ansible --version`: Muestra la versión de Ansible instalada.
- `ansible all -m ping`: Verifica la conectividad con todos los hosts definidos en el inventario.
- `ansible -i <inventario> all -m ping`: Verifica la conectividad con todos los hosts en un inventario específico.
- `ansible-playbook -i inventory_name route/of/playbook.yml`: Ejecuta un playbook de Ansible en los hosts definidos en el inventario especificado.

Hay que destacar que, aunque no siempre es pertinente, estos comandos suelen ir precedidos por `sudo` para asegurar los permisos necesarios.

<h2 id= "AnsibleVault"> 
  Ansible Vault
</h2>

Ansible Vault es una propiedad de Ansible que permite cifrar y descrifrar archivos sensibles, como direcciones IP, contraseñas o playbooks completos bajo contraseñas definidas. 

Para más detalles, consulte el siguiente enlace: [Ansible Vault](Docs/Ansible_Vault.md)


<h2 id= "Playbooks"> 
  Ansible Playbooks
</h2>

Los Ansible playbooks son archivos en formato YAML que contienen una serie de instrucciones para automatizar tareas en múltiples servidores. 
Son la parte fundamental para la implementación de Ansible, ya que permiten orquestaciones y gestión de tareas eficientemente, de manera remota.

Para más detalles, consulte el siguiente enlace:
[Ansible Playbooks](Docs/Playbooks_Ansible.md)
