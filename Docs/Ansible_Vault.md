<h1>
  Ansible Vault
</h1>
Ansible Vault es una característica de Ansible que permite cifrar y descifrar archivos sensibles, como direcciones IP, contraseñas o playbooks completos bajo contraseñas definidas. Esto es especialmente útil para proteger información confidencial en entornos donde la seguridad es una prioridad.
<h2> 
  Comandos básicos de Vault
</h2>

Algunos comandos útiles mencionados en la bitácora de Padilla y Sagot son:
- `ansible-vault create <dirección_archivo>`: Crea un nuevo archivo cifrado.
- `ansible-vault encrypt <dirección_archivo>`: Cifra un archivo existente.
- `ansible-vault decrypt <dirección_archivo>`: Descifra un archivo cifrado.
- `ansible-vault edit <dirección_archivo>`: Abre un archivo cifrado para su edición.
- `ansible-vault rekey <dirección_archivo>`: Cambia la contraseña de un archivo cifrado.

Si se desea manipular o leer un archivo cifrado, se debe agregar la opción `--ask-vault-pass` o `--ask-vault-password` al comando correspondiente.

<h2> 
  Configuración de passwords predeterminados
</h2>

En general, al crear un nuevo archivo cifrado, se pedirá una contraseña a utilizar y esta se solicitará cada vez que se intente manipular el archivo. Sin embargo, es una buena práctica definir un archivo de contraseñas para tenerlas rastreadas y evitar olvidarlas, mediante el  comando:

```
ansible-vault --vault-password-file <dirección_archivo>
```
y así especificar el archivo que contenga la o las contraseñas. En este punto es importante considerar si se utilizará una única contraseña para todos los archivos, o bien, variar de acuerdo con el uso de cada archivo. 
<h3> 
  Para múltiples contraseñas
</h3>

Si se desea utilizar múltiples contraseñas, estas se pueden diferenciar por medio de diferentes "vault IDs". Según lo indicado en la [documentación oficial](https://docs.ansible.com/projects/ansible/latest/vault_guide/vault_managing_passwords.html#choosing-between-a-single-password-and-multiple-passwords), las flags se usan de la siguiente manera:

```
--vault-id <id_1>@<dirección_archivo_pass> <dirección_archivo_a_cifrar_o_cifrado>
```
El conjunto anterior se puede concatenar a los comandos:
* `ansible-vault create`: para crear un archivo cifrado con la contraseña asociada al vault ID en el archivo de contraseñas.
* `ansible-playbook`: para ejecutar un playbook utilizando la contraseña asociada al vault ID  en el archivo de contraseñas.