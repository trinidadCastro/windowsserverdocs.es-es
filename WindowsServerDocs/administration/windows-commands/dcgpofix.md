---
title: dcgpofix
description: El tema comandos de Windows para Dcgpofix, que vuelve a crear los objetos de directiva de grupo predeterminados (GPO) para un dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1532d4c8c0266c92b4efce57a6744552a54e51b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846708"
---
# <a name="dcgpofix"></a>dcgpofix

Vuelve a crear los objetos de directiva de grupo predeterminados (GPO) para un dominio. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

#### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                 Descripción                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | Omite la versión del Active Directory del esquema de®.</br>al ejecutar este comando. De lo contrario, el comando solo funciona en la misma versión de esquema que la versión de Windows en la que se envió el comando. |
| /Target {dominio |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    Muestra la Ayuda en el símbolo del sistema.                                                                                     |

## <a name="remarks"></a>Comentarios

-   El comando **Dcgpofix** está disponible en windows Server 2008 R2 y windows Server 2008, excepto en las instalaciones Server Core.
-   Aunque el Consola de administración de directivas de grupo (GPMC) se distribuye con Windows Server 2008 R2 y Windows Server 2008, debe instalar la administración de directiva de grupo como una característica a través de Administrador del servidor.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Restaure el GPO de la Directiva de dominio predeterminada a su estado original. Perderá los cambios que haya realizado en este GPO. Como práctica recomendada, debe configurar el GPO de directiva de dominio predeterminada únicamente para administrar la configuración de directivas de cuenta predeterminada, la Directiva de contraseñas, la Directiva de bloqueo de cuentas y la Directiva de Kerberos. En este ejemplo, se omite la versión del esquema de Active Directory para que el comando **Dcgpofix** no esté limitado al mismo esquema que la versión de Windows en la que se envió el comando.
```
dcgpofix /ignoreschema /target:Domain
```
Restaure el GPO de la directiva predeterminada de controladores de dominio a su estado original. Perderá los cambios que haya realizado en este GPO. Como práctica recomendada, debe configurar el GPO de directiva de controladores de dominio predeterminados únicamente para establecer derechos de usuario y directivas de auditoría. En este ejemplo, se omite la versión del esquema de Active Directory para que el comando **Dcgpofix** no esté limitado al mismo esquema que la versión de Windows en la que se envió el comando.
```
dcgpofix /ignoreschema /target:DC
```

## <a name="additional-references"></a>Referencias adicionales

-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)