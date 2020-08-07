---
title: nfsadmin
description: Artículo de referencia para el comando nfsadmin, que administra servidor para NFS y cliente para NFS.
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797096fd7ca17c04b28f1b7490f5a8b4a58b31f6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885991"
---
# <a name="nfsadmin"></a>nfsadmin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Utilidad de línea de comandos que administra servidor para NFS o cliente para NFS en el equipo local o remoto que ejecuta servicios de Microsoft para Network File System (NFS). Si se usa sin parámetros, nfsadmin Server muestra las opciones de configuración de servidor para NFS actuales y nfsadmin Client muestra la configuración actual de cliente para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsadmin server [computername] [-u Username [-p Password]] -l
nfsadmin server [computername] [-u Username [-p Password]] -r {client | all}
nfsadmin server [computername] [-u Username [-p Password]] {start | stop}
nfsadmin server [computername] [-u Username [-p Password]] config option[...]
nfsadmin server [computername] [-u Username [-p Password]] creategroup <name>
nfsadmin server [computername] [-u Username [-p Password]] listgroups
nfsadmin server [computername] [-u Username [-p Password]] deletegroup <name>
nfsadmin server [computername] [-u Username [-p Password]] renamegroup <oldname> <newname>
nfsadmin server [computername] [-u Username [-p Password]] addmembers <hostname>[...]
nfsadmin server [computername] [-u Username [-p Password]] listmembers
nfsadmin server [computername] [-u Username [-p Password]] deletemembers <hostname><groupname>[...]
nfsadmin client [computername] [-u Username [-p Password]] {start | stop}
nfsadmin client [computername] [-u Username [-p Password]] config option[...]
```

### <a name="general-parameters"></a>Parámetros generales

| Parámetro | Descripción |
| --------- | ----------- |
| computername | Especifica el equipo remoto que desea administrar. Puede especificar el equipo con un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS), o bien mediante la dirección del Protocolo de Internet (IP). |
| -u nombre de usuario | Especifica el nombre del usuario cuyas credenciales se van a usar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato *dominio\nombredeusuario*. |
| -p contraseña | Especifica la contraseña del usuario especificado mediante la opción **-u** . Si se especifica la opción **-u** pero se omite la opción **-p** , se le solicitará la contraseña del usuario. |

### <a name="server-for-nfs-related-parameters"></a>Parámetros relacionados con servidor para NFS

| Parámetro | Descripción |
| --------- | ----------- |
| -l | Enumera todos los bloqueos retenidos por los clientes. |
| -r`{client|all}` | Libera los bloqueos mantenidos por un cliente o, si se especifica All, por todos los clientes. |
| start. | Inicia el servicio servidor para NFS. |
| stop | Detiene el servicio servidor para NFS. |
| config | Especifica la configuración general de servidor para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :<ul><li>**mapsvr = `<server>` ** : Establece el servidor como servidor de Asignación de nombres de usuario para servidor para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad sfuadmin en su lugar.</li><li>**auditlocation = `{eventlog|file|both|none}` ** : Especifica si se auditarán los eventos y dónde se registrarán los eventos. Se requiere uno de los argumentos siguientes:<ul><li>**EventLog** : especifica que los eventos auditados se registrarán solo en el registro de aplicaciones visor de eventos.</li><li>**archivo** : especifica que los eventos auditados se registrarán solo en el archivo especificado por `config fname` .</li><li>**both** : especifica que los eventos auditados se registrarán en el visor de eventos el registro de la aplicación, así como en el archivo especificado por `config fname` .</li><li>**ninguno** : especifica que los eventos no se auditan.</li></ul><li>**fname = `<file>` ** : Establece el archivo especificado por archivo como archivo de auditoría. El valor predeterminado es **%sfudir%\log \\ nfssvr. log**.</li><li>**fsize = `<size>` ** : Establece el tamaño máximo en megabytes del archivo de auditoría. El tamaño máximo predeterminado es de **7 MB**.</li><li>**`audit=[+|-]mount [+|-]read [+|-]write [+|-]create [+|-]delete [+|-]locking [+|-]all`**: Especifica los eventos que se van a registrar. Para iniciar el registro de un evento, escriba un signo más ( **+** ) antes del nombre del evento; para detener el registro de un evento, escriba un signo menos ( **-** ) antes del nombre del evento. Si se omite el signo, **+** se supone que se trata del signo. No use **All** con ningún otro nombre de evento.</li><li>**lockperiod = `<seconds>` ** : Especifica el número de segundos que servidor para NFS esperará para reclamar bloqueos después de que se haya perdido la conexión con servidor para NFS y, a continuación, se haya reestablecido o después de que se haya reiniciado el servicio servidor para NFS.</li><li>**portmapprotocol = `{TCP|UDP|TCP+UDP}` ** : Especifica qué protocolos de transporte portmap admite. La configuración predeterminada es **TCP + UDP**.</li><li>**mountprotocol = `{TCP|UDP|TCP+UDP}` ** : Especifica qué protocolos de transporte admite el montaje. La configuración predeterminada es **TCP + UDP**.</li><li>**nfsprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica qué protocolos de transporte admite el sistema de archivos de red (NFS). La configuración predeterminada es **TCP + UDP**</li><li>**nlmprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica qué protocolos de transporte admite el administrador de bloque de red (NLM). La configuración predeterminada es **TCP + UDP**.</li><li>**nsmprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica qué protocolos de transporte admite el administrador de estado de red (NSM). La configuración predeterminada es **TCP + UDP**.</li><li>**enableV3 = `{yes|no}` ** : Especifica si se admitirán los protocolos de la versión 3 de NFS. El valor predeterminado es **sí**.</li><li>**renewauth = `{yes|no}` ** : Especifica si será necesario volver a autenticar las conexiones de cliente después del período especificado por config renewauthinterval. La configuración predeterminada es **no**.</li><li>**renewauthinterval = `<seconds>` ** : Especifica el número de segundos que transcurren antes de que se fuerce la reautenticación de un cliente si `config renewauth` se establece en **sí**. El valor predeterminado es **600 segundos**.</li><li>**dircache = `<size>` ** : Especifica el tamaño en kilobytes de la memoria caché del directorio. El número especificado como tamaño debe ser un múltiplo de 4 entre 4 y 128. El tamaño predeterminado de la caché del directorio es de **128 KB**.</li><li>**translationfile = `<file>` ** : Especifica un archivo que contiene información de asignación para reemplazar caracteres en los nombres de los archivos cuando se mueven desde sistemas de archivos basados en Windows a sistemas de archivos basados en UNIX. Si no se especifica File, la traducción de caracteres de nombre de archivo está deshabilitada. Si se cambia el valor de **translationfile** , deberá reiniciar el servidor para que el cambio surta efecto.</li><li>**dotfileshidden = `{yes|no}` ** : Especifica si los archivos con nombres que comienzan por un punto (.) se marcan como ocultos en el sistema de archivos de Windows y, por tanto, se ocultan de los clientes NFS. La configuración predeterminada es **no**.</li><li>**casesensitivelookups = `{yes|no}` ** : Especifica si las búsquedas de directorio distinguen mayúsculas de minúsculas (requieren coincidencia exacta de mayúsculas de minúsculas).<p>También debe deshabilitar la distinción de mayúsculas y minúsculas del kernel de Windows para admitir nombres de archivo que distinguen mayúsculas de minúsculas. Para admitir la distinción de mayúsculas y minúsculas, cambie el valor **DWORD** de la clave del registro, `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel` , a **0**.</li><li>**ntfscase = `{lower|upper|preserve}` ** : Especifica si el uso de mayúsculas o minúsculas en los nombres de los archivos del sistema de archivos NTFS se devolverá en minúsculas, mayúsculas o en el formato almacenado en el directorio. La configuración predeterminada es **preserve**. Esta configuración no se puede cambiar si **casesensitivelookups** está establecido en **sí**.</li></ul> |
| creategroup`<name>` | Crea un nuevo grupo de clientes y le asigna el nombre especificado. |
| listgroups | Muestra los nombres de todos los grupos de clientes. |
| llamadas`<name>` | Quita el grupo de clientes especificado por nombre. |
| renamegroup `<oldname>``<newname>` | Cambia el nombre del grupo de clientes especificado por *oldname* a *NewName*. |
| addmembers`<hostname>[...]` | Agrega un *host* al grupo de clientes especificado por *nombre*. |
| listmembers`<name>` | Enumera los equipos host del grupo de clientes especificado por *nombre*. |
| deletemembers`<hostname><groupname>[...]` | Quita el cliente especificado por el *host* del grupo de clientes especificado por el *Grupo*. |

### <a name="client-for-nfs-related-parameters"></a>Parámetros relacionados con el cliente para NFS

| Parámetro | Descripción |
| --------- | ----------- |
| start | Inicia el servicio cliente para NFS. |
| stop | Detiene el servicio cliente para NFS. |
| config | Especifica la configuración general de cliente para NFS. Debe proporcionar al menos una de las siguientes opciones con el argumento del comando de **configuración** :<ul><li>**FileAccess = `<mode>` ** : Especifica el modo de permisos predeterminado para los archivos creados en los servidores de Network File System (NFS). El argumento de **modo** consta de un número de tres dígitos, de 0 a 7 (incluido), que representan los permisos predeterminados concedidos al usuario, grupo y otros. Los dígitos se traducen a permisos de estilo UNIX de la siguiente manera: *0 = ninguno*, *1 = x (ejecutar)*, *2 = w (solo escritura)*, *3 = WX (escritura y ejecución)*, *4 = r (solo lectura)*, *5 = RX (lectura y ejecución)*, *6 = RW (lectura y escritura*) y *7 = rwx (lectura, escritura y ejecución*) Por ejemplo, `fileaccess=750` proporciona permisos de lectura, escritura y ejecución al propietario, permisos de lectura y ejecución para el grupo y ningún permiso de acceso a otros usuarios.</li><li>**mapsvr = `<server>` ** : Establece el servidor como servidor de Asignación de nombres de usuario para cliente para NFS. Aunque esta opción sigue siendo compatible con la compatibilidad con versiones anteriores, debe usar la utilidad sfuadmin en su lugar.</li><li>**mtype = `{hard|soft}` ** : Especifica el tipo de montaje predeterminado. Para un montaje forzado, cliente para NFS sigue Reintentando una RPC con errores hasta que se realiza correctamente. Para un montaje flexible, cliente para NFS devuelve un error a la aplicación que realiza la llamada después de volver a intentar la llamada el número de veces especificado por la opción de reintento.</li><li>**Reintentar = `<number>` ** : Especifica el número de veces que se intenta establecer una conexión para un montaje flexible. Este valor debe estar comprendido entre 1 y 10, ambos inclusive. El valor predeterminado es **1**.</li><li>**tiempo de `<seconds>` espera =** : Especifica el número de segundos que se debe esperar una conexión (llamada a procedimiento remoto). Este valor debe ser *0,8*, *0,9*o un entero comprendido entre *1 y 60*, ambos incluidos. El valor predeterminado es **0,8**.</li><li>**Protocolo = `{TCP|UDP|TCP+UDP}` ** : Especifica qué protocolos de transporte admite el cliente. La configuración predeterminada es **TCP + UDP**.</li><li>**rsize = `<size>` ** : Especifica el tamaño, en kilobytes, del búfer de lectura. Este valor puede ser *0,5, 1, 2, 4, 8, 16* o *32*. El valor predeterminado es **32**.</li><li>**wsize = `<size>` ** : Especifica el tamaño, en kilobytes, del búfer de escritura. Este valor puede ser *0,5, 1, 2, 4, 8, 16* o *32*. El valor predeterminado es **32**.</li><li>**Perf = default** : restaura la siguiente configuración de rendimiento a los valores predeterminados: *mtype*, *Retry*, *timeout*, *rsize*o *wsize*. |

### <a name="examples"></a>Ejemplos

Para detener servidor para NFS o cliente para NFS, escriba:

```
nfsadmin server stop
nfsadmin client stop
```

Para iniciar servidor para NFS o cliente para NFS, escriba:

```
nfsadmin server start
nfsadmin client start
```

Para configurar servidor para NFS para que no distinga entre mayúsculas y minúsculas, escriba:

```
nfsadmin server config casesensitive=no
```

Para establecer que cliente para NFS distinga entre mayúsculas y minúsculas, escriba:

```
nfsadmin client config casesensitive=yes
```

Para mostrar todas las opciones de servidor para NFS o cliente para NFS actuales, escriba:

```
nfsadmin server config
nfsadmin client config
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de cmdlets de NFS](/powershell/module/nfs)
