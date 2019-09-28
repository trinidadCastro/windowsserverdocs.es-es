---
title: shadow
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb2fad4b0a553e736755f2dc56e5d88297a1fef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383960"
---
# <a name="shadow"></a>shadow

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|@no__t 0SessionName >|Especifica el nombre de la sesión que desea controlar de forma remota.|
|@no__t 0SessionID >|Especifica el identificador de la sesión que desea controlar de forma remota. Use el usuario de la **consulta** para mostrar la lista de sesiones y sus identificadores de sesión.|
|/Server: @no__t 0ServerName >|Especifica el servidor host de sesión de escritorio remoto que contiene la sesión que desea controlar de forma remota. De forma predeterminada, se usa el servidor de Host4 de sesión de escritorio remoto actual.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Puede ver o controlar activamente la sesión. Si opta por controlar activamente la sesión de un usuario, podrá escribir acciones del teclado y del mouse en la sesión.
-   Siempre puede controlar de forma remota sus propias sesiones (excepto la sesión actual), pero debe tener el permiso control total o el permiso de acceso especial control remoto para controlar de forma remota otra sesión.
-   También puede iniciar el control remoto mediante el administrador de Servicios de Escritorio remoto.
-   Antes de que comience la supervisión, el servidor advierte al usuario de que la sesión está a punto de controlarse de forma remota, a menos que esta advertencia esté deshabilitada. Puede parecer que la sesión está inmovilizada durante unos segundos mientras espera una respuesta del usuario. Para configurar el control remoto para usuarios y sesiones, use la herramienta de configuración de Servicios de Escritorio remoto o las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.
-   La sesión debe ser capaz de admitir la resolución de vídeo usada en la sesión que se controla de forma remota o se produce un error en la operación.
-   La sesión de consola no puede controlar de forma remota otra sesión ni se puede controlar de forma remota en otra sesión.
-   Si desea terminar el control remoto (sombreado), presione CTRL + \* (usando \* del teclado numérico únicamente).

## <a name="BKMK_examples"></a>Example
-   Para Shadow Session 93, escriba:
    ```
    shadow 93
    ```
-   Para sombrear la sesión ACCTG01, escriba:
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[servicios de escritorio remoto &#40;referencia de comandos Terminal Services&#41; ](remote-desktop-services-terminal-services-command-reference.md)
