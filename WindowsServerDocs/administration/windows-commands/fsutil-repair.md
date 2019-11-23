---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: Fsutil repair
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6e4e285bf8401d628f7e4bcbaeafb0c6defa3ad1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376909"
---
# <a name="fsutil-repair"></a>Fsutil repair
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
|enumerar|Enumera los enteros del registro de daños de un volumen.|
|\<VolumePath >|Especifica el volumen como el nombre de la unidad seguido de dos puntos.|
|\<LogName >|$Corrupt: el conjunto de daños confirmados en el volumen.<br />$Verify: un conjunto de posibles daños no comprobados en el volumen.|
|lación|Inicia la recuperación automática de NTFS.|
|\<FileReference >|Especifica el identificador de archivo específico del volumen NTFS (número de referencia de archivo). La referencia de archivo incluye el número de segmento del archivo.|
|query|Consulta el estado de recuperación automática del volumen NTFS.|
|set|Establece el estado de recuperación automática del volumen.|
|\<marcas >|Especifica el método de reparación que se va a usar al establecer el estado de recuperación automática del volumen.<br /><br />El parámetro **Flags** se puede establecer en tres valores:<br /><br />-   **0x01**: habilita la reparación general.<br />-   **0x09**: advierte sobre la posible pérdida de datos sin reparación.<br />-   **0x00**: deshabilita las operaciones de reparación de recuperación automática de NTFS.|
|State|Consulta el estado de daños del sistema o de un volumen determinado.|
|currir|Espera a que se completen las reparaciones. Si NTFS ha detectado un problema en un volumen en el que está realizando reparaciones, esta opción permite que el sistema espere hasta que se complete la reparación antes de ejecutar los scripts pendientes.|
|[WaitType {0&#124;1}]|Indica si se debe esperar a que se complete la reparación actual o esperar a que se completen todas las reparaciones. *WaitType* se puede establecer en los siguientes valores:<br /><br />-   **0**: espera a que se completen todas las reparaciones. (valor predeterminado)<br />-   **1**: espera a que se complete la reparación actual.|

## <a name="remarks"></a>Observaciones

-   NTFS de recuperación automática intenta corregir los daños del sistema de archivos NTFS en línea, sin necesidad de ejecutar **CHKDSK. exe** . Esta característica se presentó en Windows Server 2008. Para obtener más información, vea [recuperación automática de NTFS](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Recuperación automática de NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


