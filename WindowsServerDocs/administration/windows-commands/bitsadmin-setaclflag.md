---
title: bitsadmin setaclflag
description: Windows Commands topic for bitsadmin setaclflag, que establece las marcas de propagación de la lista de control de acceso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ac47e554dde6a555e891d89668cd12fec3179d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849678"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Establece las marcas de propagación de la lista de control de acceso (ACL) para el trabajo. Las marcas indican que desea mantener la información del propietario y de la ACL con el archivo que se está descargando. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca **flags** en `OG`.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetAclFlags <Job> <Flags>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Flags|Especifique uno o varios de los siguientes valores de marca:</br>-O: copiar información del propietario con el archivo.</br>-G: copiar información de grupo con el archivo.</br>-D: copiar información de DACL con el archivo.</br>-S: copiar información de SACL con el archivo.|

## <a name="remarks"></a>Comentarios

El modificador SetAclFlags se usa para mantener la información de la lista de control de acceso y el propietario cuando un trabajo está descargando datos de un recurso compartido de Windows (SMB).

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establecen los marcadores de propagación de la lista de control de acceso para el trabajo denominado *myDownloadJob* para mantener la información del propietario y del grupo con los archivos descargados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)