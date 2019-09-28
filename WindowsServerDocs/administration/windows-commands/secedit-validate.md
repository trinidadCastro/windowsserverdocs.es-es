---
title: 'secedit: Validate'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece0a0324b77eb4226b679bc29f7bd599f15a120
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371102"
---
# <a name="seceditvalidate"></a>secedit: Validate



Valida la configuración de seguridad almacenada en una plantilla de seguridad (archivo. inf). Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre del archivo de configuración|Obligatorio.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se validará.|

## <a name="remarks"></a>Comentarios

La validación de plantillas de seguridad puede ayudarle si una está dañada o configurada de forma inapropiada.

No se aplicará una plantilla de seguridad no válida.

El archivo de registro no se actualizará.

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado `gpupdate`por. Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Example

Después de realizar una reversión en una plantilla de seguridad, es conveniente comprobar que el archivo INF de reversión, secRBKcontoso. inf, es válido.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)