---
title: taskkill
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13366307bbf685d1e4af8c0a58d5b9d6643111dc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811047"
---
# <a name="taskkill"></a>taskkill

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza uno o más procesos o tareas. Los procesos se pueden finalizar por el identificador del proceso o el nombre de la imagen. **taskkill** reemplaza el **kill** herramienta.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                                                                                        Descripción                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<equipo >       |                                                                                    Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                     |
| /u \<dominio >\\\<nombre de usuario > | Ejecuta el comando con los permisos de cuenta del usuario que se especifica mediante *UserName* o *dominio*\\*UserName*. **/u** se puede especificar sólo si **/s** se especifica. El valor predeterminado es los permisos del usuario que ha iniciado sesión actualmente en el equipo que está emitiendo el comando. |
|      /p \<contraseña >       |                                                                                                   Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                   |
|       /Fi \<filtro >       |          Se aplica un filtro para seleccionar un conjunto de tareas. Puede usar más de un filtro o use el carácter comodín ( **\\** \*) para especificar todas las tareas o los nombres de imágenes. Vea el siguiente [tabla para los nombres de filtro válido](#filter-names-operators-and-values), operadores y valores.           |
|     /PID \<ProcessID >     |                                                                                                                 Especifica el identificador de proceso del proceso que se finalice.                                                                                                                 |
|     /im \<ImageName>      |                                                                                Especifica el nombre de la imagen del proceso que se finalice. Use el carácter comodín ( **\\** \*) para especificar todos los nombres de imagen.                                                                                |
|            /f             |                                                                    Especifica que los procesos terminar forzosamente. Este parámetro se omite para los procesos remotos; fuerza se terminan todos los procesos remotos.                                                                     |
|            /t             |                                                                                                          Finaliza el proceso especificado y cualquier proceso secundario iniciado por ella.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Los nombres de filtro, operadores y valores

| Nombre de filtro |    Operadores válidos     |                                                                Valores válidos                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STatUS    |         eq, ne         |                                                 EJECUTANDO &AMP;#124; NO RESPONDE &AMP;#124; DESCONOCIDO                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nombre de la imagen                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valor de PID                                                                   |
|   SESIÓN   | eq, ne, gt, lt, ge, le |                                                                Número de sesión                                                                |
|   CpuTime   | eq, ne, gt, lt, ge, le | Tiempo de CPU en el formato <em>HH</em> **:** <em>MM</em> **:** <em>SS</em>, donde *MM* y *SS* están comprendidos entre 0 y 59 y *HH* es cualquiera sin signo de número |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Uso de memoria en KB                                                              |
|  NOMBRE DE USUARIO   |         eq, ne         |                                               Cualquier nombre de usuario válido (*usuario* o *dominio*\\*usuario*)                                               |
|  SERVICIOS   |         eq, ne         |                                                                 Nombre del servicio                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Título de ventana                                                                 |
|   MÓDULOS   |         eq, ne         |                                                                   Nombre de DLL                                                                   |

## <a name="remarks"></a>Comentarios
* No se admiten los filtros WINDOWTITLE y el estado cuando se especifica un sistema remoto.
* El carácter comodín ( **\\** <em>) se acepta para la * */im</em>*  opción solo cuando se aplica un filtro.
* Finalización de procesos remotos es siempre lleva a cabo de manera forzada, independientemente de si el **/f** se especifica la opción.
* Proporciona un nombre de equipo para el filtro de nombre de host produce un cierre y se detienen todos los procesos.
* Puede usar **tasklist** para determinar el identificador de proceso (PID) terminar el proceso.

## <a name="examples"></a>Ejemplos

Para finalizar los procesos con 1230 identificadores de proceso, 1241 y 1253, escriba:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Para terminar forzosamente el proceso "Notepad.exe" que se ha iniciado por el sistema, escriba:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Para finalizar todos los procesos en el equipo remoto "Srvmain" con una imagen nombre comienza por "Nota", mientras que con las credenciales de la cuenta de usuario Hiropln, escriba:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Para terminar el proceso con el proceso de Id. de 2134 y todos los procesos que se inició, pero solo si los procesos iniciados por la cuenta de administrador, escriba:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Para finalizar todos los procesos que tienen un identificador de proceso mayor o igual que 1000, independientemente de sus nombres de imagen, escriba:

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
