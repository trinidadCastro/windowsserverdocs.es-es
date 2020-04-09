---
title: bitsadmin complete
description: Tema de comandos de Windows para **bitsadmin completado**, que completa el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 847f6e298ff9701064ce4e577c785f7fc78ea22c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850828"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa el trabajo. Los archivos descargados no estarán disponibles hasta que use este modificador. Use este modificador después de que el trabajo se mueva al estado transferido. De lo contrario, solo estarán disponibles los archivos que se hayan transferido correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Cuando se transfiere el estado del trabajo, BITS ha transferido correctamente todos los archivos del trabajo. Sin embargo, los archivos no estarán disponibles hasta que use el modificador **/Complete** 

Si varios trabajos usan *myDownloadJob* como su nombre, debe reemplazar *MYDOWNLOADJOB* por el GUID del trabajo para identificar el trabajo de forma única.

```
C:\>bitsadmin /complete myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)