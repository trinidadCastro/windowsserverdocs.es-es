---
title: Desarrollar una extensión de la solución
description: Desarrollar una extensión de la solución Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825476"
---
# <a name="develop-a-solution-extension"></a>Desarrollar una extensión de la solución

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Soluciones definen principalmente un único tipo de objeto que desea administrar a través de Windows Admin Center.  Estos tipos de soluciones o la conexión se incluyen con Windows Admin Center de forma predeterminada:

* Conexiones del servidor de Windows
* Conexiones de PC de Windows
* Conexiones de clúster de conmutación por error
* Conexiones de clúster hiperconvergido

Al seleccionar una conexión de la página de conexión de Windows Admin Center, la extensión de la solución para el tipo de la conexión se cargará y Windows Admin Center intentará conectarse al nodo de destino. Si la conexión es correcta, la solución de interfaz de usuario de la extensión se cargará y Windows Admin Center mostrará las herramientas para esa solución en el panel de navegación izquierdo.

Si desea crear una GUI de administración de servicios no definidos por los tipos de conexión predeterminados por encima, tal un conmutador de red u otro hardware no detectable por nombre de equipo, desea crear su propia extensión de la solución.

> [!NOTE]
> ¿No está familiarizado con los tipos de extensión diferente? Obtenga más información sobre la [tipos de arquitectura y la extensión de extensibilidad](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparar el entorno

Si no lo ha hecho ya, [preparar el entorno](prepare-development-environment.md) al instalar las dependencias y globales requisitos previos necesarios para todos los proyectos.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Cree una nueva extensión de la solución con la CLI de Windows Admin Center ##

Una vez que todas las dependencias instaladas, está listo para crear la nueva extensión de la solución.  Crear o busque una carpeta que contiene los archivos de proyecto, abra un símbolo del sistema y establece dicha carpeta como el directorio de trabajo.  Mediante la CLI de Windows Admin Center que se instaló previamente, cree una nueva extensión con la sintaxis siguiente:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de su compañía (con espacios) | ```Contoso Inc``` |
| ```{!Solution Name}``` | El nombre de la solución (con espacios) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Esto crea una nueva carpeta en el directorio de trabajo actual con el nombre especificado de la solución, copia todos los archivos de plantilla necesarios en el proyecto y los archivos se configura con la empresa, la solución y el nombre de la herramienta.  

A continuación, cambie el directorio a la carpeta que acaba de crear, a continuación, instalar las dependencias necesarias de locales, ejecute el comando siguiente:

```
npm install
```

Una vez que se complete, ha configurado todo lo que necesita para cargar la nueva extensión en Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Agregar contenido a la extensión

Ahora que ha creado una extensión con la CLI de Windows Admin Center, está listo para personalizar el contenido.  Consulte a estas guías para obtener ejemplos de lo que puede hacer:

- Agregar un [módulo vacío](guides\add-module.md)
- Agregar un [iFrame](guides\add-iframe.md)
- Crear un [el proveedor de conexión personalizado](guides\create-connection-provider.md)
- Modificar [root de comportamiento de navegación](guides\modify-root-navigation.md)
 
Puede encontrar más ejemplos nuestro [sitio GitHub SDK](https://aka.ms/wacsdk):
-  [Herramientas de desarrollo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) es una extensión totalmente operativa que puede ser de carga lateral en Windows Admin Center y contiene una amplia colección de ejemplos de funcionalidad y la herramienta de ejemplo que puede examinar y usar en su propia extensión.

## <a name="build-and-side-load-your-extension"></a>Compilación y cargan la extensión

A continuación, compilación y cargan la extensión en Windows Admin Center.  Abra una ventana de comandos, cambie el directorio a su directorio de origen, a continuación, está listo para compilar.

* Compila y sirve con Gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Ten en cuenta que debes elegir un puerto que esté actualmente disponible. Asegúrate de que no intentas utilizar el puerto en el que se ejecuta Windows Admin Center.

Tu proyecto se puede cargar en una instancia local de Windows Admin Center para pruebas conectando el proyecto servido localmente en Windows Admin Center.

* Iniciar Windows Admin Center en un navegador web
* Abre al depurador (F12)
* Abre la consola y escribe el siguiente comando:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualizar el navegador web

Tu proyecto estará visible a partir de ahora en la lista Herramientas con (transferido localmente) junto al nombre.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Tener como destino una versión diferente de los SDK de Windows Admin Center

Es fácil mantener la extensión actualizada con los cambios SDK y plataforma.  Obtenga información sobre cómo [destino una versión diferente](target-sdk-version.md) del SDK Windows Admin Center.