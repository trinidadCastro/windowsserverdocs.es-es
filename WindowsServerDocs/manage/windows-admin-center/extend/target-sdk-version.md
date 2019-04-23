---
title: Tener como destino una versión diferente de los SDK de Windows Admin Center
description: Tener como destino una versión diferente de los SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833626"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Tener como destino una versión diferente de los SDK de Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Es fácil mantener la extensión actualizada con los cambios SDK y plataforma.  Usamos [NPM etiquetas](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar el lanzamiento de nuevas características en las versiones del SDK.

Hay tres versiones del SDK que puede elegir entre:

* ```latest``` : este paquete del SDK se alinea con la versión actual de la disponibilidad general de Windows Admin Center
* ```insider``` : este paquete del SDK se alinea con la versión de vista previa actual de Windows Admin Center (disponible en Windows Server Insider Preview)
* ```next``` : este paquete SDK contiene la funcionalidad más reciente

> [!NOTE]
> Encontrará más información sobre los diferentes [versiones](https://aka.ms/WACDownloadPage) de Windows Admin Center que están disponibles para su descarga.

## <a name="targeting-sdk-version-on-a-new-project"></a>Destinado a la versión SDK en un nuevo proyecto

Al crear una nueva extensión, puede incluir el ```--version``` parámetro como destino una versión diferente del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de su compañía (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |
| ```{!version}``` | Versión del SDK | ```latest``` |

Este es un ejemplo de creación de una nueva extensión destinatarios ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Destinado a la versión SDK en un proyecto existente

Para modificar un proyecto existente para tener como destino una versión SDK diferente, modifique la siguiente línea en ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
En este ejemplo, reemplace ```latest``` con la versión del SDK deseada, es decir, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

A continuación, ejecute ```npm install``` para actualizar las referencias a lo largo de su proyecto.