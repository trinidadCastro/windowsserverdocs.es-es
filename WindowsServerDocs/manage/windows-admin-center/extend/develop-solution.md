---
title: Desarrollar una extensión de la solución
description: Desarrollar una extensión de solución SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 27ded378a40537455423f79869dfd07dcd2ba625
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949600"
---
# <a name="develop-a-solution-extension"></a>Desarrollar una extensión de la solución

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Las soluciones definen principalmente un tipo único de objeto que desea administrar a través del centro de administración de Windows.  Estas soluciones o tipos de conexión se incluyen de forma predeterminada con el centro de administración de Windows:

* Conexiones de Windows Server
* Conexiones de equipos Windows
* Conexiones de clúster de conmutación por error
* Conexiones de clúster hiperconvergido

Al seleccionar una conexión en la página de conexión del centro de administración de Windows, se carga la extensión de la solución para el tipo de conexión y el centro de administración de Windows intentará conectarse al nodo de destino. Si la conexión es correcta, la interfaz de usuario de la extensión de la solución se cargará y el centro de administración de Windows mostrará las herramientas de esa solución en el panel de navegación izquierdo.

Si desea crear una GUI de administración para los servicios no definidos por los tipos de conexión predeterminados anteriores, un conmutador de red u otro hardware que no se pueda detectar por nombre de equipo, puede que desee crear su propia extensión de solución.

> [!NOTE]
> ¿No está familiarizado con los distintos tipos de extensión? Obtenga más información sobre la [arquitectura de extensibilidad y los tipos de extensión](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparación del entorno

Si todavía no lo ha hecho, [Prepare el entorno mediante la](prepare-development-environment.md) instalación de las dependencias y los requisitos previos globales necesarios para todos los proyectos.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Crear una nueva extensión de solución con la CLI del centro de administración de Windows ##

Una vez que tenga todas las dependencias instaladas, está listo para crear la nueva extensión de la solución.  Cree o busque una carpeta que contenga los archivos del proyecto, abra un símbolo del sistema y establezca esa carpeta como el directorio de trabajo.  Con la CLI del centro de administración de Windows que se instaló previamente, cree una nueva extensión con la siguiente sintaxis:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Value | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | El nombre de su empresa (con espacios) | ```Contoso Inc``` |
| ```{!Solution Name}``` | El nombre de la solución (con espacios) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

De esta forma, se crea una nueva carpeta dentro del directorio de trabajo actual con el nombre especificado para la solución, se copian todos los archivos de plantilla necesarios en el proyecto y se configuran los archivos con el nombre de la compañía, la solución y la herramienta.

A continuación, cambie el directorio a la carpeta que acaba de crear y, a continuación, instale las dependencias locales necesarias mediante la ejecución del siguiente comando:

```
npm install
```

Una vez completado, habrá configurado todo lo que necesita para cargar la nueva extensión en el centro de administración de Windows.

## <a name="add-content-to-your-extension"></a>Agregar contenido a la extensión

Ahora que ha creado una extensión con la CLI del centro de administración de Windows, está listo para personalizar el contenido.  Consulte estas guías para obtener ejemplos de lo que puede hacer:

- Agregar un [módulo vacío](guides/add-module.md)
- Agregar un [iframe](guides/add-iframe.md)
- Crear un [proveedor de conexiones personalizado](guides/create-connection-provider.md)
- Modificar el [comportamiento de navegación raíz](guides/modify-root-navigation.md)

También se pueden encontrar más ejemplos en nuestro [sitio de GITHUB SDK](https://aka.ms/wacsdk):
-  [Herramientas de desarrollo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) es una extensión totalmente operativa que puede estar cargada en el centro de administración de Windows y contiene una amplia colección de ejemplos de herramientas y funcionalidad de ejemplo que puede examinar y usar en su propia extensión.

## <a name="build-and-side-load-your-extension"></a>Compilar y cargar la extensión

A continuación, compile y cargue la extensión en el centro de administración de Windows.  Abra una ventana de comandos, cambie el directorio al directorio de origen y, a continuación, estará listo para compilar.

* Compilar y servir con Gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Tenga en cuenta que debe elegir un puerto que esté disponible actualmente. Asegúrese de que no intenta usar el puerto en el que se ejecuta el centro de administración de Windows.

El proyecto se puede cargar en una instancia local del centro de administración de Windows para realizar pruebas adjuntando el proyecto servido localmente en el centro de administración de Windows.

* Iniciar el centro de administración de Windows en un explorador Web
* Abrir el depurador (F12)
* Abra la consola de y escriba el siguiente comando:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualizar el explorador Web

El proyecto ahora estará visible en la lista de herramientas con (lado cargado) junto al nombre.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Usar como destino una versión diferente del SDK del centro de administración de Windows

Es fácil mantener actualizada la extensión con los cambios de SDK y los cambios de plataforma.  Obtenga información acerca de cómo establecer como [destino una versión diferente](target-sdk-version.md) del SDK del centro de administración de Windows.