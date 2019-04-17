---
title: Preparar el entorno de desarrollo
description: Preparación del entorno de desarrollo SDK de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080922"
---
# Preparar el entorno de desarrollo

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Vamos a empezar a desarrollar extensiones con el SDK de Windows Admin Center.  En este documento, cubriremos el proceso para poner el entorno en funcionamiento y en ejecución para generar y probar una extensión para Windows Admin Center.

> [!NOTE]
> ¿Eres nuevo en el SDK de Windows Admin Center?  Encontrarás más información acerca de [Extensiones de Windows Admin Center](extensibility-overview.md)

Para preparar el entorno de desarrollo, realiza los pasos siguientes:

## Instalar requisitos previos

Para empezar a desarrollar con el SDK, descarga e instala los requisitos previos siguientes:

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (Versión de disponibilidad general o vista previa)
* Visual Studio o [Visual Studio Code](http://code.visualstudio.com)
* [Administrador de paquetes de nodo](https://npmjs.com/get-npm) (8.12.0 o posterior)
* [Nuget](https://www.nuget.org/downloads) (para publicación de extensiones)

> [!NOTE]
> Tienes que instalar y ejecutar Windows Admin Center en el modo de desarrollador para seguir los pasos siguientes. El modo de desarrollador permite a Windows Admin Center cargar los paquetes de extensión sin firmar.
>
>  Para habilitar el modo de desarrollador, instala Windows Admin Center desde la línea de comandos con el parámetro DEV_MODE=1. En el ejemplo siguiente, reemplaza ```<version>``` por la versión que se va a instalar, es decir, ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## Instalar dependencias globales

A continuación, instala o actualiza las dependencias necesarias para los proyectos, con el Administrador de paquetes de nodo. Estas dependencias se instalarán globalmente y estarán disponibles para todos los proyectos.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Puede instalar una versión posterior de @angular/cli, pero tenga en cuenta que si se instala una versión superior a 1.6.5, recibirás una advertencia durante el paso de compilación gulp que la versión cli local no coincide con la versión instalada.

## Pasos siguientes

Ahora que el entorno está preparado, estás listo para empezar a crear contenido.

- Crear una extensión de [herramienta](develop-tool.md)
- Crear una extensión de [solución](develop-solution.md)
- Crear un [complemento de puerta de enlace](develop-gateway-plugin.md)
- Obtén más información con nuestras [guías](guides.md)

## Kit de herramientas de diseño SDK

¡Echa un vistazo a nuestro [Kit de herramientas de diseño SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)de Windows Admin Center! Este kit de herramientas está diseñado para ayudar a simular rápidamente extensiones en PowerPoint con estilos de Windows Admin Center, controles y plantillas de página. ¡Vea la extensión aspecto que en Windows Admin Center antes de empezar a escribir código!

