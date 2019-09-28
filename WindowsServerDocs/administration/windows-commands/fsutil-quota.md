---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Cuota de fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 5e1c6793ca866ecacd8b00aa7e01d632c2538405
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376845"
---
# <a name="fsutil-quota"></a>Cuota de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en red.

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
|    deshabilitar    |                                                         Deshabilita el seguimiento y la aplicación de cuotas en el volumen especificado.                                                          |
|    pone    |                                                                   Aplica el uso de cuotas en el volumen especificado.                                                                   |
|    modify     |                                                              Modifica una cuota de disco existente o crea una nueva.                                                              |
|     query     |                                                                            Muestra las cuotas de disco existentes.                                                                            |
|     dar     |                                                                    Realiza el seguimiento del uso del disco en el volumen especificado.                                                                     |
|  Dujeron   | Busca en los registros del sistema y de la aplicación, y muestra un mensaje para indicar que se han detectado infracciones de cuota o que un usuario ha alcanzado un umbral de cuota o un límite de cuota. |
| @no__t 0VolumePath > |                                  Obligatorio. Especifica el nombre de la unidad seguido de dos puntos o el GUID en el formato **volumen {** <em>GUID</em> **}** .                                  |
| @no__t 0Threshold >  |                            Establece el límite (en bytes) en el que se emiten las advertencias. Este parámetro es necesario para el comando **fsutil quota Modify** .                            |
|   @no__t 0Limit >    |                                Establece el uso máximo permitido del disco (en bytes). Este parámetro es necesario para el comando **fsutil quota Modify** .                                |
|  @no__t 0UserName >  |                                      Especifica el nombre de dominio o de usuario. Este parámetro es necesario para el comando **fsutil quota Modify** .                                       |

## <a name="remarks"></a>Comentarios

-   Las cuotas de disco se implementan por volumen y permiten que se implementen límites de almacenamiento tanto de forma rígida como por usuario.

-   Puede usar scripts de escritura que usan **fsutil quota** para establecer los límites de cuota cada vez que se agrega un nuevo usuario o para realizar un seguimiento automático de los límites de cuota, compilarlos en un informe y enviarlos automáticamente al administrador del sistema por correo electrónico.

### <a name="BKMK_examples"></a>Example
Para enumerar las cuotas de disco existentes para un volumen de disco que se especifica con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Para enumerar las cuotas de disco existentes para un volumen de disco que se especifica con la letra de unidad **C:** , escriba:

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


