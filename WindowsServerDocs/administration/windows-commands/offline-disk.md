---
title: disco sin conexión
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a00b7f51bc950c3737ba6350fe15a7a4c6cc45e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723446"
---
# <a name="offline-disk"></a>disco sin conexión



Toma el disco en línea con el foco en el estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline disk [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Observaciones

-   Este comando funciona en los discos que están en modo SAN online. Cambia el modo de SAN a sin conexión.
-   Si se desconecta un disco dinámico de un grupo de discos, el estado del disco cambia a **ausente** y el grupo muestra un disco sin conexión. El disco que falta se mueve al grupo no válido. Si el disco dinámico es el último disco del grupo, el estado del disco cambiará a **sin conexión**y se quitará el grupo vacío.
-   Se debe seleccionar un disco para que el comando de **disco sin conexión** se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:
```
offline disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

