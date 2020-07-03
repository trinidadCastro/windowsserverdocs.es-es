---
title: prnport
description: Artículo de referencia del comando prnport, que crea, elimina y enumera los puertos de impresora TCP/IP estándar, además de mostrar y cambiar la configuración del puerto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b373547050d3d3dfb1d64160959c8dbb9e6f5c5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931167"
---
# <a name="prnport"></a>prnport

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea, elimina y enumera los puertos de impresora TCP/IP estándar, además de mostrar y cambiar la configuración del puerto. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnport o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport`.

## <a name="syntax"></a>Sintaxis

```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <portname>] [-s <Servername>] [-u <Username>] [-w <password>] [-o {raw | lpr}] [-h <Hostaddress>] [-q <Queuename>] [-n <portnumber>] -m{e | d} [-i <SNMPindex>] [-y <communityname>] -2{e | -d}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -a | Crea un puerto de impresora TCP/IP estándar. |
| -d | Elimina un puerto de impresora TCP/IP estándar. |
| -l | Enumera todos los puertos de impresora TCP/IP estándar en el equipo especificado por el parámetro **-s** . |
| -g | Muestra la configuración de un puerto de impresora TCP/IP estándar. |
| -T | Configura los valores de puerto para un puerto de impresora TCP/IP estándar. |
| -r`<portname>` | Especifica el puerto al que está conectada la impresora. |
| -s`<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -u `<Username>` -w`<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| -o`{raw|lpr}` | Especifica el protocolo que usa el puerto: TCP RAW o TCP LPR. El protocolo TCP RAW es un protocolo de mayor rendimiento en Windows que el protocolo LPR. Si usa TCP RAW, opcionalmente puede especificar el número de puerto mediante el parámetro **-n** . El número de puerto predeterminado es 9100. |
| -h`<Hostaddress>` | Especifica (por dirección IP) la impresora para la que desea configurar el puerto. |
| -q`<Queuename>` | Especifica el nombre de la cola para un puerto TCP sin formato. |
| -n`<portnumber>` | Especifica el número de puerto para un puerto TCP sin formato. El número de puerto predeterminado es 9100. |
| -m`{e|d}` | Especifica si SNMP está habilitado. El parámetro **e** habilita SNMP. El parámetro **d** deshabilita SNMP. |
| -i`<SNMPindex` | Especifica el índice SNMP, si SNMP está habilitado. Para obtener más información, consulte **rfc 1759** en el [sitio web del editor RFC](https://www.ietf.org/rfc/rfc1759.txt?number=1759). |
| -y`<communityname>` | Especifica el nombre de la comunidad SNMP, si SNMP está habilitado. |
| -2`{e|-d}` | Especifica si las colas de impresión dobles (también conocidas como rebobinar) están habilitadas para los puertos TCP LPR. Las colas de impresión dobles son necesarias porque TCP LPR debe incluir un recuento exacto de bytes en el archivo de control que se envía a la impresora, pero el Protocolo no puede obtener el recuento del proveedor de impresión local. Por lo tanto, cuando un archivo se pone en cola en una cola de impresión lpr de TCP, también se pone en cola como un archivo temporal en el directorio system32. TCP LPR determina el tamaño del archivo temporal y envía el tamaño al servidor que ejecuta LPD. El parámetro **e** habilita las colas dobles. El parámetro **d** deshabilita las colas de impresión dobles. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

### <a name="examples"></a>Ejemplos

Para mostrar todos los puertos de impresión TCP/IP estándar en el servidor \\ Servidor1, escriba:

```
cscript prnport -l -s Server1
```

Para eliminar el puerto de impresión TCP/IP estándar en el servidor \\ Servidor1 que se conecta a una impresora de red en 10.2.3.4, escriba:

```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```

Para agregar un puerto de impresión TCP/IP estándar en el servidor \\ Servidor1 que se conecta a una impresora de red en 10.2.3.4 y usa el protocolo sin formato TCP en el puerto 9100, escriba:

```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```

Para habilitar SNMP, especifique el nombre de comunidad "público" y establezca el índice de SNMP en 1 en una impresora de red en 10.2.3.4 compartida por el servidor \\ Servidor1; escriba:

```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```

Para agregar un puerto de impresión TCP/IP estándar en el equipo local que se conecta a una impresora de red en 10.2.3.4 y obtener automáticamente la configuración del dispositivo de la impresora, escriba:

```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
