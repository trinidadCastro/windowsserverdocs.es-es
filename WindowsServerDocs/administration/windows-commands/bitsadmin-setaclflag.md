---
title: bitsadmin setaclflag
description: Tema de los comandos de Windows para **setaclflag bitsadmin** -establece el control de acceso marcas propagaciones de la lista.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867406"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Establece el control de acceso de marcas de propagaciones de lista (ACL) para el trabajo. Los marcadores indican que desea mantener la información de la ACL y el propietario con el archivo que se va a descargar. Por ejemplo, para mantener el propietario y el grupo con el archivo, establezca **marcas** a `OG`.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Flags|Especifique uno o varios de los valores de marca siguientes:</br>-   O: Copiar información de propietario con el archivo.</br>-   G: Copiar información de grupo con el archivo.</br>-D: Copiar información de la DACL con el archivo.</br>-S: copia SACL información con el archivo.|

## <a name="remarks"></a>Comentarios

El modificador SetAclFlags se utiliza para mantener la información de lista de control de propietario y el acceso cuando un trabajo descarga de datos de un recurso compartido de Windows (SMB).

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece el control de acceso de marcadores de propagación de la lista del trabajo denominado *myDownloadJob* para mantener la información de propietario y el grupo con los archivos descargados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)