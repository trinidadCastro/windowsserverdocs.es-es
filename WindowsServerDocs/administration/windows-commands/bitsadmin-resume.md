---
title: bitsadmin resume
description: Tema de los comandos de Windows para **bitsadmin reanudar** -activa un trabajo nuevo o suspendido en la cola de transferencia.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842036"
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
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente reanuda el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)