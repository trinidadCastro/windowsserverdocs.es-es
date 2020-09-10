---
title: bitsadmin getaclflags
description: Artículo de referencia para el comando bitsadmin getaclflags, que recupera las marcas de propagación de la lista de control de acceso (ACL).
ms.topic: reference
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4c5c7101db157b7fa5b56833b8d5f89619bd8d51
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632328"
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
