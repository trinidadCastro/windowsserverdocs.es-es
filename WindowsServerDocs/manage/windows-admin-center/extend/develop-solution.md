---
title: Desarrollar una extensión de la solución
description: Desarrollar una extensión de la solución SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081072"
---
# Desarrollar una extensión de la solución

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Principalmente las soluciones definen un único tipo de objeto que deseas administrar a través de Windows Admin Center.  Estos tipos de soluciones o la conexión se incluyen con Windows Admin Center de manera predeterminada:

* Conexiones de Windows Server
* Conexiones de PC con Windows
* Conexiones de clúster de conmutación por error
* Conexiones de clúster hiperconvergida

Cuando seleccionas una conexión desde la página de conexión de Windows Admin Center, se carga la extensión de la solución para el tipo de la conexión y Windows Admin Center intentará conectarse al nodo de destino. Si la conexión se realiza correctamente, la solución se cargará en la interfaz de usuario de la extensión y Windows Admin Center se mostrarán las herramientas de esa solución en el panel de navegación izquierdo.

Si quieres crear una interfaz gráfica de usuario de administración para los servicios no definidos por los tipos de conexión de forma predeterminada anteriormente, este tipo un conmutador de red u otro hardware no detectable por nombre de equipo, puedes crear tu propia extensión de la solución.

> [!NOTE]
> ¿No está familiarizado con los tipos de extensión diferente? Más información sobre los [tipos de extensión y la arquitectura de extensibilidad](understand-extensions.md).

## Preparar el entorno

Si no lo has hecho ya, [Preparar el entorno](prepare-development-environment.md) al instalar las dependencias y globales requisitos previos necesarios para todos los proyectos.

## Crear una nueva extensión de solución con la CLI de Windows Admin Center ##

Una vez que tienes todas las dependencias instaladas, estás listo para crear la nueva extensión de la solución.  Crear o busca una carpeta que contiene los archivos de proyecto, abre un símbolo del sistema y establecer esa carpeta como directorio de trabajo.  Mediante la CLI de centro de administración de Windows que se instaló anteriormente, crea una nueva extensión con la sintaxis siguiente:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de tu compañía (con espacios) | ```Contoso Inc``` |
| ```{!Solution Name}``` | El nombre de la solución (con espacios) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Esto crea una nueva carpeta dentro del directorio de trabajo actual con el nombre especificado para la solución, se copian todos los archivos de plantilla necesarios en el proyecto y configura los archivos con la empresa, la solución y el nombre de la herramienta.  

A continuación, cambia el directorio a la carpeta que acabas de crear y luego instalar necesario de dependencias local ejecutando el siguiente comando:

```
npm install
```

Una vez que se completa esto, hayas configurado todo lo que necesario para cargar la nueva extensión en Windows Admin Center. 

## Agregar contenido a la extensión

Ahora que has creado una extensión con la CLI de Windows Admin Center, estás listo para personalizar el contenido.  Consulta a estas guías para ver ejemplos de lo que puedes hacer:

- Agregar un [módulo vacío](guides\add-module.md)
- Agregar un [iFrame](guides\add-iframe.md)
- Crear un [proveedor de conexión personalizada](guides\create-connection-provider.md)
- Modificar el [comportamiento de navegación de raíz](guides\modify-root-navigation.md)
 
Incluso más ejemplos pueden encontrarse nuestro [sitio de GitHub SDK](https://aka.ms/wacsdk):
-  [Herramientas de desarrollo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) es una extensión totalmente funcional que puede cargarse en Windows Admin Center y contiene una amplia colección de funcionalidad de muestra y ejemplos de herramienta que puedes explorar y usar en su propia extensión.

## Compilar y cargar la extensión

A continuación, compilar y cargar la extensión en Windows Admin Center.  Abre una ventana de comandos, cambia el directorio en el directorio de origen, a continuación, estás listo para compilar.

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

## Dirigirse a una versión diferente de los SDK de Windows Admin Center

Es fácil mantener la extensión al día con los cambios SDK y los cambios de la plataforma.  Obtén información sobre cómo [destino una versión diferente](target-sdk-version.md) de los SDK de Windows Admin Center.