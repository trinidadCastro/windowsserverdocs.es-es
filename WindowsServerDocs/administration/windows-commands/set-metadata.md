---
title: Establecer metadatos
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac73a4131d3f4065cd1aeae873734b079ad664e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370955"
---
# <a name="set-metadata"></a>Establecer metadatos



Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas que se usa para transferir instantáneas de un equipo a otro. Si se usa sin parámetros, el comando **set Metadata** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive >:] [<Path>]|Especifica la ubicación en la que se va a crear el archivo de metadatos.|
|@no__t -0MetaData. cab >|Especifica el nombre del archivo. cab para almacenar los metadatos de creación de instantáneas.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)