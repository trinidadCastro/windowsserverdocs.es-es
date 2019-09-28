---
title: nfsadmin
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373221"
---
# <a name="nfsadmin"></a>nfsadmin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar **nfsadmin** para administrar servidor para NFS y cliente para NFS.  
  
## <a name="syntax"></a>Sintaxis  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario*`[` @ no__t-7P *password*`]]` 0L  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *Password*`]]` 0R 1*Client* 3 All @ no__t-14  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *password*`]] {`start 0 stop @ no__t-11  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *Password*`]]` *opción*de configuración 1  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *contraseña*`]]` creategroup *nombre*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *password*`]]` listgroups  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *contraseña*`]]` deletegroup *nombre*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *password*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *contraseña*@no__t *-@no__t 9*.  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *password*`]]` listmembers  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *contraseña*`]]` deletemembers *host de grupo*1  
  
**nfsadmin client** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *password*`]] {`start 0 stop @ no__t-11  
  
**nfsadmin client** `[`*computerName*`] [` @ no__t-4U *nombreDeUsuario* `[` @ no__t-7P *Password*`]]` *opción*de configuración 1  
  
## <a name="description"></a>Descripción  
La utilidad **nfsadmin** Command @ no__t-1Line administra servidor para NFS o cliente para NFS en el equipo local o remoto que ejecuta servicios de Microsoft para Network File System \(NFS @ no__t-3. Si ha iniciado sesión con una cuenta que no tiene los privilegios necesarios, puede especificar un nombre de usuario y una contraseña de una cuenta que sí los tenga. La acción realizada por **nfsadmin** depende de los argumentos de comando que suministre.  
  
Además de los argumentos de comandos y las opciones de Service @ no__t-0specific, **nfsadmin** acepta lo siguiente:  
  
*NombreDeEquipo*  
Especifica el equipo remoto que desea administrar. Puede especificar el equipo mediante un servicio de nombres Internet de Windows @no__t 0WINS @ no__t-1 o un nombre de sistema de nombres de dominio \(DNS @ no__t-3, o bien por el protocolo de Internet \(IP @ no__t-5.  
  
**@no__t: nombre de** *usuario* 1U  
Especifica el nombre de usuario del usuario cuyas credenciales se van a utilizar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em> **\\** <em>nombreusuario</em>  
  
*contraseña* **de \-P**  
Especifica la contraseña del usuario especificado mediante la opción **\-U** . Si especifica la opción **\-U** pero omite la opción **\-P** , se le pedirá la contraseña del usuario.  
  
#### <a name="administering-server-for-nfs"></a>Administrar servidor para NFS  
Use el comando **nfsadmin Server** para administrar servidor para NFS. La acción específica que **nfsadmin Server** realiza depende de la opción de comando o el argumento que especifique:  
  
**@no__t 1L**  
enumera todos los bloqueos retenidos por los clientes.  
  
**\-r** {*cliente* | **All**}  
Libera los bloqueos mantenidos por un *cliente* o, si se especifica **All** , por todos los clientes.  
  
**start**  
inicia el servicio servidor para NFS.  
  
**detener**  
Detiene el servicio servidor para NFS.  
  
**config**  
Especifica la configuración general de servidor para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :  
  
**mapsvr @ no__t-1**<em>servidor</em>  
Establece el *servidor* como servidor de asignación de nombres de usuario para servidor para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**auditlocation @ no__t-1**{**EventLog** | **File**@no__t **-5 @no__t**-7**ninguno**}  
Especifica si se auditarán los eventos y dónde se registrarán los eventos. Se requiere uno de los siguientes argumentos.  
  
**EventLog**  
Especifica que los eventos auditados se registrarán solo en el registro de la aplicación Visor de eventos.  
  
**filesystem**  
Especifica que los eventos auditados se registrarán solo en el archivo especificado por **config fname**.  
  
**ambos**  
Especifica que los eventos auditados se registrarán en el registro de la aplicación Visor de eventos, así como en el archivo especificado por **config fname**.  
  
**ninguna**  
Especifica que no se auditarán los eventos.  
  
**fname @ no__t-1**<em>archivo</em>  
Establece el archivo especificado por *archivo* como archivo de auditoría. El valor predeterminado es% sfudir% @no__t -0log\\nfssvr.log  
  
**fsize @ no__t-1**@no__t *-2*  
Establece el *tamaño* como el tamaño máximo en megabytes del archivo de auditoría. El tamaño máximo predeterminado es de 7 MB.  
  
**Audit @ no__t-1**\[ **\+** | **\-** \]**mount** \=0**2**3**5**6**Read** 8**0**1 **@no__ t-23**4**Write** 6**8**9**1**2**crear** 4**6**7**9**0**Delete** 2 **@no__ t-44**5**7**8**locking** 0**2**3**5**6**All**  
Especifica los eventos que se van a registrar. Para iniciar el registro de un evento, escriba un signo más \( **\+** \) antes del nombre del evento; para detener el registro de un evento, escriba un signo menos \( **\-** \) antes del nombre del evento. Si se omite el signo, se supone el signo más. No use **All** con ningún otro nombre de evento.  
  
**lockperiod @ no__t-1**<em>segundos</em>  
Especifica el número de segundos que servidor para NFS esperará para reclamar bloqueos después de que se haya perdido la conexión con servidor para NFS y se haya reestablecido o después de que se haya reiniciado el servicio servidor para NFS.  
  
Portmapprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP  
Especifica qué protocolos de transporte portmap admite. La configuración predeterminada es **TCP @ no__t-1UDP**.  
  
mountprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica qué protocolos de transporte admite el montaje. La configuración predeterminada es **TCP @ no__t-1UDP**.  
  
nfsprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica qué protocolos de transporte Network File System \(NFS @ no__t-1 admite. La configuración predeterminada es **TCP @ no__t-1UDP**  
  
nlmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica los protocolos de transporte que el administrador de bloqueos de red \(NLM @ no__t-1 admite. La configuración predeterminada es **TCP @ no__t-1UDP**.  
  
nsmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Especifica los protocolos de transporte que el administrador de estado de red \(NSM @ no__t-1 admite. La configuración predeterminada es **TCP @ no__t-1UDP**.  
  
**enableV3 @ no__t-1**{**sí** | **no**}  
Especifica si se admitirán los protocolos de la versión 3 de NFS. El valor predeterminado es **sí**.  
  
**renewauth @ no__t-1**{**sí** | **no**}  
Especifica si será necesario volver a autenticar las conexiones de cliente después del período especificado por **config renewauthinterval**. La configuración predeterminada es **no**.  
  
**renewauthinterval @ no__t-1**<em>segundos</em>  
Especifica el número de segundos que transcurren antes de que se fuerce a un cliente a volver a autenticarse si la **configuración renewauth** está establecida en **sí**. El valor predeterminado es 600 segundos.  
  
**dircache @ no__t-1**<em>tamaño</em>  
Especifica el tamaño en kilobytes de la memoria caché del directorio. El número especificado como *tamaño* debe ser un múltiplo de 4 entre 4 y 128. El tamaño predeterminado del directorio @ no__t-0cache es 128 KB.  
  
**translationfile**\= @ no__t-2file @ no__t-3  
Especifica un archivo que contiene información de asignación para reemplazar caracteres en los nombres de los archivos cuando se mueven desde Windows @ no__t-0based a sistemas de archivos UNIX @ no__t-1based. Si no se especifica *File* , la traducción de caracteres de nombre de archivo está deshabilitada. Si se cambia el valor de **translationfile** , deberá reiniciar el servidor para que el cambio surta efecto.  
  
**dotfileshidden**\= {**sí** | **no**}  
Especifica si los archivos que se crean con nombres que comienzan por un punto \(. \) se marcarán como ocultos en el sistema de archivos de Windows y, por tanto, se ocultarán de los clientes NFS. La configuración predeterminada es **no**.  
  
**casesensitivelookups @ no__t-1**{**sí** | **no**}  
Especifica si las búsquedas de directorio distinguirán mayúsculas de minúsculas \(requiring coincide exactamente con las mayúsculas y minúsculas de los caracteres @ no__t-1.  
  
También debe deshabilitar el caso del kernel de Windows @ no__t-0insensitivity para que Server for NFS admita los nombres de archivo Case @ no__t-1sensitive. Puede deshabilitar el caso del kernel de Windows @ no__t-0insensitivity borrando la siguiente clave del registro en 0:  
  
HKLM @ no__t-0SYSTEM @ no__t-1CurrentControlSet @ no__t-2Control @ no__t-3Session Manager @ no__t-4kernel  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> Esta sección solo se aplica a Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003. Esta sección no se aplica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase @ no__t-1**{**Lower** | **Upper** | **preserve**}  
Especifica si el uso de mayúsculas o minúsculas en los nombres de los archivos del sistema de archivos NTFS se devolverá en minúsculas, mayúsculas o en el formato almacenado en el directorio. La configuración predeterminada es **preserve**. Esta configuración no se puede cambiar si **casesensitivelookups** está establecido en **yes**.  
  
*nombre* de creategroup  
crea un nuevo grupo de clientes y le asigna el *nombre*especificado.  
  
**listgroups**  
Muestra los nombres de todos los grupos de clientes.  
  
*nombre* de deletegroup  
quita el grupo de clientes especificado por *nombre*.  
  
**renamegroup** *OldName NewName*  
cambia el nombre del grupo de clientes especificado por *OldName* a *NewName* .  
  
*host de nombre*de addmembers \[... \]  
agrega el *host* al grupo de clientes especificado por *nombre*.  
  
*nombre* de listmembers  
enumera los equipos host del grupo de clientes especificado por *nombre*.  
  
*host de grupo*de deletemembers \[... \]  
quita el cliente especificado por el *host* del grupo de clientes especificado por el *Grupo*.  
  
Si no se especifica una opción de comando o un argumento, **nfsadmin Server** muestra las opciones de configuración del servidor para NFS actual.  
  
#### <a name="administering-client-for-nfs"></a>Administración de cliente para NFS  
Use el comando **nfsadmin Client** para administrar cliente para NFS. La acción específica que **nfsadmin Client** realiza depende del argumento de comando que especifique:  
  
**start**  
inicia el servicio cliente para NFS.  
  
**detener**  
Detiene el servicio cliente para NFS.  
  
**config**  
Especifica la configuración general de cliente para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :  
  
**FileAccess @ no__t-1**<em>modo</em>  
-   Especifica el modo de permisos predeterminado para los archivos creados en los servidores Network File System \(NFS @ no__t-1. El argumento de *modo* consta de tres dígitos entre 0 y 7 \(inclusive @ no__t-2 que representan los permisos predeterminados concedidos al usuario, grupo y otros \(respectively @ no__t-4. Los dígitos se traducen a los permisos de UNIX @ no__t-0style de la siguiente manera: 0 @ no__t-0none, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW y 7 @ no__t-7rwx. Por ejemplo, **FileAccess @ no__t-1750** proporciona el permiso rwx al propietario, el permiso RX en el grupo y no tiene permiso de acceso a otros usuarios.  
  
**mapsvr @ no__t-1**<em>servidor</em>  
Establece el *servidor* como servidor de asignación de nombres de usuario para cliente para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**mtype @ no__t-1**{**Hard** | **Soft**}  
Especifica el tipo de montaje predeterminado. Para un montaje forzado, cliente para NFS sigue Reintentando una RPC con errores hasta que se realiza correctamente. Para un montaje flexible, cliente para NFS devuelve un error a la aplicación que realiza la llamada después de volver a intentar la llamada el número de veces especificado por la opción de **reintento** .  
  
**vuelva a intentar @ no__t-1**<em>número</em>  
Especifica el número de veces que se intenta establecer una conexión para un montaje flexible. Este valor debe estar comprendido entre 1 y 10, ambos inclusive. El valor predeterminado es 1.  
  
**tiempo de espera: @ no__t-1**<em>segundos</em>  
Especifica el número de segundos que se va a esperar a que se llame a la llamada de procedimiento \(remote a @ no__t-1. Este valor debe ser 0,8, 0,9 o un entero comprendido entre 1 y 60, ambos incluidos. El valor predeterminado es 0,8.  
  
**Protocolo @ no__t-1 {TCP | UDP | TCP @ no__t-2UDP}**  
Especifica qué protocolos de transporte admite el cliente. La configuración predeterminada es **TCP @ no__t-1UDP**  
  
**rsize @ no__t-1**<em>tamaño</em>  
Especifica el tamaño, en kilobytes, del búfer de lectura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**wsize @ no__t-1**<em>tamaño</em>  
Especifica el tamaño, en kilobytes, del búfer de escritura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**Perf @ no__t-1default**  
Restaura los valores predeterminados de la siguiente configuración de rendimiento:  
  
-   **mtype**  
  
-   **realizar**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**FileAccess @ no__t-1**<em>modo</em>  
Especifica el modo de permisos predeterminado para los archivos creados en los servidores Network File System \(NFS @ no__t-1. El argumento de *modo* consta de tres dígitos entre 0 y 7 \(inclusive @ no__t-2 que representan los permisos predeterminados concedidos al usuario, grupo y otros \(respectively @ no__t-4. Los dígitos se traducen a los permisos de UNIX @ no__t-0style de la siguiente manera: 0 @ no__t-0none, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW y 7 @ no__t-7rwx. Por ejemplo, **FileAccess @ no__t-1750** proporciona el permiso rwx al propietario, el permiso RX en el grupo y no tiene permiso de acceso a otros usuarios.  
  
Si no se especifica una opción de comando o un argumento, **nfsadmin Client** muestra la configuración actual de cliente para NFS.  
  

