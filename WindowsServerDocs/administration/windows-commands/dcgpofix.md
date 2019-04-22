---
title: dcgpofix
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91fceb429ca00b1b3d9d36d01f5e97cfd464ccb9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825166"
---
# <a name="dcgpofix"></a>dcgpofix



Vuelve a crear los objetos de directiva de grupo (GPO) predeterminado para un dominio. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ignoreschema|Omite la versión de la consola de administración de esquema Active Directory®</br>al ejecutar este comando. En caso contrario, el comando sólo funciona en la misma versión de esquema que la versión de Windows en el que se envió el comando.|
|/ Target {nombre de dominio | DC | Ambos}|Especifica qué GPO para restaurar. Puede restaurar el GPO de directiva de dominio predeterminado, el GPO de controladores de dominio predeterminado o ambos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **dcgpofix** comando está disponible en Windows Server 2008 R2 y Windows Server 2008, excepto en las instalaciones Server Core.
-   Aunque la consola de administración de directivas de grupo (GPMC) se distribuye con Windows Server 2008 R2 y Windows Server 2008, debe instalar Administración de directivas de grupo como una característica mediante el administrador del servidor.

## <a name="BKMK_Examples"></a>Ejemplos

Restaurar el GPO de directiva predeterminada de dominio a su estado original. Perderá los cambios realizados a este GPO. Como práctica recomendada, debe configurar el GPO del dominio predeterminado solo para administrar la configuración de directivas de cuenta predeterminada, directiva de contraseñas, directiva de bloqueo de cuenta y la directiva Kerberos. En este ejemplo, omite la versión de esquema de Active Directory para que la **dcgpofix** comando no se limita al mismo esquema que la versión de Windows en el que se envió el comando.
```
dcgpofix /ignoreschema /target:Domain
```
Restaurar el GPO de directiva de controladores de dominio predeterminados a su estado original. Perderá los cambios realizados a este GPO. Como práctica recomendada, debe configurar el dominio GPO controladores predeterminados directiva sólo para establecer los derechos de usuario y directivas de auditoría. En este ejemplo, omite la versión de esquema de Active Directory para que la **dcgpofix** comando no se limita al mismo esquema que la versión de Windows en el que se envió el comando.
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>Referencias adicionales

-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)