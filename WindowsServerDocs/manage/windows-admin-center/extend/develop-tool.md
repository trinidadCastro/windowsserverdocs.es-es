---
title: Desarrollar una extensión de herramienta
description: Desarrollar una extensión de herramienta SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: f3bf885cd86015842be26124e7374f622d38a9c2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954131"
---
# <a name="develop-a-tool-extension"></a>Desarrollar una extensión de herramienta

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Una extensión de herramienta es la forma principal en que los usuarios interactúan con el centro de administración de Windows para administrar una conexión, como un servidor o un clúster. Al hacer clic en una conexión en la pantalla principal del centro de administración de Windows y conectarse, se le presentará una lista de herramientas en el panel de navegación izquierdo. Al hacer clic en una herramienta, la extensión de la herramienta se carga y se muestra en el panel derecho.

Cuando se carga una extensión de herramienta, puede ejecutar llamadas WMI o scripts de PowerShell en un servidor o clúster de destino, y Mostrar información en la interfaz de usuario o ejecutar comandos en función de los datos proporcionados por el usuario. Las extensiones de herramientas definen en qué soluciones se debe mostrar, lo que da lugar a un conjunto diferente de herramientas para cada solución.

> [!NOTE]
> ¿No está familiarizado con los distintos tipos de extensión? Obtenga más información sobre la [arquitectura de extensibilidad y los tipos de extensión](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparación del entorno

Si todavía no lo ha hecho, [Prepare el entorno mediante la](prepare-development-environment.md) instalación de las dependencias y los requisitos previos globales necesarios para todos los proyectos.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Crear una nueva extensión de herramienta con la CLI del centro de administración de Windows ##

Una vez que tenga todas las dependencias instaladas, está listo para crear la nueva extensión de herramienta.  Cree o busque una carpeta que contenga los archivos del proyecto, abra un símbolo del sistema y establezca esa carpeta como el directorio de trabajo.  Con la CLI del centro de administración de Windows que se instaló previamente, cree una nueva extensión con la siguiente sintaxis:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | El nombre de su empresa (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

De esta forma, se crea una nueva carpeta dentro del directorio de trabajo actual con el nombre especificado para la herramienta, se copian todos los archivos de plantilla necesarios en el proyecto y se configuran los archivos con el nombre de la compañía y la herramienta.

A continuación, cambie el directorio a la carpeta que acaba de crear y, a continuación, instale las dependencias locales necesarias mediante la ejecución del siguiente comando:

``` cmd
npm install
```

Una vez completado, habrá configurado todo lo que necesita para cargar la nueva extensión en el centro de administración de Windows.

## <a name="add-content-to-your-extension"></a>Agregar contenido a la extensión

Ahora que ha creado una extensión con la CLI del centro de administración de Windows, está listo para personalizar el contenido.  Consulte estas guías para obtener ejemplos de lo que puede hacer:

- Agregar un [módulo vacío](guides/add-module.md)
- Agregar un [iframe](guides/add-iframe.md)

También se pueden encontrar más ejemplos en nuestro [sitio de GITHUB SDK](https://aka.ms/wacsdk):
-  [Herramientas de desarrollo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) es una extensión totalmente operativa que puede estar cargada en el centro de administración de Windows y contiene una amplia colección de ejemplos de herramientas y funcionalidad de ejemplo que puede examinar y usar en su propia extensión.

## <a name="customize-your-extensions-icon"></a>Personalizar el icono de la extensión

Puede personalizar el icono que se muestra para la extensión en la lista de herramientas.  Para ello, modifique todas ```icon``` las entradas de ```manifest.json``` para la extensión:

``` json
"icon": "{!icon-uri}",
```

| Value | Explicación | URI de ejemplo |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | La ubicación del recurso de icono | ```assets/foo-icon.svg``` |

Nota: Actualmente, los iconos personalizados no están visibles cuando se carga la extensión en el modo de desarrollo.  Como solución alternativa, quite el contenido de de la ```target``` siguiente manera:

``` json
"target": "",
```

Esta configuración solo es válida para la carga de caras en modo de desarrollo, por lo que es importante conservar el valor contenido en ```target``` y, después, restaurarlo antes de publicar la extensión.

## <a name="build-and-side-load-your-extension"></a>Compilar y cargar la extensión

A continuación, compile y cargue la extensión en el centro de administración de Windows.  Abra una ventana de comandos, cambie el directorio al directorio de origen y, a continuación, estará listo para compilar.

* Compilar y servir con Gulp:

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

Tenga en cuenta que debe elegir un puerto que esté disponible actualmente. Asegúrese de que no intenta usar el puerto en el que se ejecuta el centro de administración de Windows.

El proyecto se puede cargar en una instancia local del centro de administración de Windows para realizar pruebas adjuntando el proyecto servido localmente en el centro de administración de Windows.

* Iniciar el centro de administración de Windows en un explorador Web
* Abrir el depurador (F12)
* Abra la consola de y escriba el siguiente comando:

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualizar el explorador Web

El proyecto ahora estará visible en la lista de herramientas con (lado cargado) junto al nombre.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Usar como destino una versión diferente del SDK del centro de administración de Windows

Es fácil mantener actualizada la extensión con los cambios de SDK y los cambios de plataforma.  Obtenga información acerca de cómo establecer como [destino una versión diferente](target-sdk-version.md) del SDK del centro de administración de Windows.