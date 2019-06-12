---
title: prnjobs
description: Obtenga información sobre cómo administrar los trabajos de impresión desde la línea de comandos.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5e9e71a21acac73aa27e8a936360c6a1e9f9b754
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436166"
---
# <a name="prnjobs"></a>prnjobs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

pone en pausa, se reanuda, se cancela y enumera los trabajos de impresión.

## <a name="syntax"></a>Sintaxis
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                                                                        Descripción                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 pone en pausa el trabajo de impresión especificado con el **-j** parámetro.                                                                                                                                                                 |
|              -m              |                                                                                                                                                                Reanuda el trabajo de impresión especificado con el **-j** parámetro.                                                                                                                                                                 |
|              -x              |                                                                                                                                                                Cancela el trabajo de impresión especificado con el **-j** parámetro.                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        Enumera todos los trabajos de impresión en una cola de impresión.                                                                                                                                                                         |
|       -s \<ServerName >       |                                                                                                                  Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.                                                                                                                  |
|      -p \<nombreImpresora >       |                                                                                                                                                           Especifica el nombre de la impresora que desea administrar. Obligatorio.                                                                                                                                                            |
|         -j \<JobID >          |                                                                                                                                                                Especifica que desea cancelar el trabajo de impresión (por número de identificación).                                                                                                                                                                 |
| -u \<UserName > -w <Password> | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando. |
|              /?              |                                                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                            |

## <a name="remarks"></a>Comentarios
-   El **prnjobs** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo prnjobs o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Ejemplos
Para pausar un trabajo de impresión con un Id. de trabajo de 27 enviado al equipo remoto para la impresión en la impresora mencionada ImpresoraColor ServidorRH, escriba:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para obtener una lista de todos los trabajos de impresión actuales en la cola de la impresora local llamada ImpresoraColor_2, escriba:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia de comandos de impresión](print-command-reference.md)
