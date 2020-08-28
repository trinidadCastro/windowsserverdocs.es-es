---
title: bitsadmin getaclflags
description: Artículo de referencia para el comando bitsadmin getaclflags, que recupera las marcas de propagación de la lista de control de acceso (ACL).
ms.topic: reference
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5254d65bb5ba3e35fcf5368e24045530a76bfd95
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033703"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera las marcas de propagación de la lista de control de acceso (ACL), que reflejan si los objetos secundarios heredan los elementos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

### <a name="remarks"></a>Observaciones

Devuelve uno o varios de los siguientes valores de marca:

- **o** copiar información del propietario con el archivo.

- **g** -copiar información de grupo con el archivo.

- **d** -copiar información de la lista de control de acceso discrecional (DACL) con el archivo.

- **s** -copiar información de la lista de control de acceso de sistema (SACL) con el archivo.

## <a name="examples"></a>Ejemplos

Para recuperar los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
