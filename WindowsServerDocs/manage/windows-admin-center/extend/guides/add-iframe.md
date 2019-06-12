---
title: Agregar un iFrame a una extensión de herramienta
description: 'Desarrollar una extensión de la herramienta Windows Admin Center SDK (proyecto Honolulu): adición de un iFrame en una extensión de herramienta'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: da3850b75a0e069f9153d3c66baef9f00b67d61c
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452590"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Agregar un iFrame a una extensión de herramienta

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

En este artículo, se agregará un iFrame en una extensión de herramienta nueva y vacía que hemos creado con la CLI de Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparar el entorno ##

Si no lo ha hecho ya, siga las instrucciones de [desarrollar una extensión de la herramienta](../develop-tool.md) para preparar el entorno y crear un nuevo, vacíe la extensión de la herramienta.

## <a name="add-a-module-to-your-project"></a>Agregue un módulo al proyecto ##

Agregue un nuevo [módulo vacío](add-module.md) al proyecto, a la que se agregará un elemento iFrame en el paso siguiente.  

## <a name="add-an-iframe-to-your-module"></a>Agregue un elemento iFrame al módulo ##

Ahora vamos a agregar un iFrame para ese módulo nuevo y vacío que acabamos de crear.

En \src\app\, examinar en la carpeta del módulo y, después, abra el archivo ```{!module-name}.component.html```, que se encuentra con la convención de nomenclatura siguiente:

| Valor | Explicación | Ejemplo de nombre de archivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.component.html``` |
    
En el archivo html, agregue el siguiente contenido:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Eso es todo, ha agregado un elemento iFrame en su extensión.  A continuación, puede [de compilación y side carga](../develop-tool.md#build-and-side-load-your-extension) la extensión en Windows Admin Center para ver los resultados.

> [!Note]
> Configuración de directiva de seguridad (CSP) de contenido podría impedir que algunos sitios representación en un iFrame dentro de Windows Admin Center. Puede aprender más sobre esto [aquí](https://content-security-policy.com/). 
