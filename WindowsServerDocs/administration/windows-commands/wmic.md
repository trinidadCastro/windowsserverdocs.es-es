---
title: wmic
description: Comando comandos de Windows para WMIC, que muestra información de WMI en un shell de comandos interactivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba4ecb4b12b03e010318bf6ca260dec00f28f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829058"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

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
