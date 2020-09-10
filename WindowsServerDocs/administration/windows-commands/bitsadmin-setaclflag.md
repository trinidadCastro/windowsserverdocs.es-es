---
title: bitsadmin setaclflag
description: Artículo de referencia para el comando bitsadmin setaclflag, que establece las marcas de propagación de la lista de control de acceso (ACL).
ms.topic: reference
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0cc0dac9027bd76735592620f89118318548dbdb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631053"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Establece las marcas de propagación de la lista de control de acceso (ACL) para el trabajo. Las marcas indican que desea mantener la información del propietario y de la ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca el parámetro **Flags** en `og` .

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| flags | Especifique uno o varios de los valores, entre los que se incluyen:<ul><li>**o** copiar información del propietario con el archivo.</li><li>**g** -copiar información de grupo con el archivo.</li><li>**d** -copiar información de la lista de control de acceso discrecional (DACL) con el archivo.</li><li>**s** -copiar información de la lista de control de acceso de sistema (SACL) con el archivo.</li></ul> |

## <a name="examples"></a>Ejemplos

Para establecer las marcas de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob*, de modo que mantiene la información de propietario y de grupo con los archivos descargados.

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
