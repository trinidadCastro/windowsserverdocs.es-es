---
title: Dirigirse a una versión diferente de los SDK de Windows Admin Center
description: Dirigirse a una versión diferente de los SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081042"
---
# Dirigirse a una versión diferente de los SDK de Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Es fácil mantener la extensión al día con los cambios SDK y los cambios de la plataforma.  Usamos [etiquetas NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) para organizar la versión de las nuevas características en las versiones de SDK.

Hay tres versiones SDK, que puedes elegir entre:

* ```latest``` : este paquete SDK se alinea con la versión de disponibilidad general actual de Windows Admin Center
* ```insider``` : este paquete SDK se alinea con la versión actual de la versión preliminar de Windows Admin Center (disponible en Windows Server Insider Preview)
* ```next``` : este paquete SDK contiene la funcionalidad más reciente

> [!NOTE]
> Obtén más información sobre las distintas [versiones](https://aka.ms/WACDownloadPage) de Windows Admin Center que están disponibles para su descarga.

## Versión de SDK en un nuevo proyecto

Al crear una nueva extensión, puedes incluir la ```--version``` parámetro seleccionar como destino una versión diferente del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de tu compañía (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |
| ```{!version}``` | Versión del SDK | ```latest``` |

Este es un ejemplo de creación de una nueva extensión destinados a ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## Versión de SDK en un proyecto existente

Para modificar un proyecto ya existente para dirigirse a una versión SDK diferentes, modificar la siguiente línea en ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
En este ejemplo, reemplaza ```latest``` con la versión del SDK deseada, es decir, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

A continuación, ejecute ```npm install``` para actualizar las referencias en todo el proyecto.