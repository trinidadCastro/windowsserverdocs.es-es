---
title: bitsadmin peercaching y setconfigurationflags
description: Artículo de referencia del comando bitsadmin y setconfigurationflags, que establece las marcas de configuración que determinan si el equipo puede servir contenido a los equipos del mismo nivel y si puede descargar contenido de los equipos del mismo nivel.
ms.topic: reference
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0027fa5c926f09b95541906f168e852292b19aaf
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026532"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching y setconfigurationflags

Establece las marcas de configuración que determinan si el equipo puede servir contenido a los elementos del mismo nivel y si puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| value | Entero sin signo con la siguiente interpretación de los bits en la representación binaria:<ul><li>Para permitir que los datos del trabajo se descarguen de un elemento del mismo nivel, establezca el bit menos significativo.</li><li>Para permitir que los datos del trabajo se atiendan a los elementos del mismo nivel, establezca el segundo bit de la derecha.</li></ul>|

## <a name="examples"></a>Ejemplos

Para especificar los datos del trabajo que se van a descargar de los elementos del mismo nivel para el trabajo denominado *myDownloadJob*:

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)

- [bitsadmin (comando de caché)](bitsadmin-peercaching.md)
