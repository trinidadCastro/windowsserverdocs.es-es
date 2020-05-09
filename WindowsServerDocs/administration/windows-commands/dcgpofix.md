---
title: dcgpofix
description: Tema de referencia del comando dcgpofix, que vuelve a crear los objetos de directiva de grupo predeterminados (GPO) para un dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f11b7db8110cd2d7dcf08cd250eba411e7ff21a8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993165"
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