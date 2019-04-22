---
title: change port
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 477761c35f08d5ec81adae80ba8f2fe9667786e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818646"
---
# <a name="change-port"></a>change port

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra o cambia las asignaciones de puerto COM para que sean compatibles con las aplicaciones MS-DOS.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
## <a name="syntax"></a>Sintaxis
```
change port [<PortX>=<PortY> | /d <PortX> | /query]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<PortX>=<PortY>|Asigna COM <*puertox*> a <*puertoy*>.|
|/d <PortX>|elimina la asignación para COM <*puertox*>.|
|/query|Muestra las asignaciones de puerto actual.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Admite la mayoría de las aplicaciones de MS-DOS solo COM1 a puertos serie COM4. El **cambiar puerto** comando asigna un puerto serie en un número de puerto diferente, lo que permite a las aplicaciones que no admiten la numeración alta COM puertos de acceso al puerto serie. la reasignación sólo funciona para la sesión actual y no se conservará si cierra la sesión desde una sesión y, a continuación, vuelva a iniciar sesión.
-   Use **cambiar puerto** sin ningún parámetro para mostrar los puertos COM disponibles y sus asignaciones actuales.
## <a name="BKMK_examples"></a>Ejemplos
-   Para asignar COM12 COM1 para su uso por una aplicación basada en MS-DOS, escriba:
    ```
    change port com12=com1
    ```
-   Para mostrar las asignaciones de puerto actual, escriba:
    ```
    change port /query
    ```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[cambiar](change.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
