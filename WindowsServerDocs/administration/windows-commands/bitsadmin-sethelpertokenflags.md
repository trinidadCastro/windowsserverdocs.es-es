---
title: bitsadmin sethelpertokenflags
description: 'Windows Commands topic for **bitsadmin sethelpertokenflags** : establece las marcas de uso de un token auxiliar que está asociado a un trabajo de transferencia de bits.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 6047c63677fac3311634ababb675be5301b7f3b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380581"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

Establece las marcas de uso de un de [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) que está asociado a un trabajo de transferencia de bits.

**BITS 3,0 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar o el GUID del trabajo.|
|Flags|Entre los valores posibles se incluyen los siguientes. 0x0001&mdash;el token auxiliar se usa para abrir el archivo local de un trabajo de carga, para crear o cambiar el nombre del archivo temporal de un trabajo de descarga, o para crear o cambiar el nombre del archivo de respuesta de un trabajo de carga y respuesta. 0x0002&mdash;el token auxiliar se usa para abrir el archivo remoto de un trabajo de carga o descarga de bloque de mensajes del servidor (SMB), o en respuesta a un desafío del servidor HTTP o del proxy para las credenciales NTLM o Kerberos implícitas. Debe llamar a `/SetCredentialsJob TargetScheme NULL NULL` para permitir que las credenciales se envíen a través de HTTP.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
