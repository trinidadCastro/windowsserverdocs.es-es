---
title: bitsadmin getaclflags
description: Tema de los comandos de Windows para **getaclflags bitsadmin** -recupera las marcas de propagaciones de lista de control de acceso.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861456"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera las marcas de propagaciones de lista (ACL) de control de acceso.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Muestra uno o varios de los valores de marca siguientes:
-   O: Copiar información de propietario con el archivo.
-   G: Copiar información de grupo con el archivo.
-   D: Copiar información de la DACL con el archivo.
-   S: Copiar información de la SACL con archivo.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recuperan los indicadores de propagación de lista de control de acceso del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)