---
title: bitsadmin setpeercachingflags
description: Tema de referencia para el comando bitsadmin setpeercachingflags, que establece las marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel y si el trabajo puede descargar contenido de elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b66b169c38ac050ecaaf6546365547148faa9cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717267"
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

Para permitir que el trabajo denominado *myDownloadJob* Descargue contenido de elementos del mismo nivel:

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
