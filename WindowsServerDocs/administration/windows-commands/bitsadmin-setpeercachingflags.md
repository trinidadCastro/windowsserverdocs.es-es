---
title: bitsadmin setpeercachingflags
description: Comando de comandos de Windows para bitsadmin setpeercachingflags, que establece las marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel y si el trabajo puede descargar contenido de elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d19e4d14b47e4aa96e9ad9d4367e872350ad4d43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849248"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Establece marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel y si el trabajo puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Valor|El valor es un entero sin signo con la siguiente interpretación de los bits en la representación binaria.</br>1: el trabajo puede descargar contenido de elementos del mismo nivel.</br>2-los archivos del trabajo se pueden almacenar en caché y servir a los equipos del mismo nivel.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establecen marcas para el trabajo denominado *myJob* , lo que permite descargar contenido de elementos del mismo nivel.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)