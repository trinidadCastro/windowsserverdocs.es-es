---
title: Scwcmd rollback
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852206"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Se aplica la directiva de deshacer más reciente disponible y, a continuación, elimina dicha directiva.

## <a name="syntax"></a>Sintaxis

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ m:\<nombreDeEquipo >|Especifica el nombre NetBIOS, nombre DNS o dirección IP de un equipo donde se debe realizar la operación de reversión.|
|/ u:\<nombre de usuario >|Especifica una cuenta de usuario alternativa que se utilizará al realizar una recuperación remota. El valor predeterminado es el inicio de sesión de usuario.|
|/ pw:\<contraseña >|Especifica una credencial de usuario alternativa que se utilizará al realizar una recuperación remota. El valor predeterminado es el inicio de sesión de usuario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para revertir la directiva de seguridad en un equipo en la dirección IP 172.16.0.0, escriba:
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)