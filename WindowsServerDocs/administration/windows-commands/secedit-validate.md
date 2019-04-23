---
title: secedit:validate
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877916"
---
# <a name="seceditvalidate"></a>secedit:validate



Valida la configuración de seguridad almacenada en una plantilla de seguridad (archivo .inf). Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre de archivo de configuración|Obligatorio.</br>Especifica la ruta de acceso y el nombre de la plantilla de seguridad que se va a validar.|

## <a name="remarks"></a>Comentarios

Validación de plantillas de seguridad puede ayudarle si uno está dañado o establecido incorrectamente.

No se aplicará una plantilla de seguridad no válido.

No se actualizará el archivo de registro.

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información sobre cómo actualizar la configuración de seguridad, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Ejemplos

Una vez que se realiza una operación de reversión en una plantilla de seguridad, desea comprobar que el archivo inf de reversión, secRBKcontoso.inf, es válido.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)