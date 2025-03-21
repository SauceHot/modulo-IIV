# modulo-IIV
comandos modulo IIV

Paso 1: Instalar NFS en el servidor
Instalar el servidor NFS:
En el servidor, instala el paquete nfs-utils:

bash
Copy
sudo dnf install nfs-utils -y
Habilitar e iniciar el servicio NFS:
Habilita e inicia el servicio NFS:

bash
Copy
sudo systemctl enable nfs-server --now
Verificar el estado del servicio:
Asegúrate de que el servicio esté activo:

bash
Copy
sudo systemctl status nfs-server
Paso 2: Crear el directorio compartido y los archivos
Crear el directorio OS3:
Crea un directorio llamado OS3 en el servidor:

bash
Copy
sudo mkdir /OS3
Crear 100 archivos dentro del directorio:
Usa un bucle para crear 100 archivos llamados Adrian [1..100].txt:

bash
Copy
for i in {1..100}; do sudo touch /OS3/"Adrian $i.txt"; done
Asignar permisos al directorio:
Asegúrate de que el directorio tenga permisos adecuados para que otros usuarios puedan acceder:

bash
Copy
sudo chmod -R 777 /OS3
sudo chown nobody:nobody /OS3
Paso 3: Configurar los exports en el servidor
Editar el archivo /etc/exports:
Define qué directorios se compartirán y con qué permisos:

bash
Copy
sudo nano /etc/exports
Agregar la siguiente línea:
Agrega la ruta del directorio OS3 y define los permisos de acceso:

bash
Copy
/OS3 *(rw,sync,no_subtree_check)


/OS3: Ruta del directorio compartido.
*: Permite el acceso a todos los clientes. Puedes restringirlo a una IP o red específica (por ejemplo, 192.168.1.0/24).

rw: Permite lectura y escritura.

sync: Sincroniza las operaciones de escritura.

no_subtree_check: Mejora el rendimiento al no verificar subárboles.

Recargar la configuración de NFS:
Aplica los cambios recargando la configuración de NFS:

bash
Copy
sudo exportfs -a
Verificar los exports:
Verifica que el directorio se haya exportado correctamente:

bash
Copy
sudo exportfs -v
Paso 4: Configurar el cliente NFS
Instalar el cliente NFS:
En la máquina cliente, instala el paquete nfs-utils:

bash
Copy
sudo dnf install nfs-utils -y
Crear un punto de montaje:
Crea un directorio donde se montará el directorio compartido:

bash
Copy
sudo mkdir /mnt/os3_shared
Montar el directorio compartido:
Monta el directorio compartido desde el servidor:

bash
Copy
sudo mount ip_servidor:/OS3 /mnt/os3_shared
ip_servidor: Dirección IP del servidor NFS.

/OS3: Ruta del directorio compartido en el servidor.

/mnt/os3_shared: Punto de montaje en el cliente.

Verificar el montaje:
Verifica que el directorio se haya montado correctamente:

bash
Copy
df -h
También puedes listar los archivos en el directorio montado:

bash
Copy
ls /mnt/os3_shared
Paso 5: Configurar el montaje automático en /etc/fstab
Editar el archivo /etc/fstab:
Abre el archivo /etc/fstab en el cliente:

bash
Copy
sudo nano /etc/fstab
Agregar la siguiente línea:
Agrega una entrada para montar automáticamente el directorio compartido al iniciar el sistema:

bash
Copy
ip_servidor:/OS3 /mnt/os3_shared nfs defaults 0 0
ip_servidor:/OS3: Ruta del directorio compartido en el servidor.

/mnt/os3_shared: Punto de montaje en el cliente.

nfs: Tipo de sistema de archivos.

defaults: Opciones de montaje predeterminadas.

0 0: Opciones de dump y fsck.

Probar el montaje automático:
Reinicia el cliente y verifica que el directorio se monte automáticamente:

bash
Copy
sudo reboot
Después de reiniciar, verifica que el directorio esté montado:

bash
Copy
df -h
También puedes listar los archivos en el directorio montado:

bash
Copy
ls /mnt/os3_shared



  sudo dnf install samba -y
  302  systemctl enable smb
  303  systemctl enable smb
  304  sudo systemctl start smb
  305  sudo systemctl enable nmb
  306  sudo systemctl start nmb
  307  sudo systemctl status nmb
  308  sudo systemctl status smb
  309  sudo mkdir -p /samba
  310  cd /samba/
  311  ls
  312  sudo tocuh adrian{1..100}.txt
  313  ls
  314  sudo touch adrian{1..100}.txt
  315  ls
  316  sudo chown hugo:hugo
  317  sudo chown hugo
  318  sudo chown hugo:hugo
  319  sudo chown -R hugo:hugo *
  320  sudo chown hugo *
  321  sudo chown 666 *
  322  ls -l
  323  sudo chown 666 *
  324  ls -l
  325  sudo chmod 666 *
  326  ls -l
  327  sudo chown nobody:nobody /samba
  328  sudo chmod -R 0775 /samba
  329  sudo groupadd sambagroup
  330  sudo useradd -M -d /samba -s /sbin/nologin hugosmb
  331  sudo smb passwd -a hugosmb
  332  sudo smbpasswd -a hugosmb
  333  sudo usermod -aG sambagroup hugosmb
  334  ls
  335  sudo usermod -aG sambagroup hugosmb /samba
  336  sudo usermod -aG sambagroup hugosmb/samba
  337  sudo usermod -aG sambagroup hugosmb /samba
  338  sudo usermod -aG sambagroup hugosmb 
  339  id hugosmb
  340  sudo chown -R :sambagroup /samba
  341  sudo nano /etc/samba/smb.conf
  342  sudo systemctl restart smb
  343  sudo systemctl restart nmb
  344  sudo firewall-cmd --add-port=137/tcp --permanent
  345  sudo firewall-cmd --add-port=139/tcp --permanent
  346  sudo firewall-cmd --add-port=445/tcp --permanent
  347  sudo firewall-cmd reload
  348  sudo firewall-cmd relload
  349  sudo firewall-cmd --reload
  350  firewall
  351  firewall-cmd --list /all
  352  firewall-cmd --list-all
  353  sudo setenforce 0
  354  cd /samba
  355  ls -l 
  356  cat adrian99.txt 

------------------------------------Paso 1: Crear un usuario en Linux y agregarlo a SAMBA
Crear el usuario lanegracubana:
Crea un usuario en Linux llamado lanegracubana y establece su contraseña (usando tu matrícula):

bash
Copy
sudo useradd -m lanegracubana
sudo passwd lanegracubana
Agregar el usuario a SAMBA:
Agrega el usuario lanegracubana a SAMBA y establece una contraseña:

bash
Copy
sudo smbpasswd -a lanegracubana
Se te pedirá que ingreses una contraseña. Usa tu matrícula como contraseña.

Verificar que el usuario esté en SAMBA:
Lista los usuarios de SAMBA para asegurarte de que lanegracubana esté registrado:

bash
Copy
sudo pdbedit -L
Paso 2: Instalar y configurar SAMBA 4 como Controlador de Dominio
Instalar SAMBA 4:
Instala SAMBA 4 y las dependencias necesarias:

bash
Copy
sudo dnf install samba samba-dc samba-client -y
Configurar SAMBA como Controlador de Dominio:

Detén el servicio SAMBA si está en ejecución:

bash
Copy
sudo systemctl stop smb
sudo systemctl stop nmb
Elimina la configuración existente de SAMBA (si la hay):

bash
Copy
sudo rm -rf /etc/samba/smb.conf
Configura SAMBA como Controlador de Dominio:

bash
Copy
sudo samba-tool domain provision --use-rfc2307 --interactive
Durante la configuración, proporciona la siguiente información:

Realm: SO3.inet (el nombre del dominio).

Domain: SO3 (el nombre corto del dominio).

Server Role: dc (Controlador de Dominio).

DNS Backend: SAMBA_INTERNAL (usar el DNS interno de SAMBA).

Administrator Password: Establece una contraseña segura para el administrador del dominio.

Iniciar el servicio SAMBA:
Una vez configurado, inicia el servicio SAMBA:

bash
Copy
sudo systemctl start samba-ad-dc
sudo systemctl enable samba-ad-dc
Verificar el estado del Controlador de Dominio:
Verifica que el Controlador de Dominio esté funcionando correctamente:

bash
Copy
sudo samba-tool domain level show
Paso 3: Unir la máquina virtual Windows al dominio
Configurar la red en Windows:

Asegúrate de que la máquina virtual Windows esté en la misma red que el servidor SAMBA.

Configura la dirección IP del servidor SAMBA como el DNS primario en la configuración de red de Windows.

Unir la máquina Windows al dominio:

Abre el Panel de Control en Windows.

Ve a Sistema y Seguridad > Sistema.

Haz clic en Cambiar configuración al lado de "Nombre del equipo, dominio y configuración del grupo de trabajo".

En la pestaña Nombre de equipo, haz clic en Cambiar.

Selecciona Dominio e ingresa el nombre del dominio (SO3.inet).

Haz clic en Aceptar.

Autenticar con el usuario lanegracubana:

Cuando se te solicite, ingresa las credenciales del usuario lanegracubana que creaste en SAMBA.

Usa el formato SO3\lanegracubana para el nombre de usuario y tu matrícula como contraseña.

Reiniciar la máquina Windows:

Reinicia la máquina Windows para aplicar los cambios.

Después del reinicio, inicia sesión en el dominio con el usuario lanegracubana.

Paso 4: Verificar la unión al dominio
Iniciar sesión en el dominio:

En la pantalla de inicio de sesión de Windows, selecciona el dominio SO3.inet e ingresa las credenciales de lanegracubana.

Verificar la pertenencia al dominio:

Abre el Panel de Control y ve a Sistema y Seguridad > Sistema.

Verifica que el equipo esté unido al dominio SO3.inet.

Probar la autenticación:

Intenta acceder a recursos compartidos en el servidor SAMBA desde Windows usando el usuario lanegracubana.
