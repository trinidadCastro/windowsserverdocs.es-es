---
title: bitsadmin setaclflag
description: Temas de comandos de Windows para **bitsadmin setaclflag**, que establece las marcas de propagación de la lista de control de acceso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0aae550e94d04db518edccafb1d6bcf46d0320b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123064"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Establece las marcas de propagación de la lista de control de acceso (ACL) para el trabajo. Las marcas indican que desea mantener la información del propietario y de la ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca el parámetro **Flags** en `og`.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| flags | Especifique uno o varios de los valores, entre los que se incluyen:<ul><li>**o** copiar información del propietario con el archivo.</li><li>**g** -copiar información de grupo con el archivo.</li><li>**d** -copiar información de la lista de control de acceso discrecional (DACL) con el archivo.</li><li>**s** -copiar información de la lista de control de acceso de sistema (SACL) con el archivo.</li></ul> |

## <a name="remarks"></a>Comentarios

El modificador/setaclflag se usa para mantener la información de la lista de control de acceso y el propietario cuando un trabajo está descargando datos de un recurso compartido de Windows (SMB).

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establecen los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob* para mantener la información del propietario y del grupo con los archivos descargados.

```
C:\>bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)&reg;'    