---
title: fsutil repair
description: Artículo de referencia para el comando fsutil repair, que administra y supervisa las operaciones de reparación de recuperación automática de NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 9c26f2be8e586205271ba1d150bb2378cc8b2045
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037333"
---
# <a name="fsutil-repair"></a>fsutil repair

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra y supervisa las operaciones de reparación de recuperación automática de NTFS. NTFS de recuperación automática intenta corregir los daños del sistema de archivos NTFS en línea, sin necesidad de que se ejecute **Chkdsk.exe** . Para obtener más información, consulte [NTFS de recuperación automática](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10)).

## <a name="syntax"></a>Sintaxis

```
fsutil repair [enumerate] <volumepath> [<logname>]
fsutil repair [initiate] <volumepath> <filereference>
fsutil repair [query] <volumepath>
fsutil repair [set] <volumepath> <flags>
fsutil repair [wait][<waittype>] <volumepath>

```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| enumerar | Enumera los enteros del registro de daños de un volumen. |
| `<logname>` | Puede ser `$corrupt` , el conjunto de daños confirmados en el volumen o `$verify` , un conjunto de posibles daños no comprobados en el volumen. |
| lación | Inicia la recuperación automática de NTFS. |
| `<filereference>` | Especifica el identificador de archivo específico del volumen NTFS (número de referencia de archivo). La referencia de archivo incluye el número de segmento del archivo. |
| Query | Consulta el estado de recuperación automática del volumen NTFS. |
| set | Establece el estado de recuperación automática del volumen. |
| `<flags>` | Especifica el método de reparación que se va a usar al establecer el estado de recuperación automática del volumen.<p>Este parámetro se puede establecer en tres valores:<ul><li>**0x01** : habilita la reparación general.</li><li>**0x09** : advierte sobre la posible pérdida de datos sin reparación.</li><li>**0x00** : deshabilita las operaciones de reparación de recuperación automática de NTFS.</li></ul> |
| state | Consulta el estado de daños del sistema o de un volumen determinado. |
| wait | Espera a que se completen las reparaciones. Si NTFS ha detectado un problema en un volumen en el que está realizando reparaciones, esta opción permite que el sistema espere hasta que se complete la reparación antes de ejecutar los scripts pendientes. |
| `[waittype {0|1}]` | Indica si se debe esperar a que se complete la reparación actual o esperar a que se completen todas las reparaciones. El parámetro *waittype* se puede establecer en los valores siguientes:<ul><li>**0** : espera a que se completen todas las reparaciones. (valor predeterminado)</li><li>**1** : espera a que se complete la reparación actual.</li></ul> |

### <a name="examples"></a>Ejemplos

Para enumerar los daños confirmados de un volumen, escriba:

```
fsutil repair enumerate C: $Corrupt
```

Para habilitar la reparación de recuperación automática en la unidad C, escriba:

```
fsutil repair set c: 1
```

Para deshabilitar la reparación de la recuperación automática en la unidad C, escriba:

```
fsutil repair set c: 0
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS de recuperación automática](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10))
