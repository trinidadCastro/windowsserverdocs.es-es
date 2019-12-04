---
title: wmic
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f5096ab82ebbd01cb4f3a7dc0cf0b15e4b9fae8e
ms.sourcegitcommit: effbc183bf4b370905d95c975626c1ccde057401
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781332"
---
# <a name="wmic"></a>wmic



Muestra información de WMI en un shell de comandos interactivo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wmic </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Los siguientes subcomandos están disponibles en todo momento:

|Subcomando|Descripción|
|-----------|-----------|
|clase|Escapa del modo de alias predeterminado de WMIC para tener acceso directamente a las clases del esquema WMI.|
|ruta de acceso|Escapa del modo de alias predeterminado de WMIC para tener acceso directamente a las instancias del esquema WMI.|
|contexto|Muestra los valores actuales de todos los conmutadores globales.|
|[salir de \| salir]|Sale del shell de comandos de WMIC.|

## <a name="BKMK_examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
