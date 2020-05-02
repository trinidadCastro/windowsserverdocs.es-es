---
title: Reversión de scwcmd
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cd4eeec1113717a40dca43f0320f2db3c4c414e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722131"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión.

## <a name="syntax"></a>Sintaxis

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/m:\<COMPUTERNAME>|Especifica el nombre NetBIOS, el nombre DNS o la dirección IP de un equipo en el que se debe realizar la operación de reversión.|
|/u:\<nombre de usuario>|Especifica la cuenta de usuario alternativa que se va a usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/PW:\<contraseña>|Especifica una credencial de usuario alternativa para usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd. exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para revertir la Directiva de seguridad en un equipo con la dirección IP 172.16.0.0, escriba:
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)