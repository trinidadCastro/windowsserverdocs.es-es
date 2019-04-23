---
title: Modificar el comportamiento de navegación raíz
description: 'Desarrollar una extensión de la solución Windows Admin Center SDK (proyecto Honolulu): modificar el comportamiento de navegación de raíz'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861736"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificar el comportamiento de navegación de raíz para una extensión de la solución

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

En esta guía, obtendrá información sobre cómo modificar el comportamiento de navegación de la raíz para que su solución tiene el comportamiento de la lista de conexión diferentes, así como cómo ocultar o mostrar la lista de herramientas.

## <a name="modifying-root-navigation-behavior"></a>Modificar el comportamiento de navegación de raíz

Abra el archivo manifest.json en \src {raíz de la extensión} y busque la propiedad "rootNavigationBehavior". Esta propiedad tiene dos valores válidos: "conexiones" o "path". El comportamiento de "conexiones" se detalla más adelante en la documentación.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Ruta de acceso de configuración como un rootNavigationBehavior

Establezca el valor de ```rootNavigationBehavior``` a ```path```y, a continuación, elimine el ```requirements``` propiedad y deje el ```path``` propiedad como una cadena vacía. Ha completado la configuración mínima necesaria para crear una extensión de la solución. Guarde el archivo y compilación de gulp -> gulp servir como una herramienta y, a continuación, lado cargaría la extensión en la extensión de Windows Admin Center local.

Una matriz de puntos de entrada de manifiesto válido tiene este aspecto:
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

Las herramientas integradas con este tipo de estructura le no requiere conexiones para cargar, pero no tendrá la funcionalidad de conectividad de nodo bien.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Configuración de conexiones como una rootNavigationBehavior

Al establecer el ```rootNavigationBehavior``` propiedad ```connections```, está indicando que el Shell de Windows Admin Center que habrá un nodo conectado (siempre un servidor de algún tipo) que se debe conectar y compruebe el estado de la conexión. Con esto, hay 2 pasos en la comprobación de conexión. (1) Windows Admin Center intentará realizar un intento de iniciar sesión en el nodo con sus credenciales (para establecer la sesión remota de PowerShell) y (2) que ejecutará el script de PowerShell que proporcione para identificar si el nodo está en un estado conectable.

Una definición de solución válido con conexiones tendrá este aspecto:

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

Cuando se establece la rootNavigationBehavior "conexiones" son necesarios para elaborar la definición de las conexiones en el manifiesto. Esto incluye la propiedad "header" (se utilizará para mostrar en el encabezado de la solución cuando un usuario lo selecciona en el menú), una matriz de connectionTypes (Esto especificará qué connectionTypes se usan en la solución. Más información en la documentación de connectionProvider.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Habilitar y deshabilitar el menú Herramientas ##

Otra propiedad disponible en la definición de la solución es la propiedad "herramientas". Esto determinará si se muestra el menú Herramientas, así como la herramienta que se van a cargar. Cuando se habilita, Windows Admin Center presentará el menú Herramientas izquierdo. Con defaultTool, es necesario agregar un punto de entrada de la herramienta en el manifiesto con el fin de cargar los recursos adecuados. El valor de "defaultTool" debe ser la propiedad "name" de la herramienta, como se define en el manifiesto.
