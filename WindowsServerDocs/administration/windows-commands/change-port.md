---
title: change port
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e587572acd1af1cc7dbd2550e1eae5244d0d1dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379596"
---
# <a name="change-port"></a>change port

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |    Parámetro    |              Descripción               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    Asigna*COM < no*está > para < > de*portabilidad*.    |
> |   /d <PortX>    | elimina la asignación para*COM < >* . |
> |     /Query      |  Muestra las asignaciones de puerto actuales.   |
> |       /?        |  Muestra la ayuda en el símbolo del sistema.  |
> 
> ## <a name="remarks"></a>Comentarios
> - La mayoría de las aplicaciones de MS-DOS solo admiten puertos serie de COM1 a COM4. El comando **cambiar puerto** asigna un puerto serie a un número de puerto diferente, lo que permite que las aplicaciones que no admiten puertos com de número alto tengan acceso al puerto serie. la reasignación solo funciona para la sesión actual y no se conserva si cierra sesión en una sesión y vuelve a iniciarla.
> - Use **cambiar puerto** sin parámetros para mostrar los puertos com disponibles y sus asignaciones actuales.
>   ## <a name="BKMK_examples"></a>Example
> - Para asignar COM12 a COM1 para su uso por parte de una aplicación basada en MS-dos, escriba:
>   ```
>   change port com12=com1
>   ```
> - Para mostrar las asignaciones de puertos actuales, escriba:
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>Referencias adicionales
>   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   [cambiar](change.md)
>   [servicios de escritorio remoto &#40;referencia de comandos&#41; Terminal Services](remote-desktop-services-terminal-services-command-reference.md)
