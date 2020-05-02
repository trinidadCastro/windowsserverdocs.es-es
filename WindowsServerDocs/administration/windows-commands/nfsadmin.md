---
title: nfsadmin
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8020b028a046ead36b5f95604cd81d679861746
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723775"
---
# <a name="nfsadmin"></a>nfsadmin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Puede usar **nfsadmin** para administrar servidor para NFS y cliente para NFS.  
  
## <a name="syntax"></a>Sintaxis  
**nfsadmin Server** `[` *NombreDeEquipo*`] [``[``]]` *UserName* \-u nombreDeUsuario\-p *contraseña* l\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` \- `{` *client* *Password* `|` *UserName* `]]` u nombreDeUsuario `[`p contraseña \-r Client All\-`}`  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` \- `|` *UserName* `]] {`u nombreDeUsuario `[`p *contraseña*START STOP\-`}`  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[``]]` *Option* *Password* *UserName* u nombreDeUsuario \-p password config (opción)\-`[...]`  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[``]]` *Name* *Password* *UserName* u nombreDeUsuario \-p contraseña creategroup nombre\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[``]]` *UserName* u nombreDeUsuario \-p *contraseña* listgroups\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[``]]` *Name* *Password* *UserName* u nombreDeUsuario \-p contraseña deletegroup nombre\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[` *Password* *UserName* `]]` *OldName NewName* u nombreDeUsuario \-p contraseña renamegroup OldName NewName\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[` *Password* *UserName* `]]` *Name Host* u nombreDeUsuario \-p contraseña de addmembers nombre host\-`[...]`  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[``]]` *UserName* u nombreDeUsuario \-p *contraseña* listmembers\-  
  
**nfsadmin Server** `[` *NombreDeEquipo*`] [` `[` *Password* *UserName* `]]` *Group Host* u nombreDeUsuario \-p contraseña deletemembers grupo host\-`[...]`  
  
**nfsadmin Client** `[` *NombreDeEquipo*`] [` \- `|` *UserName* `]] {`u nombreDeUsuario `[`p *contraseña*START STOP\-`}`  
  
**nfsadmin Client** `[` *NombreDeEquipo*`] [` `[``]]` *Option* *Password* *UserName* u nombreDeUsuario \-p password config OPTION\-`[...]`  
  
## <a name="description"></a>Descripción  
La **nfsadmin** utilidad de\-línea de comandos NFSADMIN administra servidor para NFS o cliente para NFS en el equipo local o remoto que ejecuta los servicios de Microsoft \(para\)Network File System NFS. Si inicia sesión con una cuenta que no tenga los privilegios necesarios, puede especificar un nombre de usuario y una contraseña de una cuenta que sí los tenga. La acción realizada por **nfsadmin** depende de los argumentos de comando que suministre.  
  
Además de las opciones\-y los argumentos de comandos específicos del servicio, **nfsadmin** acepta lo siguiente:  
  
*NombreDeEquipo*  
Especifica el equipo remoto que desea administrar. Puede especificar el equipo mediante un nombre de WINS \(\) del servicio de nombres de Internet de Windows o \(un\) nombre DNS del sistema de nombres \(de\) dominio, o bien mediante la dirección IP del Protocolo de Internet.  
  
*UserName* ** \-u** nombredeusuario  
Especifica el nombre del usuario cuyas credenciales se van a usar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em>**\\**<em>nombreDeUsuario</em> .  
  
*Password* ** \-p** contraseña  
Especifica la contraseña del usuario especificado mediante la ** \-opción u** . Si se especifica la ** \-opción u** pero se omite ** \-** la opción p, se le solicitará la contraseña del usuario.  
  
#### <a name="administering-server-for-nfs"></a>Administrar servidor para NFS  
Use el comando **nfsadmin Server** para administrar servidor para NFS. La acción específica que **nfsadmin Server** realiza depende de la opción de comando o el argumento que especifique:  
  
**\-CG**  
enumera todos los bloqueos retenidos por los clientes.  
  
r {*cliente* | **todos**} ** \-**  
Libera los bloqueos mantenidos por un *cliente* o, si se especifica **All** , por todos los clientes.  
  
**start**  
inicia el servicio servidor para NFS.  
  
**stop**  
Detiene el servicio servidor para NFS.  
  
**configurar**  
Especifica la configuración general de servidor para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :  
  
<em>servidor</em> de **mapsvr\=**  
Establece el *servidor* como servidor de asignación de nombres de usuario para servidor para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**auditlocation\=**{**eventlog** | **file**archivo | **both**EventLog ninguno**none**} |   
Especifica si se auditarán los eventos y dónde se registrarán los eventos. Se requiere uno de los siguientes argumentos.  
  
**EventLog**  
Especifica que los eventos auditados se registrarán solo en el registro de la aplicación Visor de eventos.  
  
**filesystem**  
Especifica que los eventos auditados se registrarán solo en el archivo especificado por **config fname**.  
  
**ambos**  
Especifica que los eventos auditados se registrarán en el registro de la aplicación Visor de eventos, así como en el archivo especificado por **config fname**.  
  
**Ninguna**  
Especifica que no se auditarán los eventos.  
  
**fname\=**(<em>archivo</em> )  
Establece el archivo especificado por *archivo* como archivo de auditoría. El valor predeterminado es% sfudir\\%\\log nfssvr. log.  
  
**tamaño\=fsize**\=*size*  
Establece el *tamaño* como el tamaño máximo en megabytes del archivo de auditoría. El tamaño máximo predeterminado es de 7 MB.  
  
**auditar\=** \] **write** **locking** **mount** \] **all** **read** **delete** **create** montaje lectura y escritura \[crear eliminar |bloquear todo \[ \[ **\+** **\-** \] **\+** **\+** | **\-** | \[ **\+** | **\-** \] **\+** \] \[ | **\-** \] **\+** \[ |**\-**\]**\+** **\-**\[| **\-**  
Especifica los eventos que se van a registrar. Para iniciar el registro de un evento, escriba un \( **\+** \) signo más antes del nombre del evento; para detener el registro de un evento, escriba un \( **\-** \) signo menos antes del nombre del evento. Si se omite el signo, se supone el signo más. No use **All** con ningún otro nombre de evento.  
  
**lockperiod\=**<em>segundos</em>  
Especifica el número de segundos que servidor para NFS esperará para reclamar bloqueos después de que se haya perdido la conexión con servidor para NFS y se haya reestablecido o después de que se haya reiniciado el servicio servidor para NFS.  
  
Portmapprotocol\={TCP | UDP | UDP\+TCP  
Especifica qué protocolos de transporte portmap admite. La configuración predeterminada es **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el montaje. La configuración predeterminada es **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite NFS \(\) Network File System. La configuración predeterminada es **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica los protocolos de transporte que \(\) es compatible con el administrador de bloqueos de red. La configuración predeterminada es **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el \(administrador\) de estado de red NSM. La configuración predeterminada es **TCP\+UDP**.  
  
**enableV3\=**{**sí** | **no**}  
Especifica si se admitirán los protocolos de la versión 3 de NFS. El valor predeterminado es **sí**.  
  
**renewauth\=**{**sí** | **no**}  
Especifica si será necesario volver a autenticar las conexiones de cliente después del período especificado por **config renewauthinterval**. La configuración predeterminada es **no**.  
  
**renewauthinterval\=**<em>segundos</em>  
Especifica el número de segundos que transcurren antes de que se fuerce a un cliente a volver a autenticarse si la **configuración renewauth** está establecida en **sí**. El valor predeterminado es 600 segundos.  
  
<em>tamaño</em> de **dircache\=**  
Especifica el tamaño en kilobytes de la memoria caché del directorio. El número especificado como *tamaño* debe ser un múltiplo de 4 entre 4 y 128. El tamaño predeterminado\-de la caché del directorio es de 128 KB.  
  
**archivo translationfile**\=\[\]  
Especifica un archivo que contiene información de asignación para reemplazar caracteres en los nombres de los archivos cuando se\-mueven desde\-Windows basados en sistemas de archivos basados en Unix. Si no se especifica *File* , la traducción de caracteres de nombre de archivo está deshabilitada. Si se cambia el valor de **translationfile** , deberá reiniciar el servidor para que el cambio surta efecto.  
  
**dotfileshidden**\={**sí** | **no**}  
Especifica si los archivos que se crean con nombres que comienzan por \(un punto. \) se marcarán como ocultos en el sistema de archivos de Windows y, por tanto, se ocultarán de los clientes NFS. La configuración predeterminada es **no**.  
  
**casesensitivelookups\=**{**sí** | **no**}  
Especifica si las búsquedas de directorio distinguirán \(mayúsculas de minúsculas que requieren coincidencia\)exacta de mayúsculas y minúsculas de caracteres.  
  
También debe deshabilitar la no distinción de\-mayúsculas y minúsculas del kernel de Windows para que\-servidor para NFS admita nombres de archivo que distinguen mayúsculas de minúsculas. Puede deshabilitar la no distinción\-de mayúsculas y minúsculas del kernel de Windows si desactiva la siguiente clave del registro en 0:  
  
HKLM\\System\\CurrentControlSet\\control\\Session Manager\\kernel  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> Esta sección solo se aplica a Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003. Esta sección no se aplica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=**{**inferior** | **upper**superior | **preserve**}  
Especifica si el uso de mayúsculas o minúsculas en los nombres de los archivos del sistema de archivos NTFS se devolverá en minúsculas, mayúsculas o en el formato almacenado en el directorio. La configuración predeterminada es **preserve**. Esta configuración no se puede cambiar si **casesensitivelookups** está establecido en **yes**.  
  
**creategroup** *nombre* de creategroup  
crea un nuevo grupo de clientes y le asigna el *nombre*especificado.  
  
**listgroups**  
Muestra los nombres de todos los grupos de clientes.  
  
**deletegroup** *nombre* de deletegroup  
quita el grupo de clientes especificado por *nombre*.  
  
**renamegroup** *OldName NewName*  
cambia el nombre del grupo de clientes especificado por *OldName* a *NewName* .  
  
**addmembers** *Name Host*host\[de nombre de addmembers...\]  
agrega el *host* al grupo de clientes especificado por *nombre*.  
  
**listmembers** *nombre* de listmembers  
enumera los equipos host del grupo de clientes especificado por *nombre*.  
  
**deletemembers** *Group Host*host\[de grupo de deletemembers...\]  
quita el cliente especificado por el *host* del grupo de clientes especificado por el *Grupo*.  
  
Si no se especifica una opción de comando o un argumento, **nfsadmin Server** muestra las opciones de configuración del servidor para NFS actual.  
  
#### <a name="administering-client-for-nfs"></a>Administración de Cliente para NFS  
Use el comando **nfsadmin Client** para administrar cliente para NFS. La acción específica que **nfsadmin Client** realiza depende del argumento de comando que especifique:  
  
**start**  
inicia el servicio cliente para NFS.  
  
**stop**  
Detiene el servicio cliente para NFS.  
  
**configurar**  
Especifica la configuración general de cliente para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :  
  
<em>modo</em> **FileAccess\=**  
-   Especifica el modo de permisos predeterminado para los archivos creados en servidores \(NFS\) de Network File System. El argumento *mode* consta de tres dígitos entre 0 y 7 \(, ambos\) incluidos, que representan los permisos predeterminados concedidos \(al\)usuario, grupo y otros, respectivamente. Los dígitos se traducen\-a los permisos de estilo Unix\=de la siguiente\=manera: 0\=ninguno, 1\=x, 2\=w, 3\=WX, 4\=r, 5 RX\=, 6 RW y 7 rwx. Por ejemplo, **fileaccess\=750** proporciona el permiso rwx al propietario, el permiso RX en el grupo y ningún permiso de acceso a otros usuarios.  
  
<em>servidor</em> de **mapsvr\=**  
Establece el *servidor* como servidor de asignación de nombres de usuario para cliente para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**mtype\=**{**Hard** | **Soft**}  
Especifica el tipo de montaje predeterminado. Para un montaje forzado, cliente para NFS sigue Reintentando una RPC con errores hasta que se realiza correctamente. Para un montaje flexible, cliente para NFS devuelve un error a la aplicación que realiza la llamada después de volver a intentar la llamada el número de veces especificado por la opción de **reintento** .  
  
<em>número</em> de **reintentos\=**  
Especifica el número de veces que se intenta establecer una conexión para un montaje flexible. Este valor debe estar comprendido entre 1 y 10, ambos inclusive. El valor predeterminado es 1.  
  
<em>segundos</em> de **tiempo de espera\=**  
Especifica el número de segundos de espera para una llamada \(\)a procedimiento remoto de conexión. Este valor debe ser 0,8, 0,9 o un entero comprendido entre 1 y 60, ambos incluidos. El valor predeterminado es 0,8.  
  
**Protocolo\={TCP | UDP | TCP\+UDP}**  
Especifica qué protocolos de transporte admite el cliente. La configuración predeterminada es **TCP\+UDP**  
  
<em>tamaño</em> **rsize\=**  
Especifica el tamaño, en kilobytes, del búfer de lectura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
<em>tamaño</em> de **wsize\=**  
Especifica el tamaño, en kilobytes, del búfer de escritura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**rendimiento\=predeterminado**  
Restaura los valores predeterminados de la siguiente configuración de rendimiento:  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>modo</em> **FileAccess\=**  
Especifica el modo de permisos predeterminado para los archivos creados en servidores \(NFS\) de Network File System. El argumento *mode* consta de tres dígitos entre 0 y 7 \(, ambos\) incluidos, que representan los permisos predeterminados concedidos \(al\)usuario, grupo y otros, respectivamente. Los dígitos se traducen\-a los permisos de estilo Unix\=de la siguiente\=manera: 0\=ninguno, 1\=x, 2\=w, 3\=WX, 4\=r, 5 RX\=, 6 RW y 7 rwx. Por ejemplo, **fileaccess\=750** proporciona el permiso rwx al propietario, el permiso RX en el grupo y ningún permiso de acceso a otros usuarios.  
  
Si no se especifica una opción de comando o un argumento, **nfsadmin Client** muestra la configuración actual de cliente para NFS.  
  

