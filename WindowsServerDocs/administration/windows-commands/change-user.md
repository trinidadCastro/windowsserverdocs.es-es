---
title: change user
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d32c27e4b4c91b553efe3b55ab38181f51ed4d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817496"
---
# <a name="change-user"></a>change user

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el modo de instalación para el servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
## <a name="syntax"></a>Sintaxis
```
change user {/execute | /install | /query}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/execute|Permite la asignación de archivo ini en el directorio principal. Esta es la configuración predeterminada.|
|/install|Deshabilita la asignación del archivo .ini en el directorio principal. Todos los archivos .ini se leen y escritos en el directorio del sistema. Debe deshabilitar la asignación del archivo .ini al instalar aplicaciones en un servidor Host de sesión de escritorio remoto.|
|/query|Muestra la configuración actual de la asignación del archivo. ini.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Use **Cambiar usuario /install** antes de instalar una aplicación para crear archivos .ini para la aplicación en el directorio del sistema. Estos archivos se usan como el origen cuando se crean los archivos .ini específicos del usuario. Después de instalar la aplicación, use **Cambiar usuario / execute** para revertir a la asignación del archivo .ini estándar.
-   La primera vez que ejecute la aplicación, lo busca en el directorio principal de sus archivos. ini. Si los archivos .ini no se encuentran en el directorio principal, pero se encuentran en el directorio del sistema, servicios de escritorio remoto copia los archivos .ini en el directorio particular, lo que garantiza que cada usuario tiene una copia única de los archivos .ini de aplicación. Los archivos .ini nuevos se crean en el directorio principal.
-   Cada usuario debe tener una copia única de los archivos .ini para una aplicación. Esto evita que los casos en que distintos usuarios podrían tener configuraciones de aplicación incompatible (por ejemplo, los directorios predeterminados diferentes o resoluciones de pantalla).
-   Cuando el sistema está en modo de instalación (es decir, **Cambiar usuario /install**), se producen varias cosas. Todas las entradas de registro que se crean están sombreadas en **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentversion\Terminal Server\Install**, en uno el **\SOFTWARE** subclave o la **\MACHINE** subclave. Las subclaves se agregan a **HKEY_CURrenT_USER** se copian en el **\SOFTWARE** de subclave y las subclaves agregado a **HKEY_LOCAL_MACHINE** se copian en el **\ MÁQUINA** subclave. Si la aplicación consulta el directorio de Windows mediante el uso de las llamadas del sistema, como GetWindowsdirectory, el servidor Host de sesión de escritorio remoto devuelve el directorio systemroot. Si se agregan las entradas del archivo .ini mediante llamadas al sistema, como WritePrivateProfileString, se agregan a los archivos en el directorio raíz.
-   Cuando el sistema vuelve al modo de ejecución (es decir, **Cambiar usuario / execute**), y la aplicación intenta leer una entrada del registro en **HKEY_CURrenT_USER** que no existe, servicios de escritorio remoto comprueba si existe una copia de la clave bajo la **\Terminal Server\Install** subclave. Si es así, las subclaves se copian en la ubicación adecuada en **HKEY_CURrenT_USER**. Si la aplicación intenta leer un archivo .ini que no existe, busca en servicios de escritorio remoto para ese archivo. ini, bajo la raíz del sistema. Si el archivo .ini está en la raíz del sistema, se copia en el subdirectorio \Windows del directorio principal del usuario. Si la aplicación consulta el directorio de Windows, el servidor Host de sesión de escritorio remoto devuelve el subdirectorio \Windows del directorio principal del usuario.
-   Cuando haya iniciado sesión, servicios de escritorio remoto comprueba si sus archivos .ini del sistema son más recientes que los archivos .ini en el equipo. Si la versión del sistema es más reciente, el archivo .ini se reemplaza o se combinan con la versión más reciente. Esto depende de si el bit INISYNC, 0 x 40, es para este archivo. ini. Se cambia el nombre de la versión anterior del archivo .ini IniFile.ctx. Si los valores del registro del sistema en el **\Terminal Server\Install** son más recientes que la versión de la subclave **HKEY_CURrenT_USER**, su versión de las subclaves se elimina y se reemplazan por las nuevas subclaves desde **\Terminal Server\Install**.
## <a name="BKMK_examples"></a>Ejemplos
-   Para deshabilitar la asignación del archivo .ini del directorio principal, escriba:
    ```
    change user /install
    ```
-   Para habilitar la asignación del archivo .ini del directorio principal, escriba:
    ```
    change user /execute
    ```
-   Para mostrar la configuración actual de la asignación del archivo. ini, escriba:
    ```
    change user /query
    ```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[cambiar](change.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
