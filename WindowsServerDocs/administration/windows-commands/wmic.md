---
title: wmic
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885846"
---
# <a name="wmic"></a>wmic



Muestra información de WMI en un shell de comandos interactiva.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
command </parameter>
```

## <a name="sub-commands"></a>Subcomandos

Los subcomandos siguientes están disponibles en todo momento:

|Subcomando|Descripción|
|-----------|-----------|
|clase|Secuencias de escape del modo de alias predeterminado de WMIC para tener acceso a las clases del esquema WMI directamente.|
|ruta de acceso|Secuencias de escape del modo de alias predeterminado de WMIC para tener acceso a instancias en el esquema de WMI directamente.|
|context|Muestra los valores actuales de todos los modificadores globales.|
|[quit \| salir]|Se cierra el WMIC shell de comandos.|

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|</parameter>|\<Descripción concisa, comience con un verbo. >|
|</param2>|\<Otra descripción concisa, se inicia con un verbo. >|


## <a name="BKMK_examples"></a>Ejemplos

Para mostrar los valores actuales de todos los modificadores globales, escriba:
```
wmic context
```
Salida es similar a la muestra siguiente:
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
Para cambiar el idioma Id. utilizado por la línea de comandos para inglés (configuración regional ID 409), tipo:
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)