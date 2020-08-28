---
title: tskill
description: Artículo de referencia de tskill, que finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión Escritorio remoto.
ms.topic: reference
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 357523ce9806910bfddc8ed8992a7ac7be388d3f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026813"
---
# <a name="tskill"></a>tskill

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión Escritorio remoto.


> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<ProcessID>|Especifica el identificador del proceso que desea finalizar.|
|\<ProcessName>|Especifica el nombre del proceso que desea finalizar. Este parámetro puede incluir caracteres comodín.|
|/server:\<ServerName>|Especifica el servidor de Terminal Server que contiene el proceso que desea finalizar. Si no se especifica **/Server** , se usa el servidor host de sesión de escritorio remoto actual.|
|/ID\<SessionID>|Finaliza el proceso que se está ejecutando en la sesión especificada.|
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
  - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
   [Referencia de comandos de servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
