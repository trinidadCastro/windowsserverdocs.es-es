---
title: taskkill
description: 'Tema de comandos de Windows para * * * *- '
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
ms.openlocfilehash: 9050788a06b66e54be59c69dd8cc837092123b00
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878133"
---
# <a name="taskkill"></a>taskkill

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza uno o más procesos o tareas. Los procesos se pueden finalizar por el identificador del proceso o el nombre de la imagen. **Taskkill** reemplaza la herramienta **Kill** .
Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                                                                                        Descripción                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<equipo >       |                                                                                    Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                     |
| /u \<dominio >\\nombreDeUsuario>\< | Ejecuta el comando con los permisos de cuenta del usuario que se especifica mediante *el nombre de usuario o el* *dominio*\\*nombreDeUsuario*. **/u** solo se puede especificar si se especifica **/s** . El valor predeterminado son los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. |
|      /p \<contraseña >       |                                                                                                   Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                   |
|       /fi \<> de filtro       |          Aplica un filtro para seleccionar un conjunto de tareas. Puede usar más de un filtro o usar el carácter comodín ( **\\** \*) para especificar todas las tareas o los nombres de las imágenes. Vea la tabla siguiente para ver los nombres de filtro, los operadores y los valores [válidos](#filter-names-operators-and-values).           |
|     /PID \<ProcessId >     |                                                                                                                 Especifica el identificador de proceso del proceso que se va a terminar.                                                                                                                 |
|     /im \<ImageName >      |                                                                                Especifica el nombre de la imagen del proceso que se va a terminar. Use el carácter comodín ( **\\** \*) para especificar todos los nombres de imagen.                                                                                |
|            /f             |                                                                    Especifica que los procesos se terminan forzosamente. Este parámetro se omite para los procesos remotos; todos los procesos remotos se terminan forzosamente.                                                                     |
|            /t             |                                                                                                          Finaliza el proceso especificado y los procesos secundarios iniciados por él.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrar nombres, operadores y valores

| Nombre del filtro |    Operadores válidos     |                                                                Valores válidos                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   ESTATUS    |         eq, ne         |                                                 EJECUCIÓN &#124; NO RESPONDE &#124; DESCONOCIDA                                                 |
|  /IM  |         eq, ne         |                                                                  Nombre de la imagen                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valor de PID                                                                   |
|   SESIÓN   | eq, ne, gt, lt, ge, le |                                                                Número de sesión                                                                |
|   CpuTime   | eq, ne, gt, lt, ge, le | Tiempo de CPU con el formato <em>HH</em> **:** <em>mm</em> **:** <em>SS</em>, donde *mm* y *SS* están entre 0 y 59 y *HH* es cualquier número sin signo. |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Uso de memoria en KB                                                              |
|  NOMBRE   |         eq, ne         |                                               Cualquier nombre de usuario válido *(usuario o* *usuario*de *dominio*\\)                                               |
|  SERVER   |         eq, ne         |                                                                 Nombre del servicio                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Título de ventana                                                                 |
|   ADICIONALES   |         eq, ne         |                                                                   Nombre de DLL                                                                   |

## <a name="remarks"></a>Comentarios
* Los filtros WINDOWTITLE y STATUs no se admiten cuando se especifica un sistema remoto.
* El carácter comodín ( **\\** <em>) se acepta para la opción * */im</em>*  solo cuando se aplica un filtro.
* La terminación de los procesos remotos siempre se lleva a cabo forzosamente, independientemente de si se especifica la opción **/f** .
* El suministro de un nombre de equipo al filtro hostname provoca un cierre y se detienen todos los procesos.
* Puede usar **TaskList** para determinar el identificador de proceso (PID) del proceso que se va a finalizar.

## <a name="examples"></a>Ejemplos

Para finalizar los procesos con los identificadores de proceso 1230, 1241 y 1253, escriba:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Para forzar la finalización del proceso "Notepad. exe" si lo inició el sistema, escriba:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Para finalizar todos los procesos del equipo remoto "Srvmain" con un nombre de imagen que empiece por "Note", mientras usa las credenciales de la cuenta de usuario Hiropln, escriba:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Para finalizar el proceso con el identificador de proceso 2134 y los procesos secundarios que inició, pero solo si los procesos se iniciaron con la cuenta de administrador, escribe:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Para finalizar todos los procesos que tienen un identificador de proceso mayor o igual que 1000, independientemente de sus nombres de imagen, escriba:

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
