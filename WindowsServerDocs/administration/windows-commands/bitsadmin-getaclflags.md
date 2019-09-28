---
title: bitsadmin getaclflags
description: En el tema comandos de Windows para **bitsadmin getaclflags** se recuperan las marcas de propagación de la lista de control de acceso.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381788"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera las marcas de propagación de la lista de control de acceso (ACL).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Muestra uno o varios de los siguientes valores de marca:
-   ACEPTAR Copie la información del propietario con el archivo.
-   I Copiar información de grupo con el archivo.
-   D: Copiar información de DACL con el archivo.
-   S: Copie la información de SACL con el archivo.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recuperan los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)