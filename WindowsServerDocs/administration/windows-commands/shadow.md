---
title: shadow
description: Comandos de Windows tema de sombra, que permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión Escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90c3202d810257cc94c73b88c5c1627901f54af0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834338"
---
# <a name="shadow"></a>shadow

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión de Escritorio remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|\<nombresesión >|Especifica el nombre de la sesión que desea controlar de forma remota.|
|\<SessionID >|Especifica el identificador de la sesión que desea controlar de forma remota. Use el usuario de la **consulta** para mostrar la lista de sesiones y sus identificadores de sesión.|
|/Server:\<ServerName >|Especifica el servidor host de sesión de escritorio remoto que contiene la sesión que desea controlar de forma remota. De forma predeterminada, se usa el servidor de Host4 de sesión de escritorio remoto actual.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Puede ver o controlar activamente la sesión. Si opta por controlar activamente una sesión de usuario, puede especificar acciones de teclado y mouse de entrada para la sesión.
-   Siempre puede controlar de forma remota sus propias sesiones (excepto la sesión actual), pero debe tener el permiso control total o el permiso de acceso especial control remoto para controlar de forma remota otra sesión.
-   También puede iniciar el control remoto mediante el administrador de Servicios de Escritorio remoto.
-   Antes de que comience la supervisión, el servidor advierte al usuario de que la sesión está a punto de controlarse remotamente, a menos que está advertencia esté deshabilitada. Es posible que la sesión parezca detenida durante unos segundos mientras espera una respuesta del usuario. Para configurar el control remoto para usuarios y sesiones, use la herramienta de configuración de Servicios de Escritorio remoto o las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.
-   Si la sesión no es compatible con la resolución de vídeo usada en la sesión que está controlando remotamente, la operación no se realiza correctamente.
-   La sesión de consola no puede controlar de forma remota otra sesión ni se puede controlar de forma remota en otra sesión.
-   Cuando desee finalizar el control remoto (sombreado), presione CTRL +\* (usando solo \* del teclado numérico).

## <a name="examples"></a><a name=BKMK_examples></a>Example
-   Para Shadow Session 93, escriba:
    ```
    shadow 93
    ```
-   Para sombrear la sesión ACCTG01, escriba:
    ```
    shadow ACCTG01
    ```

## <a name="additional-references"></a>Referencias adicionales
- Referencia [de comandos de
de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
