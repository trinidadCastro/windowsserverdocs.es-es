---
title: cacls
description: Artículo de referencia para el comando cacls. Este comando está en desuso y no se garantiza que se admita en versiones futuras de Windows.
ms.topic: reference
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8261e9711cee85ad1d59ff71f9cd8ac55e63fab8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034283"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> Este comando está en desuso. En su lugar, use [icacls](icacls.md) .

Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados.

## <a name="syntax"></a>Sintaxis

```
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<filename>` | Necesario. Muestra las ACL de los archivos especificados. |
| /t | Cambia las ACL de los archivos especificados en el directorio actual y en todos los subdirectorios. |
| /m | Cambia las ACL de los volúmenes montados en un directorio. |
| /l | Funciona en el propio vínculo simbólico en lugar de en el destino. |
| /s: SDDL | Reemplaza las ACL por las especificadas en la cadena SDDL. Este parámetro no es válido para su uso con los parámetros **/e**, **/g**, **/r**, **/p**o **/d** . |
| /e | Edite una ACL en lugar de reemplazarla. |
| /C | Continuar después de errores de acceso denegado. |
| `/g user:<perm>` | Concede derechos de acceso de usuario especificados, incluidos estos valores válidos para el permiso:<ul><li>**n** -ninguno</li><li>**r** : lectura</li><li>**w** -escritura</li><li>**c** : cambiar (escribir)</li><li>control total de **f**</li></ul> |
| /r usuario [...] | Revocar los derechos de acceso del usuario especificado. Solo es válido cuando se usa con el parámetro **/e** . |
| `[/p user:<perm> [...]` | Reemplazar los derechos de acceso del usuario especificado, incluidos los valores válidos para el permiso:<ul><li>**n** -ninguno</li><li>**r** : lectura</li><li>**w** -escritura</li><li>**c** : cambiar (escribir)</li><li>control total de **f**</li></ul> |
| [/d usuario [...] | Denegar el acceso de usuario especificado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="sample-output"></a>Salida de ejemplo

| Output | Entrada de control de acceso (ACE) se aplica a |
-------- | ------------------------------------- |
| OI | Herencia de objeto. Esta carpeta y archivos. |
| CI | Herencia del contenedor. Esta carpeta y sus subcarpetas. |
| IO | Heredar únicamente. La ACE no se aplica al archivo o directorio actual. |
| Sin mensaje de salida | Solo esta carpeta. |
| OI IA | Esta carpeta, subcarpetas y archivos. |
| OI IA IO | Solo subcarpetas y archivos. |
| IA IO | Solo subcarpetas. |
| OI IO | Solo archivos. |

#### <a name="remarks"></a>Observaciones

- Puede usar caracteres comodín (**?** y **&#42;**) para especificar varios archivos.

- Puede especificar más de un usuario.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [icacls](icacls.md)
