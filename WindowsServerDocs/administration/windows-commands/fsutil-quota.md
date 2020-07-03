---
title: fsutil quota
description: Artículo de referencia del comando fsutil quota, que administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en red.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f757f822a903f6b5c6d221e17f87cf1e73d1555f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925225"
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
| `<volumepath>` | Obligatorio. Especifica el nombre de la unidad seguido de dos puntos o del GUID en el formato `volume{GUID}` . |
| `<threshold>`  | Establece el límite (en bytes) en el que se emiten las advertencias. Este parámetro es necesario para el `fsutil quota modify` comando. |
| `<limit>` | Establece el uso máximo permitido del disco (en bytes). Este parámetro es necesario para el `fsutil quota modify` comando. |
| `<username>` | Especifica el nombre de dominio o de usuario. Este parámetro es necesario para el `fsutil quota modify` comando. |

#### <a name="remarks"></a>Comentarios

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
