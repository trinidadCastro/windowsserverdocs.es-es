---
title: tskill
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b582334d7b79b2badbb86818be1093b6a5f55080
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440816"
---
# <a name="tskill"></a>tskill

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza un proceso ejecutado en una sesión en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<ProcessID>|Especifica el identificador del proceso que desea finalizar.|
|\<ProcessName >|Especifica el nombre del proceso que desea finalizar. Este parámetro puede incluir caracteres comodín.|
|/ Server:\<ServerName >|Especifica el servidor de terminal server que contiene el proceso que desea finalizar. Si **/Server** no se especifica, se usa el servidor Host de sesión de escritorio remoto actual.|
|/ Id:\<SessionID >|Finaliza el proceso que se está ejecutando en la sesión especificada.|
|/a|Finaliza el proceso que se ejecuta en todas las sesiones.|
|/v|Muestra información sobre las acciones que se va a realizar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
- Puede usar **tskill** para finalizar solamente los procesos que pertenecen a usted, a menos que sea un administrador. Los administradores tienen acceso total a todos **tskill** funciones y pueden finalizar los procesos que se ejecutan en otras sesiones de usuario.
- Cuando finalizan todos los procesos que se ejecutan en una sesión, la sesión también termina.
- Si usas el *ProcessName* y **/Server:** <em>ServerName</em> parámetros, también debe especificar el **/ID:**  <em>SessionID</em> o **/a** parámetro.

## <a name="BKMK_examples"></a>Ejemplos
- Para finalizar el proceso 6543, escriba:
  ```
  tskill 6543
  ```
- Para finalizar el proceso "explorer" que se ejecutan en la sesión 5, escriba:
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
