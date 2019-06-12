---
title: prnmngr
description: Obtenga información sobre cómo agregar, eliminar y enumerar las impresoras y conexiones.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f0e3af7b05b77400d3d8a04d048b34b8c553438d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436230"
---
# <a name="prnmngr"></a>prnmngr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega, elimina y enumera las impresoras o conexiones de impresora, además de establecer y mostrar la impresora predeterminada.

## <a name="syntax"></a>Sintaxis
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                                                                                                                                                                        Descripción                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             Agrega una conexión de impresora local.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               elimina una conexión de impresora.                                                                                                                                                                               |
|              -x               |                                                                                                               Elimina todas las impresoras desde el servidor especificado con el **-s** parámetro. Si no especifica un servidor, Windows eliminan todas las impresoras en el equipo local.                                                                                                               |
|              -g               |                                                                                                                                                                               Muestra la impresora predeterminada.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Establece la impresora predeterminada en la impresora especificada por el **-p** parámetro.                                                                                                                                                         |
|              -l               |                                                                                                         Enumera todas las impresoras instaladas en el servidor especificado por el **-s** parámetro. Si no especifica un servidor, Windows enumeran las impresoras instaladas en el equipo local.                                                                                                         |
|               c               |                                                                                                                                      Especifica que el parámetro se aplica a las conexiones de impresora. Se puede usar con el **- a** y **- x** parámetros.                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                  |
|       -p \<nombreImpresora >       |                                                                                                                                                                Especifica el nombre de la impresora que desea administrar.                                                                                                                                                                 |
|     -m \<DrivermodelName>     |                                                                                                          Especifica el controlador que desea instalar (por nombre). A menudo se denominan controladores para el modelo de impresora compatible. Consulte la documentación de la impresora para obtener más información.                                                                                                           |
|        -r \<PortName>         |                                                                         Especifica el puerto que está conectada la impresora. Si se trata de un paralelo o un puerto serie, use el identificador del puerto (por ejemplo, LPT1: o COM1:). Si se trata de un puerto TCP/IP, utilice el nombre del puerto que especificó cuando agregó el puerto.                                                                          |
| -u \<UserName > -w \<contraseña > | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando. |
|              /?               |                                                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                            |

## <a name="remarks"></a>Comentarios
-   El **prndrvr** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa a la **prnmngr** de archivos, o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Ejemplos
Para agregar una impresora llamada ImpresoraColor_2 que está conectado al puerto LPT1 en el equipo local y requiere un controlador de impresora llamado Driver1 de impresora de color, escriba:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Para eliminar la impresora llamada ImpresoraColor_2 desde el equipo remoto ServidorRH, escriba:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
