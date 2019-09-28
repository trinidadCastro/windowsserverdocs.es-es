---
title: memoria caché de bitsadmin y setexpirationtime
description: 'Temas de comandos de Windows para la **caché de bitsadmin y setexpirationtime** : establece la hora de expiración de la caché.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381923"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>memoria caché de bitsadmin y setexpirationtime
Establece la hora de expiración de la caché.
## <a name="syntax"></a>Sintaxis
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|segundos|El número de segundos hasta que expire la memoria caché.|
## <a name="BKMK_examples"></a>Example
En el ejemplo siguiente se expira la caché en 60 segundos.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
