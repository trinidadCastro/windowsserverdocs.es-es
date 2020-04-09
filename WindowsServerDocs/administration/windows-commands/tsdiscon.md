---
title: tsdiscon
description: Temas de comandos de Windows para tsdiscon, que desconecta una sesión de un servidor host de sesión de escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b008fa920290043b64e7421e91a545123634f1e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832518"
---
# <a name="tsdiscon"></a>tsdiscon

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta una sesión de un servidor host de sesión de Escritorio remoto.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|\<SessionId >|Especifica el identificador de la sesión que se va a desconectar.|
|\<nombresesión >|Especifica el nombre de la sesión que se va a desconectar.|
|/Server:\<ServerName >|Especifica el servidor de Terminal Server que contiene la sesión que desea desconectar. De lo contrario, se usa el servidor host de sesión de escritorio remoto actual.|
|/v|Muestra información acerca de las acciones que se llevan a cabo.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Debe tener el permiso control total o el permiso de acceso especial desconectar para desconectar a otro usuario de una sesión.
-   Si no se especifica ningún identificador de sesión o nombre de sesión, **tsdiscon** desconecta la sesión actual.
-   Las aplicaciones que se estaban ejecutando cuando se desconectó la sesión se ejecutan automáticamente cuando se vuelve a conectar a esa sesión sin pérdida de datos. Use **restablecer sesión** para finalizar las aplicaciones en ejecución de la sesión desconectada, pero tenga en cuenta que esto podría provocar la pérdida de datos en la sesión.
-   El parámetro **/Server** solo es necesario si se usa **tsdiscon** desde un servidor remoto.
-   No se puede desconectar la sesión de consola.

## <a name="examples"></a><a name=BKMK_examples></a>Example
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
  ## <a name="additional-references"></a>Referencias adicionales
  - Referencia [de comandos de
  de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
