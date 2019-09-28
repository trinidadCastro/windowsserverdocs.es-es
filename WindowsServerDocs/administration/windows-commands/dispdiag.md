---
title: dispdiag
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b640883a207648d2ef6c9a7d6e5366cd0bb384c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377759"
---
# <a name="dispdiag"></a>dispdiag



Los registros muestran información en un archivo.

## <a name="syntax"></a>Sintaxis

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|- testacpi|Ejecuta la prueba de diagnóstico de Hotkey. Muestra el nombre de la clave, el código y el código de examen de cualquier clave presionada durante la prueba.|
|-d|Genera un archivo de volcado de memoria con los resultados de las pruebas.|
|-Delay \<Seconds >|Retrasa la recopilación de datos según el tiempo especificado en *segundos*.|
|-out \<FilePath >|Especifica la ruta de acceso y el nombre de archivo para guardar los datos recopilados. Este debe ser el último parámetro.|
|-?|Muestra los parámetros de comando disponibles y proporciona ayuda para usarlos.|