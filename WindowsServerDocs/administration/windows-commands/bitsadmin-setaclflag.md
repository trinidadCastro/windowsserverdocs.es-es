---
title: bitsadmin setaclflag
description: 'Windows Commands topic for **bitsadmin setaclflag** : establece las marcas de propagación de la lista de control de acceso.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fbdb12c29af7b4db8b25846d43ee1c93b2454ff2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380756"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Establece las marcas de propagación de la lista de control de acceso (ACL) para el trabajo. Las marcas indican que desea mantener la información del propietario y de la ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca **marcas** to `OG`.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Flags|Especifique uno o varios de los siguientes valores de marca:</br>ACEPTAR Copie la información del propietario con el archivo.</br>I Copiar información de grupo con el archivo.</br>D Copiar información de DACL con el archivo.</br>-S: copiar información de SACL con el archivo.|

## <a name="remarks"></a>Comentarios

El modificador SetAclFlags se usa para mantener la información de la lista de control de acceso y el propietario cuando un trabajo está descargando datos de un recurso compartido de Windows (SMB).

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establecen los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob* para mantener la información del propietario y del grupo con los archivos descargados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)