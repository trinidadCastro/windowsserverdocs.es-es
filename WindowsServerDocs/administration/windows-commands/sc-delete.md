---
title: Eliminación de SC
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 68af5f118b2cc9d7941abddccd2a1bc7fde4c6d0
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222933"
---
# <a name="sc-delete"></a>Eliminación de SC



Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto en el servicio, el servicio está marcado para eliminación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe tener el formato de convención de nomenclatura Universal (UNC) (por ejemplo, \\ \\myserver). Para ejecutar SC.exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre del servicio devolviendo por la **getkeyname** operación.|
|?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Use **agregar o quitar programas** en **Panel de Control** eliminar DHCP, DNS o cualquier otro servicio de sistema operativo integrada. Tenga en cuenta que **agregar o quitar programas** solo no quitará la subclave del registro para el servicio, pero también desinstala el servicio y eliminar todos los accesos directos a él.

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** desde el registro en el equipo local, escriba:
```
sc delete newserv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)