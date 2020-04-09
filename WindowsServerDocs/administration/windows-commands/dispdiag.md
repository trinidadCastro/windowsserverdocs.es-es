---
title: dispdiag
description: Temas de comandos de Windows para dispdiag, que registran información en un archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 498294aa6678cc4904880128bca55d7900c91db5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845388"
---
# <a name="dispdiag"></a>dispdiag

Los registros muestran información en un archivo.

## <a name="syntax"></a>Sintaxis

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|- testacpi|Ejecuta la prueba de diagnóstico de Hotkey. Muestra el nombre de la clave, el código y el código de examen de cualquier clave presionada durante la prueba.|
|-d|Genera un archivo de volcado de memoria con los resultados de las pruebas.|
|-Delay \<segundos >|Retrasa la recopilación de datos según el tiempo especificado en *segundos*.|
|-out \<FilePath >|Especifica la ruta de acceso y el nombre de archivo para guardar los datos recopilados. Este debe ser el último parámetro.|
|-?|Muestra los parámetros de comando disponibles y proporciona ayuda para usarlos.|