---
title: tsdiscon
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 577ff8ee672583b85c907642bd21256124aa8034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369866"
---
# <a name="tsdiscon"></a>tsdiscon

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta una sesión de un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|@no__t 0SessionId >|Especifica el identificador de la sesión que se va a desconectar.|
|@no__t 0SessionName >|Especifica el nombre de la sesión que se va a desconectar.|
|/Server: @no__t 0ServerName >|Especifica el servidor de Terminal Server que contiene la sesión que desea desconectar. De lo contrario, se usa el servidor host de sesión de escritorio remoto actual.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe tener el permiso control total o el permiso de acceso especial desconectar para desconectar a otro usuario de una sesión.
-   Si no se especifica ningún identificador de sesión o nombre de sesión, **tsdiscon** desconecta la sesión actual.
-   Las aplicaciones que se estaban ejecutando cuando se desconectó la sesión se ejecutan automáticamente cuando se vuelve a conectar a esa sesión sin pérdida de datos. Use **restablecer sesión** para finalizar las aplicaciones en ejecución de la sesión desconectada, pero tenga en cuenta que esto podría provocar la pérdida de datos en la sesión.
-   El parámetro **/Server** solo es necesario si se usa **tsdiscon** desde un servidor remoto.
-   No se puede desconectar la sesión de consola.

## <a name="BKMK_examples"></a>Example
- Para desconectar la sesión actual, escriba:
  ```
  tsdiscon
  ```
- Para desconectar la sesión 10, escriba:
  ```
  tsdiscon 10
  ```
- Para desconectar la sesión denominada TERM04, escriba:
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servicios de escritorio remoto &#40;referencia de comandos Terminal Services&#41; ](remote-desktop-services-terminal-services-command-reference.md)
