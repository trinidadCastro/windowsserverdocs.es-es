---
title: shadow
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877116"
---
# <a name="shadow"></a>shadow

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le permite controlar remotamente una sesión activa de otro usuario en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<SessionName>|Especifica el nombre de la sesión que desea controlar de forma remota.|
|\<SessionID>|Especifica el identificador de la sesión que desea controlar de forma remota. Use **usuario consulta** para mostrar la lista de sesiones y los identificadores de sesión.|
|/ Server:\<ServerName >|Especifica el servidor Host de sesión de escritorio remoto que contiene la sesión que desea controlar de forma remota. De forma predeterminada, se usa el servidor de escritorio remoto Host4 de la sesión actual.|
|/v|Muestra información sobre las acciones que se va a realizar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Puede ver o controlar activamente la sesión. Si opta por controlar activamente una sesión de usuario, podrá acciones el teclado y mouse a la sesión.
-   Puede controlar remotamente siempre sus propias sesiones (excepto la sesión actual), pero debe tener el permiso Control total o el permiso de acceso especial Control remoto para controlar remotamente otra sesión.
-   También puede iniciar el control remoto mediante el uso de administrador de servicios de escritorio remoto.
-   Antes de comenzar la supervisión, el servidor advierte al usuario que la sesión está a punto de controlarse remotamente, a menos que esta advertencia está deshabilitada. La sesión parezca detenida durante unos segundos mientras se espera una respuesta del usuario. Para configurar el control remoto para los usuarios y sesiones, utilice la herramienta de configuración de servicios de escritorio remoto o las extensiones de servicios de escritorio remoto a los usuarios locales y grupos y active directory a los usuarios y equipos.
-   La sesión debe ser capaz de admitir la resolución de vídeo usada en la sesión que está controlando remotamente o se produce un error en la operación.
-   La sesión de consola puede ni controlar remotamente otra sesión ni puede ser controlada remotamente por otra sesión.
-   Cuando desea terminar el control remoto (vigilancia), presione CTRL + * (mediante el uso de \* del teclado numérico).

## <a name="BKMK_examples"></a>Ejemplos
-   Para vigilar la sesión 93, escriba:
    ```
    shadow 93
    ```
-   Para vigilar la sesión ACCTG01, escriba:
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
