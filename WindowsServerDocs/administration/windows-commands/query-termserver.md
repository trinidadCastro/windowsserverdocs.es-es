---
title: consulta termserver
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14270092949baf37588059d592e6f92e694a1739
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442092"
---
# <a name="query-termserver"></a>consulta termserver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de todos los servidores Host de sesión de escritorio remoto (Host de sesión de rd) en la red.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |    Parámetro     |                                                                        Descripción                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Especifica el nombre que identifica el servidor Host de sesión de escritorio remoto.                                               |
> | / Domain:<Domain> | Especifica el dominio para consultar los servidores de terminal Server. No es necesario especificar un dominio si está consultando el dominio en el que está trabajando actualmente. |
> |     / Address     |                                                  Muestra las direcciones de red y de nodo para cada servidor.                                                  |
> |    / continue     |                                              Impide las pausas después de cada pantalla de información.                                               |
> |        /?        |                                                            Muestra la ayuda en el símbolo del sistema.                                                            |
> 
> ## <a name="remarks"></a>Comentarios
> - **consultar termserver** busca en la red para todas las adjunta los servidores Host de sesión de escritorio remoto y devuelve la información siguiente:
>   - El nombre del servidor
>   - La red (y la dirección del nodo si se utiliza la opción/Address)
>     ## <a name="BKMK_examples"></a>Ejemplos
> - Para mostrar información acerca de todos los servidores Host de sesión de escritorio remoto en la red, escriba:
>   ```
>   query termserver
>   ```
> - Para mostrar información acerca del servidor de Host de sesión de escritorio remoto denominado Server3, escriba:
>   ```
>   query termserver Server3
>   ```
> - Para mostrar información acerca de todos los servidores Host de sesión de escritorio remoto en el dominio CONTOSO, escriba:
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Para mostrar la dirección de red y de nodo para el servidor de Host de sesión de escritorio remoto llamado Server3, escriba:
>   ```
>   query termserver Server3 /address
>   ```
>   #### <a name="additional-references"></a>Referencias adicionales
>   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   [consulta](query.md)
>   [servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
