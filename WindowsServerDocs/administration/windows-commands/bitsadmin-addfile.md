---
title: bitsadmin addfile
description: Tema de los comandos de Windows para **bitsadmin addfile** -agrega un archivo para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c3027bdc4f3f8f3e3ca50400b2c5dbf33bf2bc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861756"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Agrega un archivo para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|RemoteURL|La dirección URL del archivo en el servidor.|
|LocalName|El nombre del archivo en el equipo local. *LocalName* debe contener una ruta de acceso absoluta al archivo.|

## <a name="BKMK_examples"></a>Ejemplos

Agregue un archivo para el trabajo. Repita esta llamada para cada archivo que desea agregar. Si usan varios trabajos *myDownloadJob* como su nombre, debe reemplazar *myDownloadJob* con el GUID del trabajo para identificar de forma única el trabajo.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)