---
title: mstsc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6f89c1e3b0d36f14dbd55f9e6994c788305b30d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437187"
---
# <a name="mstsc"></a>mstsc

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea las conexiones a servidores Host de sesión de escritorio remoto (Host de sesión de rd) o en otros equipos remotos, modifica un archivo de configuración existente de conexión a Escritorio remoto (.rdp) y migra los archivos de conexión antiguos que se crearon con el Administrador de conexiones de cliente a los nuevos archivos de conexión RDP.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parámetros

|        Parámetro        |                                                         Descripción                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Especifica el nombre de un archivo .rdp para la conexión.                                    |
|   / v: < servidor [:<Port>]   |                Especifica el equipo remoto y, opcionalmente, el número de puerto al que desea conectarse.                 |
|         /Admin          |                                   Se conecta a una sesión para administrar el servidor.                                   |
|           /f            |                                    Inicia conexión a Escritorio remoto en modo de pantalla completa.                                    |
|       /w:<Width>        |                                      Especifica el ancho de la ventana de escritorio remoto.                                      |
|       /h:<Height>       |                                     Especifica el alto de la ventana de escritorio remoto.                                      |
|         / pública         |                  Escritorio remoto se ejecuta en el modo público. En el modo público, los mapas de bits y las contraseñas no se almacenan en caché.                  |
|          /span          | Coincide con el ancho de escritorio remoto y el alto con el escritorio virtual local, expandiéndose entre varios monitores si fuera necesario. |
| /Edit <Connection File> |                                         Abre el archivo .rdp especificado para su edición.                                          |
|        / migrar         |       Migra los archivos de conexión antiguos que se crearon con el Administrador de conexiones de cliente a nuevos archivos de conexión RDP.       |
|           /?            |                                            Muestra la ayuda en el símbolo del sistema.                                             |

## <a name="remarks"></a>Comentarios
-   Default.rdp se almacena para cada usuario como un archivo oculto en la carpeta documentos del usuario. Crea archivos .rdp de usuario se guardan de forma predeterminada en la carpeta documentos del usuario, pero se pueden guardar en cualquier lugar.
-   Para abarcar varios monitores, los monitores, debe usar la misma resolución y deben estar alineados horizontalmente (es decir, en paralelo). Actualmente no hay ninguna compatibilidad para abarcar a varios monitores verticalmente en el sistema cliente.

## <a name="BKMK_examples"></a>Ejemplos
-   Para conectarse a una sesión en modo de pantalla completa, escriba:
    ```
    mstsc /f
    ```
-   Para abrir un archivo denominado filename.rdp para modificarlo, escriba:
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
