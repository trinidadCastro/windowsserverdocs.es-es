---
title: bitsadmin peercaching y setconfigurationflags
description: Tema de referencia para el comando bitsadmin de caché y setconfigurationflags, que establece las marcas de configuración que determinan si el equipo puede servir contenido a los equipos del mismo nivel y si puede descargar contenido de los equipos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c3ce69ce7a372311ce0c30e9b3a391ea33f45ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717236"
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
