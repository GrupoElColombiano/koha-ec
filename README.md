# Instalación de Koha LMS en una maquina con sistema operativo Ubuntu

Sigue los siguientes pasos para instalar Koha LMS:

1. Inicia sesión como usuario root:
    ```
    sudo su
     ```

2. Actualiza y mejora el sistema:
    ```
    apt-get update
    apt-get upgrade
    ```

3. Agrega el repositorio de la comunidad de Koha:
    ```
    echo deb http://debian.koha-community.org/koha stable main | sudo tee /etc/apt/sources.list.d/koha.list
    wget -O- http://debian.koha-community.org/koha/gpg.asc | apt-key add -
    apt-get update
     ```
  
4. Instala la última versión de Koha:
   ```
   apt-get install koha-common
   ```

5. Configuración del servidor:
  Abre el siguiente archivo y realiza cambios en los números de puerto:
```
  nano /etc/koha/koha-sites.conf
```
modificar las opciones 'INTRAPORT' y 'OPACPORT' de la siguiente manera:\
  INTRAPORT="8001"\
  OPACPORT="8000"

6. Instala el servidor de MariaDB:
   ```
   apt-get install mariadb-server
   ```
despues de la instalción:
   ```
   service mariadb start
   ```
7. Configurar el servidor de datos:
   ```
   mysql_secure_installation
   ```
   Responda “N” para la primera pregunta y “S” para el resto

8. Creación de una instancia de Koha:\
   Aplica los siguientes comandos para configurar los archivos de configuración de Apache:
   ```
   sudo a2enmod rewrite
   sudo a2enmod cgi
   sudo service apache2 restart
   ```
9. Create a Koha instance named library
   ```
   koha-create --create-db library
   ```

10. Abre el siguiente archivo y asigna el número de puerto para el cliente de Koha y el OPAC:
    ```
    nano /etc/apache2/ports.conf
    ```
    Pega la siguiente línea debajo de "Listen 80":\
    Listen 8000
    Listen 8001
    Reinicia el servidor Apache:\
    ```
    service apache2 restart
    ```
    
11. Habilita módulos y sitios:
 ```
 sudo a2dissite 000-default
 sudo a2enmod deflate
 sudo a2ensite library
 sudo service apache2 restart
 ```

12. Habilita y ejecuta Plack para mejorar el rendimiento (habilita solo si tienes suficiente RAM en tu máquina):
 ```
 sudo koha-plack --enable library
 sudo koha-plack --start library
 ```

13. Obtiene la contraseña predeterminada de Koha:
 ```
 sudo koha-passwd library
 ```

 La contraseña se mostrará en la terminal. Cópiala y pégala en la interfaz de inicio de sesión del cliente de Koha.

14. Contraseña predeterminada de Koha:
 Abre la siguiente URL en el navegador y completa los pasos del instalador web:
 ```
 http://localhost:8001 o http://127.0.0.1:8001
 ```

Recuerda reemplazar "library" con el nombre que hayas asignado a tu instancia de Koha.
