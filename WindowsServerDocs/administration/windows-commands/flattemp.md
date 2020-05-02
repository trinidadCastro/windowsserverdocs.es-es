---
title: flattemp
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0969209044784f87c917d90af257c5b3b523af16
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720103"
---
# <a name="flattemp"></a>flattemp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita o deshabilita las carpetas temporales sin formato.


> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Query|Consulta la configuración actual.|
|/Enable|Habilita las carpetas temporales sin formato. Los usuarios compartirán la carpeta temporal a menos que la carpeta temporal resida en la carpeta principal del usuario.|
|/Disable|Deshabilita las carpetas temporales sin formato. Cada carpeta temporal de usuarios residirá en una carpeta independiente (determinada por el ID. de sesión del usuario).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   El comando **flattemp** solo está disponible cuando se ha instalado el servicio de rol Terminal Server en un equipo que ejecuta windows Server 2008 o el servicio de rol host de sesión de escritorio remoto en un equipo que ejecuta windows Server 2008 R2.
-   Debe tener credenciales administrativas para ejecutar **flattemp**.
-   Después de que cada usuario tenga una carpeta temporal única, use **flattemp/enable** para habilitar las carpetas temporales sin formato.
-   El método predeterminado para crear carpetas temporales para varios usuarios (que normalmente apuntan por las variables de entorno TEMP y TMP) es crear subcarpetas en la carpeta **\temp** mediante logonID como el nombre de la subcarpeta. Por ejemplo, si la variable de entorno TEMP apunta a C:\Temp, la carpeta temporal asignada al usuario logonID 4 es C:\Temp\4. Con **flattemp**, puede apuntar directamente a la carpeta \temp e impedir que se formen las subcarpetas. Esto resulta útil cuando desea que las carpetas temporales del usuario estén contenidas en carpetas particulares, ya sea en una unidad local del servidor host de sesión de escritorio remoto o en una unidad de red compartida. Debe usar el comando **flattemp/enable** solo cuando cada usuario tenga una carpeta temporal independiente.
-   Es posible que se produzcan errores de aplicación si la carpeta temporal del usuario se encuentra en una unidad de red. Esto sucede cuando la unidad de red compartida deja de estar accesible momentáneamente en la red. Dado que los archivos temporales de la aplicación son inaccesibles o no están sincronizados, responde como si el disco se haya detenido. No se recomienda mover la carpeta temporal a una unidad de red. El valor predeterminado es conservar las carpetas temporales en el disco duro local. Si experimenta un comportamiento inesperado o errores de daños en disco con ciertas aplicaciones, estabilice la red o vuelva a colocar las carpetas temporales en el disco duro local.
-   Si deshabilita el uso de carpetas temporales independientes por sesión, se omite la configuración de **flattemp** . Esta opción se establece en la herramienta de configuración de Servicios de Escritorio remoto.

## <a name="examples"></a>Ejemplos
-   Para mostrar la configuración actual de las carpetas temporales sin formato, escriba:
    ```
    flattemp /query
    ```
-   Para habilitar las carpetas temporales sin formato, escriba:
    ```
    flattemp /enable
    ```
-   Para deshabilitar las carpetas temporales sin formato, escriba:
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
