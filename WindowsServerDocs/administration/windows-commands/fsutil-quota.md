---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: cuota de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 1e844d73348ee31f309f44895831ded9a2e6365c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439062"
---
# <a name="fsutil-quota"></a>cuota de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en la red.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                    Descripción                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    deshabilitar    |                                                         Deshabilita el seguimiento de cuotas y el cumplimiento en el volumen especificado.                                                          |
|    Aplicar    |                                                                   Exige el uso de cuota en el volumen especificado.                                                                   |
|    modify     |                                                              Modifica una cuota de disco existente o crea una nueva cuota.                                                              |
|     query     |                                                                            Enumera los existentes a las cuotas de disco.                                                                            |
|     Seguimiento     |                                                                    Pistas de uso del disco en el volumen especificado.                                                                     |
|  Infracciones   | Busca en los registros de aplicación y del sistema y muestra un mensaje para indicar que se detectaron infracciones de cuota o que un usuario ha alcanzado un límite de cuota o el umbral de cuota. |
| \<VolumePath> |                                  Obligatorio. Especifica el nombre de la unidad seguido de dos puntos o el GUID en formato **volumen {** <em>GUID</em> **}** .                                  |
| \<Umbral >  |                            Establece el límite (en bytes) en el que se emiten advertencias. Este parámetro es obligatorio para la **modificar la cuota de fsutil** comando.                            |
|   \<Límite >    |                                Establece el uso de disco máxima permitida (en bytes). Este parámetro es obligatorio para la **modificar la cuota de fsutil** comando.                                |
|  \<UserName>  |                                      Especifica el nombre de dominio o usuario. Este parámetro es obligatorio para la **modificar la cuota de fsutil** comando.                                       |

## <a name="remarks"></a>Comentarios

-   Las cuotas de disco se implementan en por volumen, y permiten que los límites de almacenamiento flexibles y forzados a implementarse en por usuario.

-   Puede usar escribir scripts que usen **fsutil cuota** para establecer los límites de cuota cada vez que agregue un nuevo usuario o automáticamente el seguimiento de los límites de cuota, compilarlos en un informe y enviarlos de forma automática al administrador del sistema de correo electrónico.

### <a name="BKMK_examples"></a>Ejemplos
Para obtener una lista existente de las cuotas de disco para un volumen de disco que se especifica con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Para enumerar las cuotas de disco existentes para un volumen de disco que se especifica con la letra de unidad, **C:** , tipo:

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


