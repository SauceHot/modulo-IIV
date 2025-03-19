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
