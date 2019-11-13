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
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario*`[`\-p *contraseña*`]]` \-  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` \-r `{`*cliente* `|` todo`}`  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]] {`Start `|` STOP`}`  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña* *`]]` config`[...]`*  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` *nombre* de creategroup  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` listgroups  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` *nombre* de deletegroup  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` *el host del nombre* de addmembers`[...]`  
  
**nfsadmin server** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` listmembers  
  
**nfsadmin server** `[`*NombreDeEquipo*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]]` *host de grupo* deletemembers`[...]`  
  
**nfsadmin client** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña*`]] {`Start `|` STOP`}`  
  
**nfsadmin client** `[`*computerName*`] [`\-u *nombredeusuario* `[`\-p *contraseña* *`]]` config`[...]`*  
  
## <a name="description"></a>Descripción  
La utilidad de línea de\-de comandos **nfsadmin** administra servidor para NFS o cliente para NFS en el equipo local o remoto que ejecuta los servicios de Microsoft para Network File System \(NFS\). Si ha iniciado sesión con una cuenta que no tiene los privilegios necesarios, puede especificar un nombre de usuario y una contraseña de una cuenta que sí los tenga. La acción realizada por **nfsadmin** depende de los argumentos de comando que suministre.  
  
Además de las opciones y los argumentos de comando específicos de Service\-, **nfsadmin** acepta lo siguiente:  
  
*NombreDeEquipo*  
Especifica el equipo remoto que desea administrar. Puede especificar el equipo mediante un servicio de nombres de Internet de Windows \(nombre de\) WINS o un sistema de nombres de dominio \(nombre de\) DNS, o bien por el protocolo de Internet \(dirección IP\).  
  
**\-u** *nombreDeUsuario*  
Especifica el nombre de usuario del usuario cuyas credenciales se van a utilizar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em> **\\** <em>nombreDeUsuario</em>  
  
**\-p** *contraseña*  
Especifica la contraseña del usuario especificado mediante la opción **\-u** . Si especifica la opción **\-u** pero omite la opción **\-p** , se le pedirá la contraseña del usuario.  
  
#### <a name="administering-server-for-nfs"></a>Administrar servidor para NFS  
Use el comando **nfsadmin Server** para administrar servidor para NFS. La acción específica que **nfsadmin Server** realiza depende de la opción de comando o el argumento que especifique:  
  
**\-l**  
enumera todos los bloqueos retenidos por los clientes.  
  
**\-r** {*cliente* | **todos**}  
Libera los bloqueos mantenidos por un *cliente* o, si se especifica **All** , por todos los clientes.  
  
**start**  
inicia el servicio servidor para NFS.  
  
**detener**  
Detiene el servicio servidor para NFS.  
  
**config**  
Especifica la configuración general de servidor para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :  
  
**mapsvr\=** <em>Server</em>  
Establece el *servidor* como servidor de asignación de nombres de usuario para servidor para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**auditlocation\=** {**eventlog** | **File** | **both** | **None**}  
Especifica si se auditarán los eventos y dónde se registrarán los eventos. Se requiere uno de los siguientes argumentos.  
  
**EventLog**  
Especifica que los eventos auditados se registrarán solo en el registro de la aplicación Visor de eventos.  
  
**filesystem**  
Especifica que los eventos auditados se registrarán solo en el archivo especificado por **config fname**.  
  
**ambos**  
Especifica que los eventos auditados se registrarán en el registro de la aplicación Visor de eventos, así como en el archivo especificado por **config fname**.  
  
**ninguna**  
Especifica que no se auditarán los eventos.  
  
**fname** ( <em>archivo</em>\=)  
Establece el archivo especificado por *archivo* como archivo de auditoría. El valor predeterminado es% sfudir%\\registro\\nfssvr. log.  
  
*tamaño* de\=de **fsize\=**  
Establece el *tamaño* como el tamaño máximo en megabytes del archivo de auditoría. El tamaño máximo predeterminado es de 7 MB.  
  
**audit\=** \[ **\+** | **\-** \]**montar** \[ **\+|\-** \]**read** \[ **\+** | **\-** \]**escritura** \[ **\+** | **\-** \]**Create** \[ **\+|\-** \]**Delete** \[ **\+** | **\-** \]**bloqueo** \[ **\+|\-** \]**todo**  
Especifica los eventos que se van a registrar. Para iniciar el registro de un evento, escriba un signo más \( **\+** \) antes del nombre del evento; para detener el registro de un evento, escriba un signo menos \( **\-** \) antes del nombre del evento. Si se omite el signo, se supone el signo más. No use **All** con ningún otro nombre de evento.  
  
**lockperiod\=** <em>segundos</em>  
Especifica el número de segundos que servidor para NFS esperará para reclamar bloqueos después de que se haya perdido la conexión con servidor para NFS y se haya reestablecido o después de que se haya reiniciado el servicio servidor para NFS.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Especifica qué protocolos de transporte portmap admite. La configuración predeterminada es **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el montaje. La configuración predeterminada es **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el sistema de archivos de red \(NFS\). La configuración predeterminada es **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el administrador de bloqueos de red \(NLM\). La configuración predeterminada es **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica qué protocolos de transporte admite el administrador de estado de red \(NSM\). La configuración predeterminada es **TCP\+UDP**.  
  
**enableV3\=** {**yes** | **no**}  
Especifica si se admitirán los protocolos de la versión 3 de NFS. El valor predeterminado es **sí**.  
  
**renewauth\=** {**yes** | **no**}  
Especifica si será necesario volver a autenticar las conexiones de cliente después del período especificado por **config renewauthinterval**. La configuración predeterminada es **no**.  
  
**renewauthinterval\=** <em>segundos</em>  
Especifica el número de segundos que transcurren antes de que se fuerce a un cliente a volver a autenticarse si la **configuración renewauth** está establecida en **sí**. El valor predeterminado es 600 segundos.  
  
<em>tamaño</em> de\=de dircache  
Especifica el tamaño en kilobytes de la memoria caché del directorio. El número especificado como *tamaño* debe ser un múltiplo de 4 entre 4 y 128. El directorio predeterminado\-tamaño de caché es de 128 KB.  
  
**translationfile**\=\[archivo\]  
Especifica un archivo que contiene información de asignación para reemplazar caracteres en los nombres de los archivos cuando se mueven desde Windows\-basados en sistemas de archivos basados en UNIX\-. Si no se especifica *File* , la traducción de caracteres de nombre de archivo está deshabilitada. Si se cambia el valor de **translationfile** , deberá reiniciar el servidor para que el cambio surta efecto.  
  
**dotfileshidden**\={**yes** | **no**}  
Especifica si los archivos que se crean con nombres que comienzan por un punto \(.\) se marcarán como ocultos en el sistema de archivos de Windows y, por tanto, se ocultarán de los clientes NFS. La configuración predeterminada es **no**.  
  
**casesensitivelookups\=** {**yes** | **no**}  
Especifica si las búsquedas de directorio distinguirán mayúsculas de minúsculas \(que requieran una coincidencia exacta de\)de casos de caracteres.  
  
También debe deshabilitar la\-de la distinción de mayúsculas y minúsculas del kernel de Windows para que servidor para NFS admita nombres de archivo confidenciales\-de mayúsculas o minúsculas. Puede deshabilitar la\-de la distinción de mayúsculas y minúsculas del kernel de Windows borrando la siguiente clave del registro en 0:  
  
HKLM\\SYSTEM\\CurrentControlSet\\control\\administrador de sesiones\\kernel  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> Esta sección solo se aplica a Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003. Esta sección no se aplica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=** {**lower** | **Upper** | **preserve**}  
Especifica si el uso de mayúsculas o minúsculas en los nombres de los archivos del sistema de archivos NTFS se devolverá en minúsculas, mayúsculas o en el formato almacenado en el directorio. La configuración predeterminada es **preserve**. Esta configuración no se puede cambiar si **casesensitivelookups** está establecido en **yes**.  
  
*nombre* de creategroup  
crea un nuevo grupo de clientes y le asigna el *nombre*especificado.  
  
**listgroups**  
Muestra los nombres de todos los grupos de clientes.  
  
*nombre* de deletegroup  
quita el grupo de clientes especificado por *nombre*.  
  
**renamegroup** *OldName NewName*  
cambia el nombre del grupo de clientes especificado por *OldName* a *NewName* .  
  
*host de nombre* de addmembers\[...\]  
agrega el *host* al grupo de clientes especificado por *nombre*.  
  
*nombre* de listmembers  
enumera los equipos host del grupo de clientes especificado por *nombre*.  
  
\[de *host de grupo* de **deletemembers** ...\]  
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
  
<em>modo</em> de **\=FileAccess**  
-   Especifica el modo de permisos predeterminado para los archivos creados en los servidores de Network File System \(NFS\). El argumento de *modo* consta de tres dígitos entre 0 y 7 \(\) inclusivas que representan los permisos predeterminados concedidos al usuario, grupo y otros \(\). Los dígitos se traducen a los permisos de estilo de\-de UNIX de la siguiente manera: 0\=ninguno, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW y 7\=rwx. Por ejemplo, **fileaccess\=750** proporciona el permiso rwx al propietario, el permiso RX para el grupo y ningún permiso de acceso a otros usuarios.  
  
**mapsvr\=** <em>Server</em>  
Establece el *servidor* como servidor de asignación de nombres de usuario para cliente para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad **sfuadmin** en su lugar.  
  
**mtype\=** {**Hard** | **Soft**}  
Especifica el tipo de montaje predeterminado. Para un montaje forzado, cliente para NFS sigue Reintentando una RPC con errores hasta que se realiza correctamente. Para un montaje flexible, cliente para NFS devuelve un error a la aplicación que realiza la llamada después de volver a intentar la llamada el número de veces especificado por la opción de **reintento** .  
  
<em>número</em> de **\=de reintentos**  
Especifica el número de veces que se intenta establecer una conexión para un montaje flexible. Este valor debe estar comprendido entre 1 y 10, ambos inclusive. El valor predeterminado es 1.  
  
**tiempo de espera\=** <em>segundos</em>  
Especifica el número de segundos que se debe esperar una conexión \(llamada a procedimiento remoto\). Este valor debe ser 0,8, 0,9 o un entero comprendido entre 1 y 60, ambos incluidos. El valor predeterminado es 0,8.  
  
**Protocolo\={TCP | UDP | TCP\+UDP}**  
Especifica qué protocolos de transporte admite el cliente. La configuración predeterminada es **TCP\+UDP**  
  
<em>tamaño</em> de **\=rsize**  
Especifica el tamaño, en kilobytes, del búfer de lectura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
<em>tamaño</em> de\=de wsize  
Especifica el tamaño, en kilobytes, del búfer de escritura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**rendimiento\=predeterminado**  
Restaura los valores predeterminados de la siguiente configuración de rendimiento:  
  
-   **mtype**  
  
-   **realizar**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>modo</em> de **\=FileAccess**  
Especifica el modo de permisos predeterminado para los archivos creados en los servidores de Network File System \(NFS\). El argumento de *modo* consta de tres dígitos entre 0 y 7 \(\) inclusivas que representan los permisos predeterminados concedidos al usuario, grupo y otros \(\). Los dígitos se traducen a los permisos de estilo de\-de UNIX de la siguiente manera: 0\=ninguno, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW y 7\=rwx. Por ejemplo, **fileaccess\=750** proporciona el permiso rwx al propietario, el permiso RX para el grupo y ningún permiso de acceso a otros usuarios.  
  
Si no se especifica una opción de comando o un argumento, **nfsadmin Client** muestra la configuración actual de cliente para NFS.  
  

