---
title: 'secedit: Validate'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b93ad6ceadb08f6df8390edc3fc454d951519aad
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821105"
---
# <a name="seceditvalidate"></a>secedit: Validate



Valida la configuración de seguridad almacenada en una plantilla de seguridad (archivo. inf).

## <a name="syntax"></a>Sintaxis

```
Secedit /validate <configuration file name>

```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Nombre del archivo de configuración|Necesario.</br>Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se validará.|

## <a name="remarks"></a>Observaciones

La validación de plantillas de seguridad puede ayudarle si una está dañada o configurada de forma inapropiada.

No se aplicará una plantilla de seguridad no válida.

El archivo de registro no se actualizará.

En Windows Server 2008, se ha `Secedit /refreshpolicy` reemplazado por `gpupdate` . Para obtener información acerca de cómo actualizar la configuración de seguridad, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a>Ejemplos

Después de realizar una reversión en una plantilla de seguridad, es conveniente comprobar que el archivo INF de reversión, secRBKcontoso. inf, es válido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referencias adicionales

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)