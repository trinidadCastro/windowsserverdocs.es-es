---
title: bitsadmin getaclflags
description: Temas de comandos de Windows para **bitsadmin getaclflags**, que recupera las marcas de propagación de la lista de control de acceso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d53018e2fa5c659c8cf4b0ec985beda848a8c1af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850798"
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

## <a name="remarks"></a>Comentarios

Muestra uno o varios de los siguientes valores de marca:

- **o** copiar información del propietario con el archivo.

- **g** -copiar información de grupo con el archivo.

- **d** -copiar información de la lista de control de acceso discrecional (DACL) con el archivo.

- **s** -copiar información de la lista de control de acceso de sistema (SACL) con el archivo.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)