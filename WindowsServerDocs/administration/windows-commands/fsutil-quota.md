---
title: fsutil quota
description: Artículo de referencia del comando fsutil quota, que administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en red.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: e59885b3cf2b51b8d62446a6f1e4ab2ff3b90b3f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037353"
---
# <a name="fsutil-quota"></a>fsutil quota

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en red.

## <a name="syntax"></a>Sintaxis

```
fsutil quota [disable] <volumepath>
fsutil quota [enforce] <volumepath>
fsutil quota [modify] <volumepath> <threshold> <limit> <username>
fsutil quota [query] <volumepath>
fsutil quota [track] <volumepath>
fsutil quota [violations]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| disable | Deshabilita el seguimiento y la aplicación de cuotas en el volumen especificado. |
| pone | Aplica el uso de cuotas en el volumen especificado. |
| modify | Modifica una cuota de disco existente o crea una nueva. |
| Query | Muestra las cuotas de disco existentes. |
| track | Realiza el seguimiento del uso del disco en el volumen especificado. |
| dujeron | Busca en los registros del sistema y de la aplicación, y muestra un mensaje para indicar que se han detectado infracciones de cuota o que un usuario ha alcanzado un umbral de cuota o un límite de cuota. |
| `<volumepath>` | Necesario. Especifica el nombre de la unidad seguido de dos puntos o del GUID en el formato `volume{GUID}` . |
| `<threshold>`  | Establece el límite (en bytes) en el que se emiten las advertencias. Este parámetro es necesario para el `fsutil quota modify` comando. |
| `<limit>` | Establece el uso máximo permitido del disco (en bytes). Este parámetro es necesario para el `fsutil quota modify` comando. |
| `<username>` | Especifica el nombre de dominio o de usuario. Este parámetro es necesario para el `fsutil quota modify` comando. |

#### <a name="remarks"></a>Observaciones

- Las cuotas de disco se implementan por volumen y permiten que se implementen límites de almacenamiento tanto de forma rígida como por usuario.

- Puede usar scripts de escritura que usan **fsutil quota** para establecer los límites de cuota cada vez que se agrega un nuevo usuario o para realizar un seguimiento automático de los límites de cuota, compilarlos en un informe y enviarlos automáticamente al administrador del sistema por correo electrónico.

### <a name="examples"></a>Ejemplos

Para enumerar las cuotas de disco existentes para un volumen de disco que se especifica con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil quota query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Para enumerar las cuotas de disco existentes para un volumen de disco que se especifica con la letra de unidad **C:**, escriba:

```
fsutil quota query C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
