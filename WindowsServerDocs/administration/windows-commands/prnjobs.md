---
title: prnjobs
description: Obtenga información acerca de cómo administrar los trabajos de impresión desde la línea de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a40a8cbc3c8b13c99cc440b8de797898a5a6249b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722841"
---
# <a name="prnjobs"></a>prnjobs

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Pausa, reanuda, cancela y enumera los trabajos de impresión.

## <a name="syntax"></a>Sintaxis
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                                                                        Descripción                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -Z              |                                                                                                                                                                 pausa el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -M              |                                                                                                                                                                Reanuda el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -X              |                                                                                                                                                                Cancela el trabajo de impresión especificado con el parámetro **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        enumera todos los trabajos de impresión de una cola de impresión.                                                                                                                                                                         |
|       -s \<nombreDeServidor>       |                                                                                                                  Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                  |
|      -p \<nombredeimpresora>       |                                                                                                                                                           Especifica el nombre de la impresora que desea administrar. Necesario.                                                                                                                                                            |
|         -j \<JobID>          |                                                                                                                                                                Especifica (por número de identificador) el trabajo de impresión que desea cancelar.                                                                                                                                                                 |
| -u \<nombre de usuario>-w<Password> | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
|              /?              |                                                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                            |

## <a name="remarks"></a>Observaciones
-   El comando **prnjobs** es un script de Visual Basic ubicado en el directorio\\ <language> de printing_Admin_Scripts%WINDIR%\system32\. Para usar este comando, en una ventana del símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prnjobs o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos
Para pausar un trabajo de impresión con un ID. de trabajo 27 enviado al equipo remoto llamado ServidorRH para imprimirlo en la impresora denominada colorprinter, escriba:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para enumerar todos los trabajos de impresión actuales en la cola de la impresora local llamada colorprinter_2, escriba:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia de comandos de impresión](print-command-reference.md)
