---
title: bitsadmin sethelpertokenflags
description: Windows Commands topic for bitsadmin sethelpertokenflags, que establece las marcas de uso de un token auxiliar que está asociado a un trabajo de transferencia de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c644e82026cfc1d62f3fb5d20e3925002b871036
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849498"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

Establece las marcas de uso de un de [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) que está asociado a un trabajo de transferencia de bits.

**BITS 3,0 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar o el GUID del trabajo.|
|Flags|Entre los valores posibles se incluyen los siguientes. 0x0001&mdash;el token auxiliar se usa para abrir el archivo local de un trabajo de carga, para crear o cambiar el nombre del archivo temporal de un trabajo de descarga, o para crear o cambiar el nombre del archivo de respuesta de un trabajo de carga y respuesta. 0x0002&mdash;el token auxiliar se usa para abrir el archivo remoto de un trabajo de carga o descarga de bloque de mensajes del servidor (SMB), o en respuesta a un desafío del servidor HTTP o del proxy para las credenciales NTLM o Kerberos implícitas. Debe llamar a `/SetCredentialsJob TargetScheme NULL NULL` para permitir que las credenciales se envíen a través de HTTP.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
