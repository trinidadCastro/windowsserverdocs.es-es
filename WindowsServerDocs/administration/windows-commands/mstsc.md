---
title: mstsc
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3355aa310bb9919b5218052878d94416f577381
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723829"
---
# <a name="mstsc"></a>mstsc

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

crea conexiones a los servidores de host de sesión de Escritorio remoto (host de sesión de escritorio remoto) o a otros equipos remotos, edita un archivo de configuración existente de Conexión a Escritorio remoto (. RDP) y migra los archivos de conexión heredados que se crearon con el administrador de conexiones de cliente a nuevos archivos de conexión. RDP.

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                         Descripción                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Especifica el nombre de un archivo. RDP para la conexión.                                    |
|  /v: <servidor\>[: <puerto\>] |                Especifica el equipo remoto y, opcionalmente, el número de puerto al que desea conectarse.                 |
|         /admin          |                                   Le conecta a una sesión para administrar el servidor.                                   |
|           /f            |                                    inicia Conexión a Escritorio remoto en modo de pantalla completa.                                    |
|       /w<Width>        |                                      Especifica el ancho de la ventana de Escritorio remoto.                                      |
|       /h<Height>       |                                     Especifica el alto de la ventana Conexión a Escritorio remoto.                                      |
|         /public         |                  Ejecuta Escritorio remoto en modo público. En modo público, las contraseñas y los mapas de bits no se almacenan en caché.                  |
|          /span          | Coincide con el ancho y el alto de Escritorio remoto con el escritorio virtual local, lo que abarca varios monitores si es necesario. |
| /Edit<Connection File> |                                         Abre el archivo. RDP especificado para su edición.                                          |
|        /migrate         |       Migra los archivos de conexión heredados que se crearon con el administrador de conexiones de cliente a los nuevos archivos de conexión. RDP.       |
|           /?            |                                            Muestra la ayuda en el símbolo del sistema.                                             |

## <a name="remarks"></a>Observaciones
-   Default. RDP se almacena para cada usuario como un archivo oculto en la carpeta de documentos del usuario. Los archivos. RDP creados por el usuario se guardan de forma predeterminada en la carpeta documentos del usuario, pero se pueden guardar en cualquier lugar.
-   Para abarcar varios monitores, los monitores deben usar la misma resolución y deben estar alineados horizontalmente (es decir, en paralelo). Actualmente no se admite la expansión de varios monitores verticalmente en el sistema cliente.

## <a name="examples"></a>Ejemplos
-   Para conectarse a una sesión en modo de pantalla completa, escriba:
    ```
    mstsc /f
    ```
-   Para abrir un archivo llamado FILENAME. RDP para su edición, escriba:
    ```
    mstsc /edit filename.rdp
    ```

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
