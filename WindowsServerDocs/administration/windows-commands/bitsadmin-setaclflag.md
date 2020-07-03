---
title: bitsadmin setaclflag
description: Artículo de referencia para el comando bitsadmin setaclflag, que establece las marcas de propagación de la lista de control de acceso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dcf07f944813c0c8d7a4ff4c4f52c598c0f3bf47
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927951"
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
