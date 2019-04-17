---
title: Agregar un iFrame a una extensión de herramienta
description: 'Desarrollar una extensión de herramienta SDK de Windows Admin Center (proyecto Honolulu): agregar un iFrame a una extensión de herramienta'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080932"
---
# Agregar un iFrame a una extensión de herramienta

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

En este artículo, agregaremos un iFrame a una extensión de herramienta nuevo, vacío que hemos creado con la CLI de Windows Admin Center.

## Preparar el entorno ##

Si no lo has hecho ya, siga las instrucciones de [desarrollar una extensión de herramienta](..\develop-tool.md) para preparar el entorno y crear una extensión de herramienta nuevo y vacío.

## Agregar un módulo al proyecto ##

Agregar un nuevo [módulo vacío](add-module.md) al proyecto, al que también se agregarán iFrame en el siguiente paso.  

## Agregar un iFrame al módulo ##

Ahora, agregaremos un iFrame módulo vacía que acabamos de crear.

En \src\app\, busca en la carpeta del módulo y luego abre el archivo ```{!module-name}.component.html```encontrado siguiendo esta convención de nomenclatura:

| Valor | Explicación | Ejemplo de nombre de archivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.component.html``` |
    
En el archivo html, agrega el siguiente contenido:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Eso es todo, has agregado un iFrame a la extensión.  A continuación, puedes [compilar y cargar](..\develop-tool.md#build-and-side-load-your-extension) la extensión en el centro de administración de Windows para ver los resultados.
