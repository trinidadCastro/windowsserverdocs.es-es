---
title: disco en línea
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c61d852ba71329c3d7345d74fd352a6c19436cec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837898"
---
# <a name="online-disk"></a>disco en línea



Traslada los discos que están actualmente sin conexión a un estado en línea.

> [!IMPORTANT]
> Este comando no está disponible en ninguna edición de Windows Vista.

> [!IMPORTANT]
> Este comando producirá un error si se usa en un disco de solo lectura.

Para obtener instrucciones sobre cómo usar este comando, consulte [reactivación de un disco dinámico que falta o que está sin conexión](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintaxis

```
online disk [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Cuando se usa sin parámetros en Windows Vista, este comando funciona en un grupo de discos. En el caso de los discos básicos, nunca hay más de un disco por grupo. En el caso de los discos dinámicos, el grupo incluye todos los discos dinámicos no externos.
-   En el caso de los discos básicos, este comando intentará poner en conexión el disco seleccionado y todos los volúmenes del disco.
-   En el caso de los discos dinámicos, este comando intentará conectar todos los discos que no estén marcados como externos en el equipo local. También intentará poner en conexión todos los volúmenes en el conjunto de discos dinámicos.
-   Si se conecta un disco dinámico de un grupo de discos y es el único disco del grupo, se vuelve a crear el grupo original y el disco se mueve a ese grupo. Si hay otros discos en el grupo y están en línea, el disco simplemente se vuelve a agregar al grupo.
-   Si el grupo de un disco seleccionado contiene volúmenes reflejados o RAID-5, este comando también vuelve a sincronizar estos volúmenes.
-   Se debe seleccionar un disco para que este comando se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para poner en línea el disco que tiene el foco, escriba:
```
online disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

