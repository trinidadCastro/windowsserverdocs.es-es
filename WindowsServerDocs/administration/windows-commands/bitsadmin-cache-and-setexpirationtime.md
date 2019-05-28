---
title: setexpirationtime y bitsadmin caché
description: Tema de los comandos de Windows para **bitsadmin caché y setexpirationtime** -establece la hora de expiración de caché.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e896df0a88c0cfc4eec07aba4807f184e7abe32
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008940"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>setexpirationtime y bitsadmin caché
Establece la hora de expiración de caché.
## <a name="syntax"></a>Sintaxis
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|en segundos|El número de segundos hasta que expire la memoria caché.|
## <a name="BKMK_examples"></a>Ejemplos
En el siguiente ejemplo expira la caché en 60 segundos.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
