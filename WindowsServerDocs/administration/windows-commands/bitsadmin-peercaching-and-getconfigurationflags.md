---
title: bitsadmin peercaching y getconfigurationflags
description: Artículo de referencia para el comando bitsadmin de caché y getconfigurationflags, que obtiene las marcas de configuración que determinan si el equipo atiende el contenido a los equipos del mismo nivel y si puede descargar contenido de los equipos del mismo nivel.
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 383387b135f38663a84999e041a4f6864d40a01d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893622"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin peercaching y getconfigurationflags

Obtiene las marcas de configuración que determinan si el equipo atiende el contenido a los elementos del mismo nivel y si puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para obtener las marcas de configuración para el trabajo denominado *myDownloadJob*:

```
bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)

- [bitsadmin (comando de caché)](bitsadmin-peercaching.md)
