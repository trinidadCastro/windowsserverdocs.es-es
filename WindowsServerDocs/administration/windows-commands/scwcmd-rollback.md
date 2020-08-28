---
title: Reversión de scwcmd
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25c726b649028f66ca97ebc0280175d1713b7ef7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036183"
---
# <a name="scwcmd-rollback"></a>Scwcmd: Rollback

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión.

## <a name="syntax"></a>Sintaxis

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/m\<ComputerName>|Especifica el nombre NetBIOS, el nombre DNS o la dirección IP de un equipo en el que se debe realizar la operación de reversión.|
|/u\<UserName>|Especifica la cuenta de usuario alternativa que se va a usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/PW\<Password>|Especifica una credencial de usuario alternativa para usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para revertir la Directiva de seguridad en un equipo con la dirección IP 172.16.0.0, escriba:
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)