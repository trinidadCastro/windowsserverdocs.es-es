---
title: nfsadmin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4dc49e23d67ae68c598367de5a3fb0d7d6398a8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437165"
---
# <a name="nfsadmin"></a>nfsadmin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar **nfsadmin** para administrar servidor para NFS y cliente para NFS.  
  
## <a name="syntax"></a>Sintaxis  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName*`[`\-p *contraseña* `]]` \-l  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` \-r `{` *cliente* `|` todas`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]] {`iniciar `|` detener`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` config *opción*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` creategroup *nombre*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` listgroups  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` deletegroup *nombre*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` addmembers *nombre de Host*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` listmembers  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` deletemembers *grupo Host*`[...]`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]] {`iniciar `|` detener`}`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *contraseña* `]]` config *opción*`[...]`  
  
## <a name="description"></a>Descripción  
El **nfsadmin** comando\-administra la utilidad de línea de servidor para NFS o cliente para NFS en el equipo local o remoto que ejecuta Services de Microsoft para Network File System \(NFS\). Si ha iniciado sesión con una cuenta que no tiene los privilegios necesarios, puede especificar un nombre de usuario y contraseña de una cuenta que sí. La acción realizada por **nfsadmin** depende de los argumentos de comando que proporcione.  
  
Además de servicio\-argumentos de comandos específicos y opciones, **nfsadmin** acepta los siguientes:  
  
*computerName*  
Especifica el equipo remoto que desea administrar. Puede especificar el equipo con un servicio de nombres Internet de Windows \(WINS\) nombre o un sistema de nombres de dominio \(DNS\) nombre, o mediante el protocolo Internet \(IP\) dirección.  
  
**\-u** *UserName*  
Especifica el nombre de usuario del usuario cuyas credenciales se van a usarse. Podría ser necesario agregar el nombre de dominio al nombre de usuario en el formulario <em>dominio</em> **\\** <em>nombre de usuario</em>  
  
**\-p** *contraseña*  
Especifica la contraseña del usuario especificado mediante el  **\-u** opción. Si especifica la  **\-u** opción pero omita el  **\-p** opción, se le solicitará la contraseña del usuario.  
  
#### <a name="administering-server-for-nfs"></a>Administración del servidor para NFS  
Use la **nfsadmin server** comandos para administrar servidor para NFS. La acción concreta que **nfsadmin server** toma depende de la opción de comando o un argumento que especifique:  
  
**\-l**  
Enumera todos los bloqueos retenidos por los clientes.  
  
**\-r** {*client* | **all**}  
Libera los bloqueos mantenidos por un *cliente* o, si **todas** se especifica, todos los clientes.  
  
**start**  
inicia el servicio servidor para NFS.  
  
**stop**  
Detiene el servicio servidor para NFS.  
  
**config**  
Especifica la configuración general de servidor para NFS. Debe proporcionar al menos una de las siguientes opciones con la **config** argumento de comando:  
  
**mapsvr\=** <em>server</em>  
Conjuntos de *server* como el servidor de asignación de nombres de usuario para el servidor para NFS. Aunque esta opción sigue admitiendo por compatibilidad con versiones anteriores, se debe utilizar el **sfuadmin** utilidad en su lugar.  
  
**auditlocation\=** {**eventlog** | **archivo** | **ambos** | **ninguno** }  
Especifica si se va a auditar los eventos y que se registrarán los eventos. Falta uno de los siguientes argumentos.  
  
**eventlog**  
Especifica que se registrarán los eventos auditados solo en la aplicación Visor de registro de eventos.  
  
**file**  
Especifica que se registrarán los eventos auditados solo en el archivo especificado por **config fname**.  
  
**both**  
Especifica que se registrarán los eventos auditados en el registro de aplicación del Visor de eventos, así como el archivo especificado por **config fname**.  
  
**Ninguno**  
Especifica que no se auditarán los eventos.  
  
**fname\=** <em>archivo</em>  
Establece el archivo especificado por *archivo* como el archivo de auditoría. El valor predeterminado es % sfudir %\\registro\\nfssvr.log  
  
**fsize\=** \=*size*  
Conjuntos de *tamaño* como el tamaño máximo en megabytes del archivo de auditoría. El tamaño máximo predeterminado es 7 MB.  
  
**auditar\=** \[ **\+** | **\-** \]**montar** \[ **\+** | **\-** \] **leer** \[ **\+** | **\-** \] **escribir** \[ **\+** | **\-** \] **crear** \[ **\+** | **\-** \] **eliminar** \[ **\+** | **\-** \] **bloqueo** \[ **\+** | **\-** \] **todas**  
Especifica los eventos se registrarán. Para empezar a registrar un evento, escriba un signo \( **\+** \) delante del nombre del evento; para detener el registro de un evento, escriba un signo menos \( **\-** \) delante del nombre del evento. Si se omite el inicio de sesión, se supone el signo más. No use **todas** con ningún otro nombre de evento.  
  
**lockperiod\=** <em>segundos</em>  
Especifica el número de segundos que esperará el servidor para NFS para recuperar los bloqueos después de haber perdido una conexión al servidor para NFS y, a continuación, puede restablecer o después de reiniciar el servicio servidor para NFS.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Especifica los protocolos de transporte admite Portmap. El valor predeterminado es **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Especifica el tipo de transporte protocolos montar admite. El valor predeterminado es **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Especifica el tipo de transporte protocolos Network File System \(NFS\) admite. El valor predeterminado es **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica el tipo de transporte protocolos Administrador de bloqueos de red \(NLM\) admite. El valor predeterminado es **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Especifica el tipo de transporte protocolos Administrador de estado de la red \(NSM\) admite. El valor predeterminado es **TCP\+UDP**.  
  
**enableV3\=** {**yes** | **no**}  
Especifica si se admitirán los protocolos de la versión 3 de NFS. El valor predeterminado es **Sí**.  
  
**renewauth\=** {**yes** | **no**}  
Especifica si las conexiones de cliente deberán volver a autenticarse tras el período especificado por **config renewauthinterval**. El valor predeterminado es **ningún**.  
  
**renewauthinterval\=** <em>seconds</em>  
Especifica el número de segundos que transcurren antes de que un cliente se ve obligado a volver a autenticarse si **config renewauth** está establecido en **Sí**. El valor predeterminado es 600 segundos.  
  
**dircache\=** <em>size</em>  
Especifica el tamaño en kilobytes, de la caché de directorio. El número especificado como *tamaño* debe ser un múltiplo de 4 entre 4 y 128. El directorio predeterminado\-tamaño de caché es de 128 KB.  
  
**translationfile**\=\[file\]  
Especifica un archivo que contiene información de asignación para reemplazar caracteres en los nombres de archivos al moverlos desde Windows\-basado en UNIX\-en función de los sistemas de archivos. Si *archivo* no se especifica, se deshabilita la traducción de caracteres de nombre de archivo. Si el valor de **translationfile** es cambia, debe reiniciar el servidor para que el cambio surta efecto.  
  
**dotfileshidden**\={**yes** | **no**}  
Especifica si los archivos que están creadas con nombres que empiecen por un período \(.\) se marcados como ocultos en el sistema de archivos de Windows y por consiguiente ocultarse a los clientes NFS. El valor predeterminado es **ningún**.  
  
**casesensitivelookups\=** {**Sí** | **ningún**}  
Especifica si las búsquedas de directorio se distinguen mayúsculas de minúsculas \(que requieren una coincidencia exacta del caso de carácter\).  
  
También deberá deshabilitar el caso del kernel de Windows\-minúsculas en orden para servidor para NFS admita caso\-nombres de los archivos confidenciales. Puede deshabilitar el caso del kernel de Windows\-no distinguir desactivando la siguiente clave del registro en 0:  
  
HKLM\\sistema\\CurrentControlSet\\Control\\Administrador de sesión\\kernel  
  
Obcaseinsensitive DWOrd   
  
> [!IMPORTANT]  
> En esta sección solo se aplica a Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003. En esta sección no se aplica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=** {**lower** | **upper** | **preserve**}  
Especifica si se devolverá el caso de los caracteres en los nombres de archivos en el sistema de archivos NTFS en minúsculas, mayúsculas, o en el formulario que se almacenan en el directorio. El valor predeterminado es **conservar**. No se puede cambiar esta configuración si **casesensitivelookups** está establecido en **Sí**.  
  
**creategroup** *name*  
crea un nuevo grupo de cliente, dándole especificado *nombre*.  
  
**listgroups**  
Muestra los nombres de todos los grupos de clientes.  
  
**deletegroup** *name*  
Quita el grupo de cliente especificado por *nombre*.  
  
**renamegroup** *OldName NewName*  
cambia el nombre del grupo de cliente especificado por *OldName* a *NewName*  
  
**AddMembers** *nombre Host*\[...\]  
Agrega *Host* al grupo de cliente especificado por *nombre*.  
  
**ListMembers** *nombre*  
Enumera los equipos host en el grupo de cliente especificado por *nombre*.  
  
**deletemembers** *grupo Host*\[...\]  
Quita el cliente especificado por *Host* desde el grupo de cliente especificado por *grupo*.  
  
Si no especifica un argumento, o la opción de comando **nfsadmin server** muestra el servidor actual para la configuración de NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administración de cliente para NFS  
Use la **nfsadmin client** comandos para administrar el cliente para NFS. La acción concreta que **nfsadmin client** toma depende del argumento de comando que especifique:  
  
**start**  
inicia al servicio cliente para NFS.  
  
**stop**  
Detiene al servicio cliente para NFS.  
  
**config**  
Especifica las opciones generales de cliente para NFS. Debe proporcionar al menos una de las siguientes opciones con la **config** argumento de comando:  
  
**fileaccess\=** <em>mode</em>  
-   Especifica el modo de permiso predeterminado para los archivos creados en Network File System \(NFS\) servidores. El *modo* argumento consta de un tres dígitos de 0 a 7 \(inclusivo\) que representa los permisos predeterminados concedidos al usuario, grupo y otros \(respectivamente\). Los dígitos se traducen en UNIX\-estilo permisos como sigue: 0\=none, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw y 7\=rwx. Por ejemplo, **fileaccess\=750** ofrece rwx permiso al propietario, permiso de rx para el grupo y no tiene permiso de acceso a otros usuarios.  
  
**mapsvr\=** <em>server</em>  
Conjuntos de *server* como el servidor de asignación de nombres de usuario de cliente para NFS. Aunque esta opción sigue admitiendo por compatibilidad con versiones anteriores, se debe utilizar el **sfuadmin** utilidad en su lugar.  
  
**mtype\=** {**hard** | **soft**}  
Especifica el tipo de montaje predeterminado. Para un montaje forzado, cliente para NFS continúa intentándolo una llamada RPC con errores hasta que lo consiga. Para un montaje flexible, cliente para NFS devuelve un error en la aplicación que realiza la llamada después de volver a intentar la llamada el número de veces especificado por el **vuelva a intentar** opción.  
  
**retry\=** <em>number</em>  
Especifica el número de veces que se intenta realizar una conexión para un montaje flexible. Este valor debe ser de 1 a 10, inclusive. El valor predeterminado es 1.  
  
**timeout\=** <em>seconds</em>  
Especifica el número de segundos de espera para una conexión \(llamada a procedimiento remoto\). Este valor debe ser 0,8, 0,9 o un entero entre 1 y 60, ambos inclusive. El valor predeterminado es 0,8.  
  
**Protocolo\={TCP | UDP | TCP\+UDP}**  
Especifica el tipo de transporte protocolos admita el cliente. El valor predeterminado es **TCP\+UDP**  
  
**rsize\=** <em>size</em>  
Especifica el tamaño, en kilobytes, del búfer de lectura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**wsize\=** <em>size</em>  
Especifica el tamaño, en kilobytes, del búfer de escritura. Este valor puede ser 0,5, 1, 2, 4, 8, 16 o 32. El valor predeterminado es 32.  
  
**perf\=default**  
Restaura las siguientes opciones de rendimiento para los valores predeterminados:  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>mode</em>  
Especifica el modo de permiso predeterminado para los archivos creados en Network File System \(NFS\) servidores. El *modo* argumento consta de un tres dígitos de 0 a 7 \(inclusivo\) que representa los permisos predeterminados concedidos al usuario, grupo y otros \(respectivamente\). Los dígitos se traducen en UNIX\-estilo permisos como sigue: 0\=none, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw y 7\=rwx. Por ejemplo, **fileaccess\=750** ofrece rwx permiso al propietario, permiso de rx para el grupo y no tiene permiso de acceso a otros usuarios.  
  
Si no especifica un argumento, o la opción de comando **nfsadmin client** muestra el cliente actual para la configuración de NFS.  
  

