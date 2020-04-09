---
title: prnmngr
description: Obtenga información acerca de cómo agregar, eliminar y enumerar impresoras y conexiones.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 621bd6ef68b4243fc010c5c704c286a22028cd6e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837238"
---
# <a name="prnmngr"></a>prnmngr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega, elimina y enumera impresoras o conexiones de impresora, además de establecer y mostrar la impresora predeterminada.

## <a name="syntax"></a>Sintaxis
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

### <a name="parameters"></a>Parámetros

|           Parámetro           |                                                                                                                                                                                        Descripción                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             agrega una conexión de impresora local.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               elimina una conexión de impresora.                                                                                                                                                                               |
|              -x               |                                                                                                               elimina todas las impresoras del servidor especificado con el parámetro **-s** . Si no especifica un servidor, Windows elimina todas las impresoras en el equipo local.                                                                                                               |
|              -g               |                                                                                                                                                                               Muestra la impresora predeterminada.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Establece la impresora predeterminada en la impresora especificada por el parámetro **-p** .                                                                                                                                                         |
|              -l               |                                                                                                         muestra todas las impresoras instaladas en el servidor especificado por el parámetro **-s** . Si no especifica un servidor, Windows enumera las impresoras instaladas en el equipo local.                                                                                                         |
|               c               |                                                                                                                                      Especifica que el parámetro se aplica a las conexiones de impresora. Se puede usar con los parámetros **-a** y **-x** .                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                  |
|       -p \<Nombredeimpresora >       |                                                                                                                                                                Especifica el nombre de la impresora que desea administrar.                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          Especifica (por nombre) el controlador que desea instalar. Los controladores a menudo se denominan para el modelo de impresora que admiten. Consulte la documentación de la impresora para obtener más información.                                                                                                           |
|        -r \<PortName >         |                                                                         Especifica el puerto al que está conectada la impresora. Si se trata de un puerto paralelo o serie, use el identificador del puerto (por ejemplo, LPT1: o COM1:). Si se trata de un puerto TCP/IP, utilice el nombre de puerto que se especificó cuando se agregó el puerto.                                                                          |
| -u \<nombreDeUsuario >-w \<contraseña > | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
|              /?               |                                                                                                                                                                           Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                            |

## <a name="remarks"></a>Comentarios
-   El comando **prndrvr** es un script de Visual Basic ubicado en el printing_Admin_Scripts%windir%\system32\\\<language> directorio. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo **PRNMNGR** o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="examples"></a><a name="BKMK_examples"></a>Example
Para agregar una impresora llamada colorprinter_2 conectada a LPT1 en el equipo local y requiere un controlador de impresora llamado color Printer Driver1, escriba:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Para eliminar la impresora denominada colorprinter_2 del equipo remoto llamado ServidorRH, escriba:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[Referencia del comando Print](print-command-reference.md)
