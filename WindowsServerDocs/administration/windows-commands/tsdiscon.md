---
title: tsdiscon
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1b5fca329864ebed9eab66671a17493f0fc3ca8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440913"
---
# <a name="tsdiscon"></a>tsdiscon

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta una sesión de un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<SessionId>|Especifica el identificador de la sesión a desconectar.|
|\<SessionName>|Especifica el nombre de la sesión a desconectar.|
|/ Server:\<ServerName >|Especifica el servidor de terminal server que contiene la sesión que desea desconectar. En caso contrario, se usa el servidor Host de sesión de escritorio remoto actual.|
|/v|Muestra información sobre las acciones que se va a realizar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe tener el permiso Control total o desconecte el permiso de acceso especial Desconectar otro usuario de una sesión.
-   Si no se especifica ningún Id. de sesión o el nombre de la sesión, **tsdiscon** desconecta la sesión actual.
-   Las aplicaciones que se estaban ejecutando cuando se desconecta la sesión se ejecutan automáticamente cuando se vuelve a conectar a esa sesión sin pérdida de datos. Use **restablecer sesión** para finalizar las aplicaciones en ejecución de la sesión desconectada, pero tenga en cuenta que esto podría provocar la pérdida de datos en la sesión.
-   El **/Server** parámetro es obligatorio solo si usa **tsdiscon** desde un servidor remoto.
-   No se puede desconectar la sesión de consola.

## <a name="BKMK_examples"></a>Ejemplos
- Para desconectar la sesión actual, escriba:
  ```
  tsdiscon
  ```
- Para desconectar la sesión 10, escriba:
  ```
  tsdiscon 10
  ```
- Para desconectar la sesión TERM04, escriba:
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
