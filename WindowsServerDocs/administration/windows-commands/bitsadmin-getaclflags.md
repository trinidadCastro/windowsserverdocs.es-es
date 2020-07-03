---
title: bitsadmin getaclflags
description: Artículo de referencia para el comando bitsadmin getaclflags, que recupera las marcas de propagación de la lista de control de acceso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a05a0ed1c29e7cf1b0583ce9a0fcfdbe73e7a12
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928339"
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

### <a name="remarks"></a>Comentarios

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
