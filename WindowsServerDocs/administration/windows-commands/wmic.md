---
title: wmic
description: Artículo de referencia de WMIC, que muestra información de WMI en un shell de comandos interactivo.
ms.topic: reference
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6396f3e6ebc39722c3f1a79d32d629b206cceba2
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038093"
---
# <a name="wmic"></a>wmic



Muestra información de WMI en un shell de comandos interactivo.



## <a name="syntax"></a>Sintaxis

```
wmic </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Los siguientes subcomandos están disponibles en todo momento:

|Subcomando|Descripción|
|-----------|-----------|
|clase|Escapa del modo de alias predeterminado de WMIC para tener acceso directamente a las clases del esquema WMI.|
|path|Escapa del modo de alias predeterminado de WMIC para tener acceso directamente a las instancias del esquema WMI.|
|context|Muestra los valores actuales de todos los conmutadores globales.|
|[salir \| salir]|Sale del shell de comandos de WMIC.|

## <a name="examples"></a>Ejemplos

Para mostrar los valores actuales de todos los conmutadores globales, escriba:
```
wmic context
```
El resultado es similar al que se muestra a continuación:
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
Para cambiar el identificador de idioma usado por la línea de comandos a Inglés (ID. de configuración regional 409), escriba:
```
wmic /locale:ms_409
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
