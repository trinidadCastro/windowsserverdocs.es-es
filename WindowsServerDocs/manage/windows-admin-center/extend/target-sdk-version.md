---
title: Usar como destino una versión diferente del SDK del centro de administración de Windows
description: Usar como destino una versión diferente del SDK del centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0d3b7af5229f7b8487aa9f04eaf0d1756d8c02f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356967"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Usar como destino una versión diferente del SDK del centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Es fácil mantener actualizada la extensión con los cambios de SDK y los cambios de plataforma.  Usamos [etiquetas de NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar la versión de las nuevas características en las versiones del SDK.

Hay tres versiones de SDK entre las que puede elegir:

* ```latest```: este paquete del SDK se alinea con la versión de disponibilidad general actual del centro de administración de Windows
* ```insider```: este paquete del SDK se alinea con la versión preliminar actual del centro de administración de Windows (disponible en Windows Server Insider Preview)
* ```next```: este paquete de SDK contiene la funcionalidad más reciente

> [!NOTE]
> Obtenga más información sobre las diferentes [versiones](https://aka.ms/WACDownloadPage) del centro de administración de Windows que se pueden descargar.

## <a name="targeting-sdk-version-on-a-new-project"></a>Versión del SDK de destino en un proyecto nuevo

Al crear una nueva extensión, puede incluir el parámetro ```--version``` para tener como destino una versión diferente del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | El nombre de su empresa (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |
| ```{!version}``` | Versión del SDK | ```latest``` |

Este es un ejemplo en el que se crea una nueva extensión que tiene como destino ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Versión del SDK de destino en un proyecto existente

Para modificar un proyecto existente para que tenga como destino otra versión del SDK, modifique la siguiente línea en ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
En este ejemplo, reemplace ```latest``` por la versión del SDK que desee, es decir, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

A continuación, ejecute ```npm install``` para actualizar las referencias en todo el proyecto.