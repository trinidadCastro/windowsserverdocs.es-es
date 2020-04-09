---
title: proceso de consulta
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ee42286691444c3a667801be3174514a81441c6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836968"
---
# <a name="query-process"></a>proceso de consulta

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre los procesos que se ejecutan en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
Puede usar este comando para averiguar qué programas ejecuta un usuario específico y también qué usuarios ejecutan un programa específico.
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Parámetros
> 
> |      Parámetro       |                                                                 Descripción                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    muestra los procesos de todas las sesiones.                                                     |
> |     <ProcessID>      |                                   Especifica el identificador numérico que identifica el proceso que desea consultar.                                   |
> |      <UserName>      |                                       Especifica el nombre del usuario cuyos procesos desea enumerar.                                       |
> |    <SessionName>     |                                     Especifica el nombre de la sesión cuyos procesos desea enumerar.                                      |
> |       /ID:<nn>       |                                      Especifica el identificador de la sesión cuyos procesos desea enumerar.                                       |
> |    <ProgramName>     |                     Especifica el nombre del programa cuyos procesos desea consultar. Se requiere la extensión. exe.                     |
> | /server:<ServerName> | Especifica el servidor host de sesión de escritorio remoto cuyos procesos desea enumerar. Si no se especifica, se usa el servidor en el que ha iniciado sesión actualmente. |
> |          /?          |                                                     Muestra la Ayuda en el símbolo del sistema.                                                     |
> 
> ## <a name="remarks"></a>Comentarios
> - Los administradores tienen acceso total a todas las funciones de **proceso de consulta** .
> - Si no especifica el*nombre de usuario*< >, <*nombresesión*>, **/ID:** <*nn*>, <*nombreprograma*> o **\\** * parámetros, el **proceso de consulta** solo muestra los procesos que pertenecen al usuario actual.
> - Si se especifica una sesión, debe identificar una sesión activa.
> - el **proceso de consulta** devuelve la siguiente información:
>   -   El usuario propietario del proceso.
>   -   La sesión que posee el proceso
>   -   Identificador de la sesión.
>   -   Nombre del proceso.
>   -   Identificador del proceso.
> - Cuando el **proceso de consulta** devuelve información, se muestra un signo mayor que (>) antes de cada proceso que pertenece a la sesión actual.
>   ## <a name="examples"></a><a name=BKMK_examples></a>Example
> - Para mostrar información sobre los procesos que están usando todas las sesiones, escriba:
>   ```
>   query process *
>   ```
> - Para mostrar información sobre los procesos utilizados por el ID. de sesión 2, escriba:
>   ```
>   query process /ID:2
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   referencia de comandos de la
>   de [consulta](query.md) [servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
