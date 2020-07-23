---
title: reset session
description: Artículo de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 14ef7bdcb8490787b3fadff0cb842070f7a71446
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956246"
---
# <a name="reset-session"></a>reset session

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite restablecer (eliminar) una sesión en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).


> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<SessionName>|Especifica el nombre de la sesión que desea restablecer. Para determinar el nombre de la sesión, use el comando **query Session** .|
|\<SessionID>|Especifica el identificador de la sesión que se va a restablecer.|
|/server:\<ServerName>|Especifica el servidor de Terminal Server que contiene la sesión que desea restablecer. De lo contrario, se usa el servidor host de sesión de escritorio remoto actual.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
-   Siempre puede restablecer sus propias sesiones, pero debe tener permiso de acceso de control total para restablecer la sesión de otro usuario.
-   Tenga en cuenta que el restablecimiento de una sesión de usuario sin advertir al usuario puede provocar la pérdida de datos en la sesión.
-   Debe restablecer una sesión sólo si no funciona correctamente o ha dejado de responder.
-   El parámetro **/Server** solo es necesario si usa **restablecer sesión** desde un servidor remoto.

## <a name="examples"></a>Ejemplos
- Para restablecer la sesión designada como RDP-TCP # 6, escriba:
  ```
  reset session rdp-tcp#6
  ```
- Para restablecer la sesión que utiliza el ID. de sesión 3, escriba:
  ```
  reset session 3
  ```

## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Referencia de comandos de servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
