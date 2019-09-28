---
title: bitsadmin resume
description: 'Tema de comandos de Windows para la **reanudación de bitsadmin** : activa un trabajo nuevo o suspendido en la cola de transferencia.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380772"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



Activa un trabajo nuevo o suspendido en la cola de transferencia.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se reanuda el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)