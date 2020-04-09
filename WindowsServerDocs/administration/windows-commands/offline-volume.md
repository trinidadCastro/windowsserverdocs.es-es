---
title: volumen sin conexión
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 821132b41b55410223c0310f283b076526c9fbc4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837978"
---
# <a name="offline-volume"></a>volumen sin conexión



Toma el volumen en línea que tiene el foco en el estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline volume [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un volumen para que esto se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para desconectar el disco con el foco, escriba:
```
offline volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

