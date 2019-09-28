---
title: Modificar el comportamiento de navegación raíz
description: Desarrollar una extensión de solución SDK del centro de administración de Windows (proyecto Honolulu)-modificar el comportamiento de navegación raíz
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 78c94f3ea13f54ac31f9de9dd60873b93eba2c17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385280"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificar el comportamiento de navegación raíz para una extensión de solución

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En esta guía aprenderá a modificar el comportamiento de navegación raíz de la solución para que tenga un comportamiento diferente de la lista de conexiones, así como a ocultar o Mostrar la lista de herramientas.

## <a name="modifying-root-navigation-behavior"></a>Modificar el comportamiento de navegación raíz

Abra el archivo manifest. JSON en {raíz de la extensión} \src y busque la propiedad "rootNavigationBehavior". Esta propiedad tiene dos valores válidos: "Connections" o "path". El comportamiento de "conexiones" se detalla más adelante en la documentación de.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Establecimiento de la ruta de acceso como un rootNavigationBehavior

Establezca el valor de ```rootNavigationBehavior``` en ```path``` y, a continuación, elimine la propiedad ```requirements``` y deje la propiedad ```path``` como una cadena vacía. Ha completado la configuración mínima necesaria para compilar una extensión de la solución. Guarde el archivo y Gulp Build-> Gulp sirven como lo haría con una herramienta y, a continuación, cargue la extensión en la extensión del centro de administración de Windows local.

Una matriz de entryPoints de manifiesto válida tiene el siguiente aspecto:
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

Las herramientas creadas con este tipo de estructura no requerirán que se carguen las conexiones, pero tampoco tendrán la funcionalidad de conectividad entre nodos.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Establecimiento de conexiones como rootNavigationBehavior

Al establecer la propiedad ```rootNavigationBehavior``` en ```connections```, está indicando al shell del centro de administración de Windows que habrá un nodo conectado (siempre un servidor de algún tipo) al que debe conectarse y comprobar el estado de la conexión. Con esto, hay dos pasos para comprobar la conexión. 1) el centro de administración de Windows intentará iniciar sesión en el nodo con sus credenciales (para establecer la sesión remota de PowerShell) y 2) ejecutará el script de PowerShell que proporciona para identificar si el nodo se encuentra en un estado conectado.

Una definición de solución válida con conexiones tendrá el siguiente aspecto:

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

Cuando rootNavigationBehavior se establece en "conexiones", es necesario crear la definición de las conexiones en el manifiesto. Esto incluye la propiedad "header" (se usará para mostrar en el encabezado de la solución cuando un usuario la selecciona en el menú), una matriz connectionTypes (esto especificará qué connectionTypes se usan en la solución. Más información sobre esto en la documentación de connectionProvider).

## <a name="enabling-and-disabling-the-tools-menu"></a>Habilitar y deshabilitar el menú herramientas ##

Otra propiedad disponible en la definición de la solución es la propiedad "Tools". Esto determinará si se muestra el menú herramientas, así como la herramienta que se cargará. Cuando está habilitada, el centro de administración de Windows representará el menú de herramientas de la izquierda. Con defaultTool, es necesario agregar un punto de entrada de herramienta al manifiesto para poder cargar los recursos adecuados. El valor de "defaultTool" debe ser la propiedad "Name" de la herramienta tal y como se define en el manifiesto.
