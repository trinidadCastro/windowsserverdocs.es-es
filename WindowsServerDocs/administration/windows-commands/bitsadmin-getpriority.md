---
title: bitsadmin GetPriority (
description: Tema de comandos de Windows para **bitsadmin GetPriority (** , que recupera la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b27829a0fb852abb88c88a65e61e8d7693ca2df2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850548"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin GetPriority (

Recupera la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="remarks"></a>Comentarios

La prioridad de este comando puede ser:

- **PLANOS**

- **CALIDAD**

- **SIN**

- **HABILITA**

- **UNKNOWN**

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la prioridad del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
