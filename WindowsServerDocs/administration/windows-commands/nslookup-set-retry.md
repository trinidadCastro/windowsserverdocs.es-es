---
title: nslookup set retry
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63d72a45c33da099c5936d625b27aa71ef002280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857666"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el número de reintentos.
## <a name="syntax"></a>Sintaxis
```
set retry=<Number>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<Number>|Especifica el nuevo valor para el número de reintentos. El número predeterminado de reintentos es 4.|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   Cuando no se recibe una respuesta a una solicitud en un período de tiempo determinado, el período de tiempo de espera se duplica y se vuelve a enviar la solicitud. El valor de reintento controla cuántas veces se vuelve a enviar una solicitud antes de desistir. Puede cambiar el período de tiempo de espera con el **Establecer tiempo de espera** subcomando.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup establece tiempo de espera](nslookup-set-timeout.md)
