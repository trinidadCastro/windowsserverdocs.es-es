---
title: Preparación del entorno de desarrollo
description: Preparar el entorno de desarrollo SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.openlocfilehash: fe519498e8021bde67b87ec7f78b3e1b9a64160b
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766948"
---
# <a name="prepare-your-development-environment"></a>Preparación del entorno de desarrollo

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Vamos a empezar a desarrollar extensiones con el SDK del centro de administración de Windows.  En este documento, trataremos el proceso para poner en marcha su entorno para compilar y probar una extensión para el centro de administración de Windows.

> [!NOTE]
> ¿Es nuevo en el SDK del centro de administración de Windows?  Más información acerca [de las extensiones para el centro de administración de Windows](extensibility-overview.md)

Para preparar el entorno de desarrollo, realice los pasos siguientes:

## <a name="install-prerequisites"></a>Requisitos previos de instalación

Para empezar a desarrollar con el SDK de, descargue e instale los siguientes requisitos previos:

* [Centro de administración de Windows](../overview.md) (GA o versión preliminar)
* Visual Studio o [Visual Studio Code](https://code.visualstudio.com)
* [Node.js](https://nodejs.org/en/download/releases/) (versión 10.3.0)
* [Administrador de paquetes de node](https://npmjs.com/get-npm) (8.12.0 o posterior)
* [Nuget](https://www.nuget.org/downloads) (para extensiones de publicación)

> [!NOTE]
> Debe instalar y ejecutar el centro de administración de Windows en modo de desarrollo para seguir los pasos siguientes. El modo de desarrollo permite que el centro de administración de Windows cargue paquetes de extensión sin firmar. El centro de administración de Windows solo se puede instalar en el modo de desarrollo en un equipo con Windows 10.
>
>  Para habilitar el modo de desarrollo, instale el centro de administración de Windows desde la línea de comandos con el parámetro DEV_MODE = 1. En el ejemplo siguiente, reemplace ```<version>``` por la versión que está instalando, es decir, ```WindowsAdminCenter1809.msi``` .
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
>Puede instalar una versión posterior de @angular/cli , pero tenga en cuenta que si instala una versión superior a 7.1.2, recibirá una advertencia durante el paso de compilación de Gulp que indica que la versión de la CLI local no coincide con la versión instalada.

## <a name="next-steps"></a>Pasos siguientes

Ahora que el entorno está preparado, está listo para empezar a crear contenido.

- Crear una extensión de [herramienta](develop-tool.md)
- Crear una extensión de [solución](develop-solution.md)
- Creación de un [complemento de puerta de enlace](develop-gateway-plugin.md)
- Más información con nuestras [guías](guides.md)

## <a name="sdk-design-toolkit"></a>Kit de herramientas de diseño de SDK

Consulte el kit de herramientas de [diseño del SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)de Windows admin Center. Este kit de herramientas está diseñado para ayudarle a simular rápidamente extensiones en PowerPoint mediante estilos, controles y plantillas de página del centro de administración de Windows. Vea Cuál puede ser su extensión en el centro de administración de Windows antes de comenzar a codificar.