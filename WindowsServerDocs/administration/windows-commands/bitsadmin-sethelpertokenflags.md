---
title: Bitsadmin sethelpertokenflags
description: Tema de los comandos de Windows para **sethelpertokenflags bitsadmin** -establece las marcas de uso de un token auxiliar que está asociado con un trabajo de transferencia de BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: cc9652afe73476041aa42e64671885bfc1af9628
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813866"
---
# <a name="bitsadmin-sethelpertokenflags"></a>Bitsadmin sethelpertokenflags

Establece las marcas de uso para un [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) que está asociado con un trabajo de transferencia de BITS.

**3.0 y versiones anteriores de BITS**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo.|
|Flags|Los valores posibles son las siguientes. 0 x 0001&mdash;el token auxiliar se utiliza para abrir el archivo local de un trabajo de carga para crear o cambiar el nombre del archivo temporal de un trabajo de descarga, o para crear o cambiar el nombre del archivo de respuesta de un trabajo de respuesta de carga. 0 x 0002&mdash;el token auxiliar se utiliza para abrir el archivo de carga de un bloque de mensajes del servidor (SMB) remoto o descargar el trabajo o en respuesta a un desafío de servidor o proxy HTTP para implícita NTLM o Kerberos credenciales. Debe llamar a `/SetCredentialsJob TargetScheme NULL NULL` para permitir que las credenciales que se enviarán a través de HTTP.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
