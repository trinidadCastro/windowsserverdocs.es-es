---
title: bitsadmin sethelpertokenflags
description: Artículo de referencia para el comando bitsadmin sethelpertokenflags, que establece las marcas de uso de un token auxiliar que está asociado a un trabajo de transferencia de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d5fc4c1d78fdf7ea6a6bb67cf435c8e71d5a9cbc
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927779"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

Establece las marcas de uso de un [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)   que está asociado a un trabajo de transferencia de bits.

> [!NOTE]
> Este comando no es compatible con BITS 3,0 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| flags | Posibles valores de token de la aplicación auxiliar, incluidos:<ul><li>**0x0001.** Se usa para abrir el archivo local de un trabajo de carga, para crear o cambiar el nombre del archivo temporal de un trabajo de descarga, o para crear o cambiar el nombre del archivo de respuesta de un trabajo de carga y respuesta.</li><li>**0x0002.** Se usa para abrir el archivo remoto de un trabajo de carga o descarga de bloque de mensajes del servidor (SMB), o en respuesta a un desafío del servidor HTTP o del proxy para las credenciales NTLM o Kerberos implícitas.</li></ul>Debe llamar  `/setcredentialsjob targetscheme null null`   a para enviar las credenciales a través de http. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
