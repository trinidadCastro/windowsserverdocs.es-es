---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: reparación de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846686"
---
# <a name="fsutil-repair"></a>reparación de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra y supervisa las operaciones de reparación de recuperación automática de NTFS.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|Enumerar|Enumera las entradas de registro de daños de un volumen.|
|\<volumepath>|Especifica el volumen como el nombre de la unidad seguido de dos puntos.|
|\<LogName>|$Dañado: el conjunto de datos corruptos confirmadas en el volumen.<br />$Comprobar: un conjunto de datos corruptos posibles, no comprobados en el volumen.|
|iniciar|Inicia la recuperación automática de NTFS.|
|\<FileReference>|Especifica el identificador de archivo específico para cada volumen NTFS (número de referencia de archivo). La referencia de archivo incluye el número de segmentos del archivo.|
|query|Consulta el estado de recuperación automática del volumen NTFS.|
|set|Establece el estado de recuperación automática del volumen.|
|\<Flags>|Especifica el método de reparación que se usará al establecer el estado de recuperación automática del volumen.<br /><br />El **marcas** parámetro puede establecerse en tres valores:<br /><br />-   **0x01**: Habilita la reparación general.<br />-   **0x09**: Advierte sobre la posible pérdida de datos sin la reparación.<br />-   **0x00**: Deshabilita Autorrecuperación las operaciones de reparación NTFS.|
|Estado|Consulta el estado de daños del sistema o para un volumen determinado.|
|Espere|Espera a que reparaciones initiate en completarse. Si NTFS ha detectado un problema en un volumen en el que se realiza reparaciones, esta opción permite que el sistema debe esperar hasta que la reparación se complete antes de ejecutar cualquier script pendiente.|
|[WaitType {0&#124;1}]|Indica si se debe esperar la reparación actual para completar o espere a que todas las reparaciones en completarse. *WaitType* se puede establecer en los siguientes valores:<br /><br />-   **0**: Espera a que todas las reparaciones en completarse. (valor predeterminado)<br />-   **1**: Espera a que la reparación actual completar.|

## <a name="remarks"></a>Comentarios

-   Recuperación automática de NTFS intenta corregir los daños de en línea, el sistema de archivos NTFS sin necesidad de **Chkdsk.exe** para ejecutarse. Esta característica se introdujo en Windows Server 2008. Para obtener más información, consulte [NTFS de recuperación de autoservicio](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Ejemplos

Para enumerar los daños de un volumen confirmados, escriba:

```
fsutil repair enumerate C: $Corrupt 
```

Para habilitar la recuperación automática de reparación en la unidad C, escriba:

```
fsutil repair set c: 1
```

Para deshabilitar la recuperación automática de reparación en la unidad C, escriba:

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS de resolución automática](https://go.microsoft.com/fwlink/?LinkID=165401)


