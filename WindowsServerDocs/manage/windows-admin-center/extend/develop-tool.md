---
title: Desarrollar una extensión de herramienta
description: Desarrollar una extensión de la herramienta Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 092a97c1166f1090dd7c556f1ab86d42a1f46ee4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889276"
---
# <a name="develop-a-tool-extension"></a>Desarrollar una extensión de herramienta

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Una extensión de la herramienta es la principal manera que los usuarios interactúan con Windows Admin Center para administrar una conexión, como un servidor o clúster. Cuando haga clic en una conexión en la pantalla principal de Windows Admin Center y se conecta, a continuación, se presenta con una lista de herramientas del panel de navegación izquierdo. Al hacer clic en una herramienta, la extensión de la herramienta se carga y se muestra en el panel derecho.

Cuando se carga una extensión de la herramienta, puede ejecutar scripts de PowerShell o las llamadas de WMI en un clúster o servidor de destino y mostrar información en la interfaz de usuario o ejecutar comandos en función de entrada del usuario. Extensiones de herramientas definen qué soluciones se debe mostrar, lo que resulta en un conjunto diferente de herramientas para cada solución.

> [!NOTE]
> ¿No está familiarizado con los tipos de extensión diferente? Obtenga más información sobre la [tipos de arquitectura y la extensión de extensibilidad](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparar el entorno

Si no lo ha hecho ya, [preparar el entorno](prepare-development-environment.md) al instalar las dependencias y globales requisitos previos necesarios para todos los proyectos.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Cree una nueva extensión de la herramienta con la CLI de Windows Admin Center ##

Una vez que todas las dependencias instaladas, está listo para crear la nueva extensión de la herramienta.  Crear o busque una carpeta que contiene los archivos de proyecto, abra un símbolo del sistema y establece dicha carpeta como el directorio de trabajo.  Mediante la CLI de Windows Admin Center que se instaló previamente, cree una nueva extensión con la sintaxis siguiente:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de su compañía (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Esto crea una nueva carpeta en el directorio de trabajo actual con el nombre especificado para la herramienta, copia todos los archivos de plantilla necesarios en el proyecto y los archivos se configura con el nombre de empresa y la herramienta.  

A continuación, cambie el directorio a la carpeta que acaba de crear, a continuación, instalar las dependencias necesarias de locales, ejecute el comando siguiente:

``` cmd
npm install
```

Una vez que se complete, ha configurado todo lo que necesita para cargar la nueva extensión en Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Agregar contenido a la extensión

Ahora que ha creado una extensión con la CLI de Windows Admin Center, está listo para personalizar el contenido.  Consulte a estas guías para obtener ejemplos de lo que puede hacer:

- Agregar un [módulo vacío](guides\add-module.md)
- Agregar un [iFrame](guides\add-iframe.md)
 
Puede encontrar más ejemplos nuestro [sitio GitHub SDK](https://aka.ms/wacsdk):
-  [Herramientas de desarrollo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) es una extensión totalmente operativa que puede ser de carga lateral en Windows Admin Center y contiene una amplia colección de ejemplos de funcionalidad y la herramienta de ejemplo que puede examinar y usar en su propia extensión.

## <a name="customize-your-extensions-icon"></a>Personalizar el icono de la extensión

Puede personalizar el icono que se muestra para la extensión en la lista de herramientas.  Para ello, modifique todas ```icon``` las entradas de ```manifest.json``` para la extensión:

``` json
"icon": "{!icon-uri}",
```

| Valor | Explicación | Uri de ejemplo |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | La ubicación del recurso de icono | ```assets/foo-icon.svg``` |

Nota: Actualmente, los iconos personalizados no son visibles al lado carguen su extensión en modo de desarrollador.  Como alternativa, elimine el contenido de ```target``` como sigue:

``` json
"target": "",
```

Esta configuración sólo es válida para la instalación de prueba en el modo de desarrollo, por lo que es importante conservar el valor contenido en ```target``` y, a continuación, restáurela antes de publicar la extensión.

## <a name="build-and-side-load-your-extension"></a>Compilación y cargan la extensión

A continuación, compilación y cargan la extensión en Windows Admin Center.  Abra una ventana de comandos, cambie el directorio a su directorio de origen, a continuación, está listo para compilar.

* Compila y sirve con Gulp:

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

Ten en cuenta que debes elegir un puerto que esté actualmente disponible. Asegúrate de que no intentas utilizar el puerto en el que se ejecuta Windows Admin Center.

Tu proyecto se puede cargar en una instancia local de Windows Admin Center para pruebas conectando el proyecto servido localmente en Windows Admin Center.

* Iniciar Windows Admin Center en un navegador web
* Abre al depurador (F12)
* Abre la consola y escribe el siguiente comando:

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualizar el navegador web

Tu proyecto estará visible a partir de ahora en la lista Herramientas con (transferido localmente) junto al nombre.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Tener como destino una versión diferente de los SDK de Windows Admin Center

Es fácil mantener la extensión actualizada con los cambios SDK y plataforma.  Obtenga información sobre cómo [destino una versión diferente](target-sdk-version.md) del SDK Windows Admin Center.