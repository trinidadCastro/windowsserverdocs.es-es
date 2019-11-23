---
title: prnjobs
description: Obtenga información acerca de cómo administrar los trabajos de impresión desde la línea de comandos.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c4fb9be9545274bbbf33926042f7a4deec5ceb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372099"
---
# <a name="prnjobs"></a>prnjobs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pausa, reanuda, cancela y enumera los trabajos de impresión.

## <a name="syntax"></a>Sintaxis
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                                                                        Descripción                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 pausa el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -m              |                                                                                                                                                                Reanuda el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -x              |                                                                                                                                                                Cancela el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        enumera todos los trabajos de impresión de una cola de impresión.                                                                                                                                                                         |
|       -s \<ServerName >       |                                                                                                                  Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                  |
|      -p \<Nombredeimpresora >       |                                                                                                                                                           Especifica el nombre de la impresora que desea administrar. Obligatorio.                                                                                                                                                            |
|         -j \<JobID >          |                                                                                                                                                                Especifica (por número de identificador) el trabajo de impresión que desea cancelar.                                                                                                                                                                 |
| -u \<nombreDeUsuario >-w <Password> | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
|              /?              |                                                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                            |

## <a name="remarks"></a>Observaciones
-   El comando **prnjobs** es un script de Visual Basic ubicado en el printing_Admin_Scripts%windir%\system32\\\<language> directorio. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnjobs o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Example
Para pausar un trabajo de impresión con un ID. de trabajo 27 enviado al equipo remoto llamado ServidorRH para imprimirlo en la impresora denominada colorprinter, escriba:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para enumerar todos los trabajos de impresión actuales en la cola de la impresora local llamada colorprinter_2, escriba:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia de comandos de impresión](print-command-reference.md)
