---
title: bitsadmin setpeercachingflags
description: Comando de comandos de Windows para **bitsadmin setpeercachingflags**, que establece las marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel y si el trabajo puede descargar contenido de elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b4a7807975fb46440301e30b1fdbd01784d7c85
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122776"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Establece marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel y si el trabajo puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| value | Entero sin signo, incluido:<ul><li>**1.** el trabajo puede descargar contenido de elementos del mismo nivel.</li><li>**2.** los archivos del trabajo se pueden almacenar en caché y servir a los equipos del mismo nivel.</li></ul> |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establecen marcas para el trabajo denominado *myDownloadJob*, lo que permite descargar contenido de elementos del mismo nivel.

```
C:\>bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)