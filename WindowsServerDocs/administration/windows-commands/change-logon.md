---
title: change logon
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a5668618417ec315a452d911dbe15c02c30eeb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861146"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Habilita o deshabilita los inicios de sesión de sesiones de cliente o muestra el estado de inicio de sesión actual.
Esta utilidad es útil para el mantenimiento del sistema.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
## <a name="syntax"></a>Sintaxis
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/query|Muestra el estado de inicio de sesión actual, habilitado o deshabilitado.|
|/enable|Permite a los inicios de sesión desde las sesiones de cliente, pero no desde la consola.|
|/Disable|Deshabilita los inicios de sesión posteriores desde sesiones de cliente, pero no desde la consola. No afecta a los usuarios ha iniciado sesión actualmente.|
|/drain|Deshabilita los inicios de sesión de nuevas sesiones de cliente, pero permite las reconexiones a las sesiones existentes.|
|/drainuntilrestart|Deshabilita los inicios de sesión de nuevas sesiones de cliente hasta que se reinicie el equipo, pero permite las reconexiones a las sesiones existentes.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Solo los administradores pueden usar el **Cambiar inicio de sesión** comando.
-   Los inicios de sesión se vuelven a habilitar al reiniciar el sistema. Si están conectados al servidor Host de sesión de escritorio remoto (Host de sesión de rd) desde una sesión de cliente y deshabilitar los inicios de sesión y, a continuación, cierre la sesión antes de volver a habilitar los inicios de sesión, no podrá volver a conectarse a la sesión. Para volver a habilitar los inicios de sesión de sesiones de cliente, inicie sesión en la consola.
## <a name="BKMK_examples"></a>Ejemplos
-   Para mostrar el estado actual del inicio de sesión, escriba:
    ```
    change logon /query
    ```
-   Para habilitar los inicios de sesión de sesiones de cliente, escriba:
    ```
    change logon /enable
    ```
-   Para deshabilitar los inicios de sesión de cliente, escriba:
    ```
    change logon /disable
    ```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[cambiar](change.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
