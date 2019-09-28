---
title: change logon
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c04eaffe366dce079aed53351589c1b5026954e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379643"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Habilita o deshabilita los inicios de sesión de las sesiones de cliente o muestra el estado de inicio de sesión actual.
Esta utilidad es útil para el mantenimiento del sistema.
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |     Parámetro      |                                                       Descripción                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /Query       |                             Muestra el estado de inicio de sesión actual, si está habilitado o deshabilitado.                              |
> |      /Enable       |                              Habilita los inicios de sesión desde las sesiones de cliente, pero no desde la consola.                              |
> |      /Disable      |  Deshabilita los inicios de sesión posteriores de las sesiones de cliente, pero no de la consola. No afecta a los usuarios que han iniciado sesión actualmente.   |
> |       /drain       |                 Deshabilita los inicios de sesión de las nuevas sesiones de cliente, pero permite las reconexiones a las sesiones existentes.                 |
> | /drainuntilrestart | Deshabilita los inicios de sesión de nuevas sesiones de cliente hasta que se reinicia el equipo, pero permite las reconexiones a las sesiones existentes. |
> |         /?         |                                           Muestra la ayuda en el símbolo del sistema.                                           |
> 
> ## <a name="remarks"></a>Comentarios
> - Solo los administradores pueden usar el comando **cambiar inicio de sesión** .
> - Los inicios de sesión se vuelven a habilitar al reiniciar el sistema. Si está conectado al servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto) desde una sesión de cliente y deshabilita los inicios de sesión, y cierra la sesión antes de volver a habilitar los inicios de sesión, no podrá volver a conectarse a la sesión. Para volver a habilitar los inicios de sesión desde las sesiones de cliente, inicie sesión en la consola de.
>   ## <a name="BKMK_examples"></a>Example
> - Para mostrar el estado de inicio de sesión actual, escriba:
>   ```
>   change logon /query
>   ```
> - Para habilitar los inicios de sesión desde las sesiones de cliente, escriba:
>   ```
>   change logon /enable
>   ```
> - Para deshabilitar los inicios de sesión de cliente, escriba:
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>Referencias adicionales
>   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   [cambiar](change.md)
>   [servicios de escritorio remoto &#40;referencia de comandos&#41; Terminal Services](remote-desktop-services-terminal-services-command-reference.md)
