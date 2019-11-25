---
title: Eliminación de SC
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad64d0f7c772b8d29a191b5f3e690d74c8765717
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371280"
---
# <a name="sc-delete"></a>Eliminación de SC



Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto para el servicio, el servicio se marca para su eliminación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName >|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, \\\\Server). Para ejecutar SC. exe localmente, omita este parámetro.|
|\<ServiceName >|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Use **Agregar o quitar programas** en el **Panel de control** para eliminar DHCP, DNS o cualquier otro servicio de sistema operativo integrado. Tenga en cuenta que en **Agregar o quitar programas** no solo se quitará la subclave del registro para el servicio, sino que también se desinstalará el servicio y se eliminarán los accesos directos a él.

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** del registro en el equipo local, escriba:
```
sc delete newserv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)