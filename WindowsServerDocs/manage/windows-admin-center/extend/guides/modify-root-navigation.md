---
title: Modificar el comportamiento de navegación raíz
description: Desarrollar una extensión de la solución SDK de Windows Admin Center (proyecto Honolulu) - modificar el comportamiento de navegación de raíz
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905045"
---
# Modificar el comportamiento de navegación de raíz para una extensión de la solución

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

En esta guía te obtendrás información sobre cómo modificar el comportamiento de navegación de la raíz de la solución para que el comportamiento de la lista de conexiones diferentes, así como cómo mostrar u ocultar la lista de herramientas.

## Modificar el comportamiento de navegación de raíz

Abre el archivo de manifest.json en {raíz de la extensión} \src y busca la propiedad "rootNavigationBehavior". Esta propiedad tiene dos valores válidos: "conexiones" o "path". El comportamiento de "conexiones" se detallará más adelante en la documentación.

### Ruta de acceso de configuración como un rootNavigationBehavior

Establece el valor de ```rootNavigationBehavior``` a ```path```y, a continuación, elimina el ```requirements``` propiedad y deja el ```path``` propiedad como una cadena vacía. Has completado la configuración mínima necesaria para crear una extensión de la solución. Guarda el archivo y gulp compilación -> gulp servir como lo harían una herramienta y, a continuación, lado cargar la extensión en la extensión de Windows Admin Center local.

Una matriz de puntos de entrada válidos manifiesto tiene este aspecto:
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

Herramientas integradas con este tipo de estructura le conexiones no se requiere para cargar, pero no tienen funcionalidad de conectividad de nodo ya sea.

### Conexiones de configuración como un rootNavigationBehavior

Cuando se establece la ```rootNavigationBehavior``` propiedad ```connections```, se indica que el Shell de Windows Admin Center que haya un nodo conectado (siempre un servidor de algún tipo) que se debe conectar, y comprueba el estado de conexión. Teniendo esto, hay 2 pasos en la comprobación de conexión. 1) Windows Admin Center intentará realizar un intento de iniciar sesión en el nodo con sus credenciales (para establecer la sesión remota de PowerShell) y 2) se ejecutará el script de PowerShell que proporciones para identificar si el nodo se encuentra en un estado conectable.

Una definición de la solución válido con conexiones tendrá este aspecto:

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

Cuando el rootNavigationBehavior se establece en "conexiones" son necesarios para crear la definición de conexiones en el manifiesto. Esto incluye la propiedad "encabezado" (se usará para mostrar en el encabezado de la solución cuando un usuario lo selecciona en el menú), una matriz de connectionTypes (Esto permitirá especificar qué connectionTypes se usan en la solución. Obtener más información sobre esto en la documentación de connectionProvider.).

## Habilitar y deshabilitar el menú Herramientas ##

Otra propiedad disponible en la definición de la solución es la propiedad "herramientas". Esto determinará si se muestra el menú Herramientas, así como la herramienta que se cargará. Cuando está habilitada, Windows Admin Center representará el menú de herramientas de la izquierda. Con defaultTool, es necesario que agregues un punto de entrada de la herramienta en el manifiesto para cargar los recursos adecuados. El valor de "defaultTool" debe ser la propiedad "name" de la herramienta, tal como se define en el manifiesto.
