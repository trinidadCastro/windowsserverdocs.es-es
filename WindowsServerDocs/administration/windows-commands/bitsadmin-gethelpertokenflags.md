---
title: bitsadmin gethelpertokenflags
description: Artículo de referencia para el comando bitsadmin gethelpertokenflags, que devuelve las marcas de uso de un token auxiliar que está asociado a un trabajo de transferencia de BITS.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d49a2aef829b3e443ad5e1fd3e70d8c260f59ec8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894242"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Devuelve las marcas de uso de un [token auxiliar](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)   que está asociado a un trabajo de transferencia de bits.

> [!NOTE]
> Este comando no es compatible con BITS 3,0 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

### <a name="remarks"></a>Observaciones

Posibles valores devueltos, entre los que se incluyen:

- **0x0001.** El token auxiliar se usa para abrir el archivo local de un trabajo de carga, para crear o cambiar el nombre del archivo temporal de un trabajo de descarga, o para crear o cambiar el nombre del archivo de respuesta de un trabajo de carga y respuesta.

- **0x0002.** El token auxiliar se usa para abrir el archivo remoto de un trabajo de carga o descarga de bloque de mensajes del servidor (SMB), o en respuesta a un desafío del servidor HTTP o del proxy para las credenciales NTLM o Kerberos implícitas. Debe llamar  `/SetCredentialsJob TargetScheme NULL NULL`   a para permitir que las credenciales se envíen a través de http.

## <a name="examples"></a>Ejemplos

Para recuperar las marcas de uso de un token auxiliar asociado a un trabajo de transferencia de BITS denominado *myDownloadJob*:

```
bitsadmin /gethelpertokenflags myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
