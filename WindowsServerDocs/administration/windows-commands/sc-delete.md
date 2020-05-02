---
title: Eliminación de SC
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd40b5eb82def3b3c437cbdb5b60d279529d25a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722191"
---
# <a name="sc-delete"></a>Eliminación de SC



Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto para el servicio, el servicio se marca para su eliminación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDeServidor>|Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por \\ \\ejemplo, mi Server). Para ejecutar SC. exe localmente, omita este parámetro.|
|\<ServiceName>|Especifica el nombre de servicio devuelto por la operación **getkeyname** .|
|?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Use **Agregar o quitar programas** en el **Panel de control** para eliminar DHCP, DNS o cualquier otro servicio de sistema operativo integrado. Tenga en cuenta que en **Agregar o quitar programas** no solo se quitará la subclave del registro para el servicio, sino que también se desinstalará el servicio y se eliminarán los accesos directos a él.

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** del registro en el equipo local, escriba:
```
sc delete newserv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)