---
title: select volume
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2737a25eb9095b70fd6939a4f38b751868323f3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024989"
---
# <a name="select-volume"></a>select volume

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

selecciona el volumen especificado y desplaza el foco a él. Este comando también se puede usar para mostrar el volumen que tiene actualmente el foco en el disco seleccionado.



## <a name="syntax"></a>Sintaxis

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>Parámetros

| Parámetro |                                                                               Descripción                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | Número del volumen en el que se va a recibir el foco. Puede ver los números de todos los volúmenes del disco seleccionado actualmente mediante el comando **List Volume** en Diskpart. |
|    <d>    |                                                 La letra de unidad o la ruta de acceso del punto de montaje del volumen que va a recibir el foco.                                                 |

## <a name="remarks"></a>Observaciones

-   Si no se especifica ningún volumen, este comando muestra el volumen que tiene actualmente el foco en el disco seleccionado.

-   En un disco básico, la selección de un volumen también proporciona el foco a la partición correspondiente.

-   Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.

-   Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.

## <a name="examples"></a>Ejemplos
Para desplazar el foco al volumen 2, escriba:

```
select volume=2
```

Para desplazar el foco a la unidad C, escriba:

```
select volume=c
```

Para desplazar el foco al volumen montado en una carpeta denominada mountpath, escriba:

```
select volume=c:\mountpath
```

Para mostrar el volumen que tiene actualmente el foco en el disco seleccionado, escriba:

```
select volume
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)




