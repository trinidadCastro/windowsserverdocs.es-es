---
title: Mediante el comando get-AllServers
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853096"
---
# <a name="using-the-get-allservers-command"></a>Mediante el comando get-AllServers



Recupera información sobre todos los servidores de servicios de implementación de Windows.

> [!NOTE]
> Este comando puede tardar un período prolongado de tiempo en completarse si hay muchos servidores de servicios de implementación de Windows en su entorno o si la conexión de red los servidores de la vinculación es lenta.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ Mostrar: {Config | Imágenes | Todos los}|Especifica qué tipo de información que se va a devolver.</br>-   **Configuración** devuelve información de configuración del servidor.</br>-   **Imágenes** devuelve información acerca de los grupos de imágenes, las imágenes de arranque e imágenes de instalación en el servidor.</br>-   **Todos los** devuelve información de configuración y la imagen de servidor.|
|[/ Detallada]|Cuando se usa junto con el **/Show:Images** o **/Show:All**, devuelve todos los metadatos de cada imagen de la imagen. Si el **/detallada** opción no se especifica, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo.|
|[/ Forest: {Sí | No}]|Especifica si se devuelve información para todo el bosque o dominio local. Si no se especifica un valor para esta opción, el comportamiento predeterminado es devolver los servidores en el dominio local.|

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de todos los servidores, escriba:
```
WDSUTIL /Get-AllServers /Show:Config
```
Para ver información detallada sobre todos los servidores, escriba:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)