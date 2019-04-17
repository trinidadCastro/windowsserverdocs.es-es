---
title: Desarrollar un complemento de puerta de enlace
description: Desarrollar un complemento de puerta de enlace SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081162"
---
# Desarrollar un complemento de puerta de enlace

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Un complemento de puerta de enlace de Windows Admin Center permite la comunicación de API de la interfaz de usuario de la herramienta o la solución a un nodo de destino.  Windows Admin Center hospeda un servicio de puerta de enlace que se transmite los comandos y los scripts de los complementos de puerta de enlace que se ejecuta en los nodos de destino. El servicio de puerta de enlace se puede ampliar para incluir complementos de puerta de enlace personalizados que admiten protocolos que no sean los valores predeterminados.

Estos complementos de puerta de enlace se incluyen de forma predeterminada con Windows Admin Center:

* Complemento de puerta de enlace de PowerShell
* Complemento de puerta de enlace WMI

Si lo deseas para comunicarse con un protocolo distinto a PowerShell o WMI, como con REST, puedes crear tu propio complemento de puerta de enlace.  Los complementos de puerta de enlace se cargan en un AppDomain independiente desde el proceso de puerta de enlace existente, pero usan el mismo nivel de elevación de los derechos.

> [!NOTE]
> ¿No está familiarizado con los tipos de extensión diferente? Más información sobre los [tipos de extensión y la arquitectura de extensibilidad](understand-extensions.md).

## Preparar el entorno

Si no lo has hecho ya, [Preparar el entorno](prepare-development-environment.md) al instalar las dependencias y globales requisitos previos necesarios para todos los proyectos.

## Crear un complemento de puerta de enlace (biblioteca de C#)

Para crear un complemento de puerta de enlace personalizados, crea una nueva clase de C# que implementa el ```IPlugIn``` de la interfaz de la ```Microsoft.ManagementExperience.FeatureInterfaces``` espacio de nombres.  

> [!NOTE]
> El ```IFeature``` interfaz, disponible en versiones anteriores del SDK, que ahora está marcado como obsoleto.  Todos los desarrollo del complemento de puerta de enlace debe usar IPlugIn (o, opcionalmente, la clase abstracta HttpPlugIn).

### Descargar la muestra de GitHub

Para empezar a trabajar rápidamente con un complemento de puerta de enlace personalizados, puedes clonar o descargar una copia de nuestro [proyecto de complemento de muestra C#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) de nuestro [sitio de GitHub](https://aka.ms/wacsdk)de SDK de Windows Admin Center.

### Agregar contenido

Agregar contenido nuevo a la copia clonada del proyecto del [proyecto de complemento de muestra C#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (o su propio proyecto) para contener las API personalizadas, a continuación, crear el archivo DLL del complemento de puerta de enlace personalizados para usarse en los siguientes pasos.

### Implementar el complemento de prueba

Probar la DLL del complemento de puerta de enlace personalizados, cargar en un proceso de puerta de enlace de Windows Admin Center.

Windows Admin Center busca todos los complementos de ActiveX en un ```plugins``` carpeta en la carpeta de datos de la aplicación de la máquina (con el valor de CommonApplicationData de la enumeración Environment.SpecialFolder). En Windows 10, esta ubicación es ```C:\ProgramData\Server Management Experience```.  Si el ```plugins``` carpeta no existe aún, puede crear la carpeta.

> [!NOTE]
> Para invalidar la ubicación del complemento en una versión de depuración, puedes actualizar el valor de configuración "StaticsFolder". Si estás depurando localmente, esta configuración está en App.Config de la solución de escritorio. 

Dentro de la carpeta de complementos de ActiveX (en este ejemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Crea una nueva carpeta con el mismo nombre que el ```Name``` valor de propiedad de la ```Feature``` en la DLL del complemento de puerta de enlace personalizados (en nuestro proyecto de muestra, el ```Name``` es "Muestra Uno")
* Copia el archivo DLL de complemento de puerta de enlace personalizados a esta nueva carpeta
* Reiniciar el proceso de Windows Admin Center

Después de reinicia el proceso de administración de Windows, podrás ejercicio las API en el complemento de puerta de enlace personalizados DLL emitiendo una GET, coloca, revisión, eliminar o publicar en ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### Opcional: Asociar al complemento de depuración

En Visual Studio 2017, en el menú Depurar, selecciona "Asociar al proceso". En la siguiente ventana, desplazarse por la lista de procesos disponibles y haga clic en "Adjuntar" SMEDesktop.exe. Una vez, se inicia el depurador puede colocar un punto de interrupción en el código de la característica y, a continuación, ejercicio mediante el formato de dirección URL anterior. Para nuestro proyecto de ejemplo (nombre de la característica: "Muestra Uno") es la dirección URL: "http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno"

## Crear una extensión de herramienta con la CLI de Windows Admin Center ##

Ahora, debemos crear una extensión de herramienta que se puede llamar a tu complemento de puerta de enlace personalizados.  Crear o busca una carpeta donde quieres almacenar los archivos de proyecto, abre un símbolo del sistema y establecer esa carpeta como directorio de trabajo.  Mediante la CLI de centro de administración de Windows que se instaló anteriormente, crea una nueva extensión con la sintaxis siguiente:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de tu compañía (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Esto crea una nueva carpeta dentro del directorio de trabajo actual con el nombre especificado para la herramienta, se copian todos los archivos de plantilla necesarios en el proyecto y configura los archivos con el nombre de empresa y la herramienta.  

A continuación, cambia el directorio a la carpeta que acabas de crear y luego instalar necesario de dependencias local ejecutando el siguiente comando:

```
npm install
```

Una vez que se completa esto, hayas configurado todo lo que necesario para cargar la nueva extensión en Windows Admin Center. 

## Conectar la extensión de herramienta para el complemento de puerta de enlace personalizados

Ahora que has creado una extensión con la CLI de Windows Admin Center, estás listo para conectar la extensión de herramienta para el complemento de puerta de enlace personalizados, siguiendo estos pasos:

- Agregar un [módulo vacío](guides\add-module.md)
- Usar el [complemento de puerta de enlace personalizados](guides\use-custom-gateway-plugin.md) en la extensión de herramienta
 
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