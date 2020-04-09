---
title: comprobar
description: Comando comandos de Windows para comprobar, que indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91a0777999a604a23e2de83eda6b89c926cb241c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830058"
---
# <a name="verify"></a>comprobar



Indica a **cmd** si desea comprobar que los archivos se escriben correctamente en un disco. Si se usa sin parámetros, **Verify** muestra el valor actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
verify [on | off]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[on \| OFF]|Activa o desactiva la configuración de **comprobación** .|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar la configuración de **verificación** actual, escriba:
```
verify
```
Para activar la configuración de **comprobación** , escriba:
```
Verify on
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)