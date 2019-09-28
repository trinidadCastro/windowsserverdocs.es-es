---
title: prnport
description: Obtenga información acerca de cómo crear, eliminar y enumerar puertos de impresora.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c9c162cef2a3ae2f3de1e891691572130ae68f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372554"
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
|              -l              |                                                                                                                                                                                                                                                             enumera todos los puertos de impresora TCP/IP estándar del equipo especificado con el parámetro **-s** .                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Muestra la configuración de un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           Configura los valores de puerto para un puerto de impresora TCP/IP estándar.                                                                                                                                                                                                                                                                           |
|        -r \<PortName >        |                                                                                                                                                                                                                                                                                Especifica el puerto al que está conectada la impresora.                                                                                                                                                                                                                                                                                 |
|       -s \<ServerName >       |                                                                                                                                                                                                                               Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                                                                                                                                |
| -u \<UserName >-w <Password> |                                                                                                              Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione.                                                                                                               |
|     -o {RAW &#124; LPR}      |                                                                                                                                                                                                              Especifica el protocolo que utiliza el puerto: TCP RAW o TCP LPR. Si usa TCP RAW, opcionalmente puede especificar el número de puerto mediante el parámetro **-n** . El número de puerto predeterminado es 9100.                                                                                                                                                                                                              |
|      -h \<Hostaddress >       |                                                                                                                                                                                                                                                                   Especifica (por dirección IP) la impresora para la que desea configurar el puerto.                                                                                                                                                                                                                                                                    |
|       -q \<QueueName >        |                                                                                                                                                                                                                                                                                     Especifica el nombre de la cola para un puerto TCP sin formato.                                                                                                                                                                                                                                                                                     |
|       -n \<PortNumber >       |                                                                                                                                                                                                                                                                    Especifica el número de puerto para un puerto TCP sin formato. El número de puerto predeterminado es 9100.                                                                                                                                                                                                                                                                    |
|        -m {e &#124; d}        |                                                                                                                                                                                                                                                       Especifica si SNMP está habilitado. El parámetro **e** habilita SNMP. El parámetro **d** deshabilita SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Especifica el índice SNMP, si SNMP está habilitado. Para obtener más información, vea RFC 1759 en el [sitio web del editor RFC](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<CommunityName >      |                                                                                                                                                                                                                                                                                Especifica el nombre de la comunidad SNMP, si SNMP está habilitado.                                                                                                                                                                                                                                                                                |
|       -2 {e &#124; -d}        | Especifica si las colas de impresión dobles (también conocidas como rebobinar) están habilitadas para los puertos TCP LPR. Las colas de impresión dobles son necesarias porque TCP LPR debe incluir un recuento exacto de bytes en el archivo de control que se envía a la impresora, pero el Protocolo no puede obtener el recuento del proveedor de impresión local. Por lo tanto, cuando un archivo se pone en cola en una cola de impresión lpr de TCP, también se pone en cola como un archivo temporal en el directorio system32. TCP LPR determina el tamaño del archivo temporal y envía el tamaño al servidor que ejecuta LPD. El parámetro **e** habilita las colas dobles. El parámetro **d** deshabilita las colas de impresión dobles. |
|              /?              |                                                                                                                                                                                                                                                                                         Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Comentarios
-   El comando **prnport** es un script de Visual Basic ubicado en el directorio%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnport o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).
-   El protocolo TCP RAW es un protocolo de mayor rendimiento en Windows que el protocolo LPR.

## <a name="BKMK_examples"></a>Example
Para mostrar todos los puertos de impresión TCP/IP estándar en el servidor \\ \ Servidor1, escriba:
```
cscript prnport -l -s Server1
```
Para eliminar el puerto de impresión TCP/IP estándar en el servidor \\ \ Servidor1 que se conecta a una impresora de red en 10.2.3.4, escriba:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Para agregar un puerto de impresión TCP/IP estándar en el servidor \\ \ Servidor1 que se conecta a una impresora de red en 10.2.3.4 y usa el protocolo TCP RAW en el puerto 9100, escriba:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Para habilitar SNMP, especifique el nombre de comunidad "público" y establezca el índice de SNMP en 1 en una impresora de red en 10.2.3.4 compartida por el servidor \\ \ Servidor1, escriba:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Para agregar un puerto de impresión TCP/IP estándar en el equipo local que se conecta a una impresora de red en 10.2.3.4 y obtener automáticamente la configuración del dispositivo de la impresora, escriba:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comando de impresión](print-command-reference.md)
