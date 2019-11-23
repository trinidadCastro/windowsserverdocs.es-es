---
title: Reversión de scwcmd
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f089ea3e6e5d5b95080356dd239272b95a76b37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371211"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión.

## <a name="syntax"></a>Sintaxis

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/m:\<ComputerName >|Especifica el nombre NetBIOS, el nombre DNS o la dirección IP de un equipo en el que se debe realizar la operación de reversión.|
|/u:\<nombre de usuario >|Especifica la cuenta de usuario alternativa que se va a usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/PW:\<contraseña >|Especifica una credencial de usuario alternativa para usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd. exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Example

Para revertir la Directiva de seguridad en un equipo con la dirección IP 172.16.0.0, escriba:
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)