---
title: proceso de consulta
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e15a41fc931d2cf04d60759a63e3b80392265175
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840726"
---
# <a name="query-process"></a>proceso de consulta

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de los procesos que se ejecutan en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Puede usar este comando para averiguar qué programas se está ejecutando un usuario específico, y también los usuarios que ejecutan un programa específico.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
## <a name="syntax"></a>Sintaxis
```
query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|*|Enumera los procesos de todas las sesiones.|
|<ProcessID>|Especifica el identificador numérico que identifica el proceso que desea consultar.|
|<UserName>|Especifica el nombre del usuario cuyos procesos que desea enumerar.|
|<SessionName>|Especifica el nombre de la sesión cuyos procesos que desea enumerar.|
|/ Id:<nn>|Especifica el identificador de la sesión cuyos procesos que desea enumerar.|
|<ProgramName>|Especifica el nombre del programa cuyos procesos que desea consultar. La extensión .exe es necesaria.|
|/server:<ServerName>|Especifica el servidor Host de sesión de escritorio remoto cuyos procesos que desea enumerar. Si no se especifica, se utiliza el servidor donde han iniciado sesión.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Los administradores tienen acceso total a todos **el proceso de consulta** funciones.
-   Si no especifica el <*UserName*>, <*SessionName*>, **/ID:**<*nn*>, <*NombrePrograma*>, o **\*** parámetros, **el proceso de consulta** sólo mostrará los procesos que pertenecen al usuario actual.
-   Si se especifica una sesión, debe identificar una sesión activa.
-   **el proceso de consulta** devuelve la siguiente información:
    -   El usuario propietario del proceso
    -   La sesión propietaria del proceso
    -   El identificador de la sesión
    -   El nombre del proceso
    -   El identificador del proceso
-   Cuando **el proceso de consulta** devuelve información, un signo mayor que (>) se muestra antes de cada proceso al que pertenece a la sesión actual.
## <a name="BKMK_examples"></a>Ejemplos
-   Para mostrar información sobre los procesos que se están usando todas las sesiones, escriba:
    ```
    query process *
    ```
-   Para mostrar información sobre los procesos que se están usando 2 Id. de sesión, escriba:
    ```
    query process /ID:2
    ```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[consulta](query.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
