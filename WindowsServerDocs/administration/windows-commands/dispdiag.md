---
title: dispdiag
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831466"
---
# <a name="dispdiag"></a>dispdiag



Muestra información en un archivo.

## <a name="syntax"></a>Sintaxis

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-testacpi|Ejecuta la prueba de diagnóstico de tecla de acceso rápido. Muestra el nombre de clave, código y análisis de código para cualquier tecla se presiona durante la prueba.|
|-d|Genera un archivo de volcado de memoria con los resultados de pruebas.|
|-delay \<segundos >|Retrasa la recopilación de datos de tiempo especificado en *segundos*.|
|-out \<FilePath >|Especifica la ruta de acceso y nombre de archivo para guardar los datos recopilados. Debe ser el último parámetro.|
|-?|Muestra los parámetros de los comandos disponibles y proporciona ayuda para su uso.|