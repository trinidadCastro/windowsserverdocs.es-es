---
title: tskill
description: Tema de referencia de tskill, que finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión de Escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13bd18a84dccbbeee88c24b9b07208b3174bc558
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721239"
---
# <a name="tskill"></a>tskill

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión Escritorio remoto.


> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<ProcessID>|Especifica el identificador del proceso que desea finalizar.|
|\<ProcessName>|Especifica el nombre del proceso que desea finalizar. Este parámetro puede incluir caracteres comodín.|
|/Server:\<ServerName>|Especifica el servidor de Terminal Server que contiene el proceso que desea finalizar. Si no se especifica **/Server** , se usa el servidor host de sesión de escritorio remoto actual.|
|/ID:\<SessionID>|Finaliza el proceso que se está ejecutando en la sesión especificada.|
|/a|Finaliza el proceso que se está ejecutando en todas las sesiones.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
- Puede usar **tskill** para finalizar solo los procesos que le pertenecen, a menos que sea administrador. Los administradores tienen acceso total a todas las funciones de **tskill** y pueden finalizar los procesos que se ejecutan en otras sesiones de usuario.
- Si se terminan todos los procesos ejecutados en una sesión, la sesión también termina.
- Si usa los parámetros *processName* y **/Server:**<em>ServerName</em> , también debe especificar el parámetro **/ID:**<em>SessionID</em> o **/a** .

## <a name="examples"></a>Ejemplos
- Para finalizar el proceso 6543, escriba:
  ```
  tskill 6543
  ```
- Para finalizar el explorador de procesos que se ejecuta en la sesión 5, escriba:
  ```
  tskill explorer /id:5
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Referencia de comandos de servicios de escritorio remoto de la clave](command-line-syntax-key.md)
  de sintaxis de línea de comandos[(Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
