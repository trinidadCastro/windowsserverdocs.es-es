---
title: Usar como destino una versión diferente del SDK del centro de administración de Windows
description: Usar como destino una versión diferente del SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 3d6b08e69d69a37b31b616994b3bdb67666cb2bb
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766938"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Usar como destino una versión diferente del SDK del centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Es fácil mantener actualizada la extensión con los cambios de SDK y los cambios de plataforma.  Usamos [etiquetas de NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar la versión de las nuevas características en las versiones del SDK.

Hay tres versiones de SDK entre las que puede elegir:

* ```latest``` : este paquete del SDK se alinea con la versión de disponibilidad general actual del centro de administración de Windows
* ```insider``` : este paquete del SDK se alinea con la versión preliminar actual del centro de administración de Windows (disponible en Windows Server Insider Preview).
* ```next``` : este paquete de SDK contiene la funcionalidad más reciente

> [!NOTE]
> Obtenga más información sobre las diferentes [versiones](../overview.md) del centro de administración de Windows que se pueden descargar.

## <a name="targeting-sdk-version-on-a-new-project"></a>Versión del SDK de destino en un proyecto nuevo

Al crear una nueva extensión, puede incluir el ```--version``` parámetro para tener como destino una versión diferente del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Value | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | El nombre de su empresa (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |
| ```{!version}``` | Versión del SDK | ```latest``` |

A continuación se muestra un ejemplo de cómo crear una nueva extensión con destino ```insider``` :

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Versión del SDK de destino en un proyecto existente

Para modificar un proyecto existente para que tenga como destino otra versión del SDK, modifique la línea siguiente en ```package.json``` :

```
"@microsoft/windows-admin-center-sdk": "latest",
```
En este ejemplo, reemplace ```latest``` por la versión del SDK que desee, es decir ```insider``` :

```
"@microsoft/windows-admin-center-sdk": "insider",
```

A continuación, ejecute ```npm install``` para actualizar las referencias en todo el proyecto.