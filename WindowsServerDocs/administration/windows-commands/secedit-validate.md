---
title: 'secedit: Validate'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9425f7a1fb821f4ecbaa7c1689c3baabbff6223
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834878"
---
# <a name="seceditvalidate"></a>secedit: Validate



Valida la configuración de seguridad almacenada en una plantilla de seguridad (archivo. inf). Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre del archivo de configuración|Obligatorio.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se validará.|

## <a name="remarks"></a>Comentarios

La validación de plantillas de seguridad puede ayudarle si una está dañada o configurada de forma inapropiada.

No se aplicará una plantilla de seguridad no válida.

El archivo de registro no se actualizará.

En Windows Server 2008, `Secedit /refreshpolicy` se ha reemplazado por `gpupdate`. Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Después de realizar una reversión en una plantilla de seguridad, es conveniente comprobar que el archivo INF de reversión, secRBKcontoso. inf, es válido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referencias adicionales

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)