---
title: flattemp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fc14a6fe1a355f7c20c130fba3fb1f17e49b6f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872936"
---
# <a name="flattemp"></a>flattemp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita o deshabilita las carpetas temporales lineales.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/query|Consulta la configuración actual.|
|/enable|Habilita las carpetas temporales lineales. Los usuarios compartirán la carpeta temporal a menos que se encuentra la carpeta temporal en la carpeta principal de usuario s.|
|/Disable|Deshabilita las carpetas temporales lineales. Cada carpeta temporal de s usuario residen en una carpeta independiente (determinada por el usuario s Id. de sesión).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   El **flattemp** comando solo está disponible cuando se ha instalado el servicio de rol Terminal Server en un equipo que ejecuta Windows Server 2008 o el servicio de rol Host de sesión de escritorio remoto en un equipo que ejecuta Windows Server 2008 R2.
-   Debe tener credenciales administrativas para ejecutar **flattemp**.
-   Después de cada usuario tiene una carpeta temporal única, use **flattemp /enable** para habilitar las carpetas temporales lineales.
-   El método predeterminado para crear carpetas temporales para varios usuarios (normalmente indicados por las variables de entorno TEMP y TMP) consiste en crear subcarpetas en la **\Temp** carpeta utilizando el ID de registro como el nombre de subcarpeta. Por ejemplo, si la variable de entorno TEMP señala a C:\Temp, la carpeta temporal asignada para el inicio de sesión de usuario 4 es C:\Temp\4. Uso de **flattemp**, puede señalar directamente a la carpeta \Temp y evitar que las subcarpetas que forman. Esto es útil cuando desee que las carpetas temporales del usuario para poder estar contenidos en las carpetas particulares, ya sea en una unidad local del servidor de Host de sesión de escritorio remoto o en una unidad de red compartida. Debe usar el **flattemp /enable** comando solo cuando cada usuario tiene una carpeta temporal independiente.
-   Pueden producirse errores de aplicación si la carpeta temporal del usuario se encuentra en una unidad de red. Esto se produce cuando la unidad de red compartida deja de estar accesible momentáneamente en la red. Dado que los archivos temporales de la aplicación son inaccesibles o fuera de sincronización, responde como si se ha detenido el disco. No se recomienda mover la carpeta temporal a una unidad de red. El valor predeterminado es mantener las carpetas temporales en el disco duro local. Si experimenta un comportamiento inesperado o errores de daños en el disco con determinadas aplicaciones, estabilizar la red o mueva las carpetas temporales en el disco duro local.
-   Si deshabilita el uso independiente de las carpetas temporales por sesión, **flattemp** se omite la configuración. Esta opción se establece en la herramienta de configuración de servicios de escritorio remoto.

## <a name="BKMK_examples"></a>Ejemplos
-   Para mostrar la configuración actual de las carpetas temporales lineales, escriba:
    ```
    flattemp /query
    ```
-   Para habilitar las carpetas temporales lineales, escriba:
    ```
    flattemp /enable
    ```
-   Para deshabilitar las carpetas temporales lineales, escriba:
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
