---
title: dcgpofix
description: Artículo de referencia para el comando dcgpofix, que vuelve a crear los objetos de directiva de grupo predeterminados (GPO) para un dominio.
ms.topic: reference
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9c669181fcf7aec73e038a9a211959cbc5879b1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028433"
---
# <a name="dcgpofix"></a>dcgpofix

Vuelve a crear los objetos de directiva de grupo predeterminados (GPO) para un dominio. Para llegar al Consola de administración de directivas de grupo (GPMC), debe instalar la administración de directiva de grupo como una característica a través de Administrador del servidor.

>[!IMPORTANT]
> Como práctica recomendada, debe configurar el GPO de directiva de dominio predeterminada únicamente para administrar la configuración de **directivas de cuenta** predeterminada, la Directiva de contraseñas, la Directiva de bloqueo de cuentas y la Directiva de Kerberos. Además, debe configurar el GPO de directiva de controladores de dominio predeterminados únicamente para establecer derechos de usuario y directivas de auditoría.

## <a name="syntax"></a>Sintaxis

```
dcgpofix [/ignoreschema] [/target: {domain | dc | both}] [/?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /ignoreschema | Omite la versión del esquema de Active Directory cuando se ejecuta este comando. De lo contrario, el comando solo funciona en la misma versión de esquema que la versión de Windows en la que se envió el comando. |
| `/target {domain | dc | both` | Especifica si se va a establecer como destino la Directiva de dominio predeterminada, la directiva predeterminada de controladores de dominio o ambos tipos de directivas. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para administrar la configuración de **las directivas de cuentas** predeterminadas, la Directiva de contraseñas, la Directiva de bloqueo de cuentas y la Directiva de Kerberos, y omitir la versión de esquema Active Directory, escriba:

```
dcgpofix /ignoreschema /target:domain
```

Para configurar el GPO de directiva de controladores de dominio predeterminados únicamente para establecer derechos de usuario y directivas de auditoría, y omitir la versión del esquema de Active Directory, escriba:

```
dcgpofix /ignoreschema /target:dc
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)