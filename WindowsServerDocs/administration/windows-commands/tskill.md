---
title: tskill
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 697363c91837ff675a14099fd212f4f0753b739b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392333"
---
# <a name="tskill"></a>tskill

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza un proceso que se ejecuta en una sesión en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|@no__t 0ProcessID >|Especifica el identificador del proceso que desea finalizar.|
|@no__t 0ProcessName >|Especifica el nombre del proceso que desea finalizar. Este parámetro puede incluir caracteres comodín.|
|/Server: @no__t 0ServerName >|Especifica el servidor de Terminal Server que contiene el proceso que desea finalizar. Si no se especifica **/Server** , se usa el servidor host de sesión de escritorio remoto actual.|
|/ID: \<SessionID >|Finaliza el proceso que se está ejecutando en la sesión especificada.|
|/a|Finaliza el proceso que se está ejecutando en todas las sesiones.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
- Puede usar **tskill** para finalizar solo los procesos que le pertenecen, a menos que sea administrador. Los administradores tienen acceso total a todas las funciones de **tskill** y pueden finalizar los procesos que se ejecutan en otras sesiones de usuario.
- Cuando todos los procesos que se ejecutan en una sesión finalizan, la sesión también finaliza.
- Si usa los parámetros *processName* y **/Server:** <em>ServerName</em> , también debe especificar el parámetro **/ID:** <em>SessionID</em> o **/a** .

## <a name="BKMK_examples"></a>Example
- Para finalizar el proceso 6543, escriba:
  ```
  tskill 6543
  ```
- Para finalizar el proceso "explorador" que se ejecuta en la sesión 5, escriba:
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servicios de escritorio remoto &#40;referencia de comandos Terminal Services&#41; ](remote-desktop-services-terminal-services-command-reference.md)
