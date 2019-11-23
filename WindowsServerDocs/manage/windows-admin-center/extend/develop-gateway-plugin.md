---
title: Desarrollar un complemento de puerta de enlace
description: Desarrollar un complemento de puerta de enlace SDK del centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406930"
---
# <a name="develop-a-gateway-plugin"></a>Desarrollar un complemento de puerta de enlace

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Un complemento de puerta de enlace del centro de administración de Windows permite la comunicación de API desde la interfaz de usuario de la herramienta o solución a un nodo de destino.  El centro de administración de Windows hospeda un servicio de puerta de enlace que retransmite comandos y scripts de complementos de puerta de enlace que se ejecutan en nodos de destino. El servicio de puerta de enlace se puede extender para incluir complementos de puerta de enlace personalizados que admiten protocolos distintos de los predeterminados.

Estos complementos de puerta de enlace se incluyen de forma predeterminada con el centro de administración de Windows:

* Complemento de puerta de enlace de PowerShell
* Complemento de puerta de enlace WMI

Si desea comunicarse con un protocolo distinto de PowerShell o WMI, como con REST, puede crear su propio complemento de puerta de enlace.  Los complementos de puerta de enlace se cargan en un AppDomain independiente del proceso de puerta de enlace existente, pero usan el mismo nivel de elevación para los derechos.

> [!NOTE]
> ¿No está familiarizado con los distintos tipos de extensión? Obtenga más información sobre la [arquitectura de extensibilidad y los tipos de extensión](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparar el entorno

Si todavía no lo ha hecho, [Prepare el entorno mediante la](prepare-development-environment.md) instalación de las dependencias y los requisitos previos globales necesarios para todos los proyectos.

## <a name="create-a-gateway-plugin-c-library"></a>Crear un complemento de puertaC# de enlace (biblioteca)

Para crear un complemento de puerta de enlace personalizado, C# cree una nueva clase que implemente la interfaz de ```IPlugIn``` desde el espacio de nombres ```Microsoft.ManagementExperience.FeatureInterfaces```.  

> [!NOTE]
> La interfaz de ```IFeature```, disponible en versiones anteriores del SDK, ahora está marcada como obsoleta.  Todo el desarrollo del complemento de puerta de enlace debe usar IPlugIn (o, opcionalmente, la clase abstracta HttpPlugIn).

### <a name="download-sample-from-github"></a>Descargar el ejemplo de GitHub

Para empezar a trabajar rápidamente con un complemento de puerta de enlace personalizado, puede clonar o descargar una copia de nuestro [proyecto de complemento de C# ejemplo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) en nuestro sitio de [GitHub](https://aka.ms/wacsdk)del SDK del centro de administración de Windows.

### <a name="add-content"></a>Agregar contenido

Agregue nuevo contenido a la copia clonada del proyecto [de C# proyecto de complemento de ejemplo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (o su propio proyecto) para que contenga las API personalizadas y, a continuación, cree el archivo DLL del complemento de puerta de enlace personalizado que se usará en los pasos siguientes.

### <a name="deploy-plugin-for-testing"></a>Implementar complemento para pruebas

Pruebe el archivo DLL del complemento de puerta de enlace personalizada cargándolo en el proceso de puerta de enlace del centro de administración de Windows.

El centro de administración de Windows busca todos los complementos en una carpeta ```plugins``` en la carpeta datos de la aplicación del equipo actual (mediante el valor CommonApplicationData de la enumeración Environment. SpecialFolder). En Windows 10, esta ubicación es ```C:\ProgramData\Server Management Experience```.  Si el ```plugins``` carpeta no existe todavía, puede crear la carpeta usted mismo.

> [!NOTE]
> Puede invalidar la ubicación del complemento en una compilación de depuración actualizando el valor de configuración "StaticsFolder". Si está depurando localmente, este valor se encuentra en el archivo app. config de la solución de escritorio. 

Dentro de la carpeta plugins (en este ejemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Cree una nueva carpeta con el mismo nombre que el valor de propiedad ```Name``` de la ```Feature``` en el archivo DLL del complemento de puerta de enlace personalizada (en nuestro proyecto de ejemplo, el ```Name``` es "sample uno").
* Copie el archivo DLL del complemento de puerta de enlace personalizado en esta nueva carpeta.
* Reiniciar el proceso del centro de administración de Windows

Una vez que se reinicie el proceso de administración de Windows, podrá ejercitar las API en el archivo DLL del complemento de puerta de enlace personalizada mediante la emisión de GET, PUT, PATCH, DELETE o POST en ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Opcional: adjuntar a complemento para depurar

En Visual Studio 2017, en el menú Depurar, seleccione "asociar al proceso". En la siguiente ventana, desplácese por la lista procesos disponibles y seleccione SMEDesktop. exe y, a continuación, haga clic en "adjuntar". Una vez que se inicia el depurador, puede colocar un punto de interrupción en el código de la característica y, a continuación, ejecutar el formato de dirección URL anterior. En nuestro proyecto de ejemplo (nombre de característica: "ejemplo uno"), la dirección URL es: "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Crear una extensión de herramienta con la CLI del centro de administración de Windows ##

Ahora debemos crear una extensión de herramienta desde la que puede llamar al complemento de puerta de enlace personalizada.  Cree o busque una carpeta donde desee almacenar los archivos del proyecto, abra un símbolo del sistema y establezca esa carpeta como el directorio de trabajo.  Con la CLI del centro de administración de Windows instalada anteriormente, cree una nueva extensión con la siguiente sintaxis:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | El nombre de su empresa (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

De esta forma, se crea una nueva carpeta dentro del directorio de trabajo actual con el nombre especificado para la herramienta, se copian todos los archivos de plantilla necesarios en el proyecto y se configuran los archivos con el nombre de la compañía y la herramienta.  

A continuación, cambie el directorio a la carpeta que acaba de crear y, a continuación, instale las dependencias locales necesarias mediante la ejecución del siguiente comando:

```
npm install
```

Una vez completado, habrá configurado todo lo que necesita para cargar la nueva extensión en el centro de administración de Windows. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Conexión de la extensión de herramienta al complemento de puerta de enlace personalizada

Ahora que ha creado una extensión con la CLI del centro de administración de Windows, está listo para conectar la extensión de herramienta al complemento de puerta de enlace personalizada, para lo que debe seguir estos pasos:

- Agregar un [módulo vacío](guides/add-module.md)
- Usar el [complemento de puerta de enlace personalizado](guides/use-custom-gateway-plugin.md) en la extensión de la herramienta
 
## <a name="build-and-side-load-your-extension"></a>Compilar y cargar la extensión

A continuación, compile y cargue la extensión en el centro de administración de Windows.  Abra una ventana de comandos, cambie el directorio al directorio de origen y, a continuación, estará listo para compilar.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Usar como destino una versión diferente del SDK del centro de administración de Windows

Es fácil mantener actualizada la extensión con los cambios de SDK y los cambios de plataforma.  Obtenga información acerca de cómo establecer como [destino una versión diferente](target-sdk-version.md) del SDK del centro de administración de Windows.