---
title: Preparar el entorno de desarrollo
description: Preparación del entorno de desarrollo SDK de Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server
ms.openlocfilehash: 136107210d2a8a4b336c9e4eb809e2ca096bfba2
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269222"
---
# <a name="prepare-your-development-environment"></a>Preparar el entorno de desarrollo

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Vamos a empezar a desarrollar extensiones con el SDK del centro de administración de Windows.  En este documento, trataremos el proceso para poner en marcha su entorno para compilar y probar una extensión para el centro de administración de Windows.

> [!NOTE]
> ¿Eres nuevo en el SDK de Windows Admin Center?  Encontrarás más información acerca de [Extensiones de Windows Admin Center](extensibility-overview.md)

Para preparar el entorno de desarrollo, realiza los pasos siguientes:

## <a name="install-prerequisites"></a>Instalar requisitos previos

Para empezar a desarrollar con el SDK, descarga e instala los requisitos previos siguientes:

* [Centro de administración de Windows](https://aka.ms/WACDownloadPage) (GA o versión preliminar)
* Visual Studio o [Visual Studio Code](https://code.visualstudio.com)
* [Node. js](https://nodejs.org/en/download/releases/) (versión 10.3.0)
* [Administrador de paquetes de node](https://npmjs.com/get-npm) (8.12.0 o posterior)
* [Nuget](https://www.nuget.org/downloads) (para publicación de extensiones)

> [!NOTE]
> Tienes que instalar y ejecutar Windows Admin Center en el modo de desarrollador para seguir los pasos siguientes. El modo de desarrollador permite a Windows Admin Center cargar los paquetes de extensión sin firmar. El centro de administración de Windows solo se puede instalar en el modo de desarrollo en un equipo con Windows 10. 
>
>  Para habilitar el modo de desarrollador, instala Windows Admin Center desde la línea de comandos con el parámetro DEV_MODE=1. En el ejemplo siguiente, reemplaza ```<version>``` por la versión que se va a instalar, es decir, ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Instalar dependencias globales

A continuación, instale o actualice las dependencias necesarias para los proyectos, con el administrador de paquetes de node. Estas dependencias se instalarán globalmente y estarán disponibles para todos los proyectos.

```
npm install -g npm

npm install -g @angular/cli@7.1.2

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Puede instalar una versión posterior de @angular/cli, sin embargo, tenga en cuenta que si instala una versión superior a 7.1.2, recibirá una advertencia durante el paso de compilación de Gulp que indica que la versión de la CLI local no coincide con la versión instalada.

## <a name="next-steps"></a>Pasos siguientes

Ahora que el entorno está preparado, está listo para empezar a crear contenido.

- Crear una extensión de [herramienta](develop-tool.md)
- Crear una extensión de [solución](develop-solution.md)
- Crear un [complemento de puerta de enlace](develop-gateway-plugin.md)
- Obtén más información con nuestras [guías](guides.md)

## <a name="sdk-design-toolkit"></a>Kit de herramientas de diseño de SDK

Consulte el kit de herramientas de [diseño del SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)de Windows admin Center. Este kit de herramientas está diseñado para ayudarle a simular rápidamente extensiones en PowerPoint mediante estilos, controles y plantillas de página del centro de administración de Windows. Vea Cuál puede ser su extensión en el centro de administración de Windows antes de comenzar a codificar.

