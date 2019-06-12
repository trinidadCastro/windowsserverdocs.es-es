---
title: prnport
description: Aprenda a crear, eliminar y enumerar puertos de impresora.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a7c48dcfa4db548d27722ee316eda797fd1ef2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442132"
---
# <a name="prnport"></a>prnport

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea, elimina y enumera los puertos de impresora TCP/IP estándar, además de mostrar y cambiar la configuración del puerto.

## <a name="syntax"></a>Sintaxis
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                                                                                                                                                                                     Descripción                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       crea un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                                        |
|              -d              |                                                                                                                                                                                                                                                                                       elimina un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             Enumera todos los puertos de impresora TCP/IP estándar en el equipo especificado con la **-s** parámetro.                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Muestra la configuración de un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           Configura las opciones de puerto para un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                           |
|        -r \<PortName>        |                                                                                                                                                                                                                                                                                Especifica el puerto al que está conectada la impresora.                                                                                                                                                                                                                                                                                 |
|       -s \<ServerName >       |                                                                                                                                                                                                                               Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                                                                                                                                |
| -u \<UserName > -w <Password> |                                                                                                              Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando.                                                                                                               |
|     -o {raw &#124; lpr}      |                                                                                                                                                                                                              Especifica el puerto que utiliza el protocolo que: TCP sin formato o TCP lpr. Si utiliza TCP sin formato, también puede especificar el número de puerto mediante el uso de la **- n** parámetro. El número de puerto predeterminado es 9100.                                                                                                                                                                                                              |
|      -h \<dirección >       |                                                                                                                                                                                                                                                                   Especifica la impresora que desea configurar el puerto (por dirección IP).                                                                                                                                                                                                                                                                    |
|       -q \<QueueName>        |                                                                                                                                                                                                                                                                                     Especifica el nombre de cola para un puerto TCP sin formato.                                                                                                                                                                                                                                                                                     |
|       -n \<PortNumber >       |                                                                                                                                                                                                                                                                    Especifica el número de puerto para un puerto TCP sin formato. El número de puerto predeterminado es 9100.                                                                                                                                                                                                                                                                    |
|        -m{e &#124; d}        |                                                                                                                                                                                                                                                       Especifica si está habilitado el SNMP. El parámetro **e** permite SNMP. El parámetro **d.** deshabilita SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Especifica el índice SNMP, si SNMP está habilitado. Para obtener más información, vea Rfc 1759 en el [sitio Web del editor Rfc](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<nombreComunidad >      |                                                                                                                                                                                                                                                                                Especifica el nombre de comunidad SNMP, si SNMP está habilitado.                                                                                                                                                                                                                                                                                |
|       -2{e &#124; -d}        | Especifica si se habilitan colas de impresión dobles (también conocido como dobles) para los puertos TCP lpr. Colas de impresión dobles son necesarias porque TCP lpr debe incluir un recuento de bytes exacto en el archivo de control que se envía a la impresora, pero el protocolo no puede obtener el recuento de proveedor de impresión local. Por lo tanto, cuando se envía un archivo a un TCP de cola de impresión lpr, también se pone cola como un archivo temporal en el directorio system32. TCP lpr determina el tamaño del archivo temporal y envía el tamaño para el servidor que ejecuta LPD. El parámetro **e** permite colas de impresión dobles. El parámetro **d.** deshabilita las colas de impresión dobles. |
|              /?              |                                                                                                                                                                                                                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Comentarios
-   El **prnport** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo prnport o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).
-   El protocolo TCP sin formato es un protocolo más alto de rendimiento en Windows que el protocolo lpr.

## <a name="BKMK_examples"></a>Ejemplos
Para mostrar todos los puertos de impresión TCP/IP estándar en el servidor \\\Server1, escriba:
```
cscript prnport -l -s Server1
```
Para eliminar el puerto de impresión TCP/IP estándar en el servidor \\\Server1 que se conecta a una impresora de red en 10.2.3.4, tipo:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Para agregar un puerto de impresión TCP/IP estándar en el servidor \\\Server1 que se conecta a una impresora de red en 10.2.3.4 y usa el protocolo TCP sin formato en el puerto 9100, tipo:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Para habilitar SNMP, especifique el nombre de comunidad "public" y establecer el índice SNMP en 1 en una impresora de red en 10.2.3.4 compartidos por el servidor \\\Server1, escriba:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Para agregar un puerto de impresión TCP/IP estándar en el equipo local que se conecta a una impresora de red en 10.2.3.4 y obtener automáticamente la configuración del dispositivo de la impresora, escriba:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
