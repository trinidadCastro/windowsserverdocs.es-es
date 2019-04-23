---
title: Desarrollar un complemento de puerta de enlace
description: Desarrollar un complemento de puerta de enlace Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834396"
---
# <a name="develop-a-gateway-plugin"></a>Desarrollar un complemento de puerta de enlace

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Un complemento de puerta de enlace de Windows Admin Center permite la comunicación de API desde la interfaz de usuario de la herramienta o la solución a un nodo de destino.  Windows Admin Center hospeda un servicio de puerta de enlace que retransmite los comandos y scripts de complementos de la puerta de enlace que se ejecuta en los nodos de destino. El servicio de puerta de enlace puede ampliarse para incluir complementos de puerta de enlace personalizados que admiten protocolos distintos de los valores predeterminados.

Estos complementos de puerta de enlace se incluyen de forma predeterminada con Windows Admin Center:

* Complemento de puerta de enlace de PowerShell
* Complemento de puerta de enlace WMI

Si desea comunicarse con un protocolo distinto de PowerShell o WMI, como con REST, puede crear su propio complemento de puerta de enlace.  Complementos de la puerta de enlace se cargan en un AppDomain independiente desde el proceso de puerta de enlace existente, pero usar el mismo nivel de elevación de derechos.

> [!NOTE]
> ¿No está familiarizado con los tipos de extensión diferente? Obtenga más información sobre la [tipos de arquitectura y la extensión de extensibilidad](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparar el entorno

Si no lo ha hecho ya, [preparar el entorno](prepare-development-environment.md) al instalar las dependencias y globales requisitos previos necesarios para todos los proyectos.

## <a name="create-a-gateway-plugin-c-library"></a>Crear un complemento de puerta de enlace (C# biblioteca)

Para crear un complemento de puerta de enlace personalizado, cree un nuevo C# clase que implementa el ```IPlugIn``` interfaz desde el ```Microsoft.ManagementExperience.FeatureInterfaces``` espacio de nombres.  

> [!NOTE]
> El ```IFeature``` interfaz, disponible en versiones anteriores del SDK, ahora está marcado como obsoleto.  Todas las programaciones de complemento de puerta de enlace deben usar IPlugIn (o, opcionalmente, la clase abstracta HttpPlugIn).

### <a name="download-sample-from-github"></a>Descargue el ejemplo de GitHub

Para empezar a trabajar rápidamente con un complemento de puerta de enlace personalizada, puede clonar o descargar una copia de nuestra [ejemplo C# proyecto de complemento](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) desde nuestro SDK de Windows Admin Center [sitio de GitHub](https://aka.ms/wacsdk).

### <a name="add-content"></a>Agregar contenido

Agregar nuevo contenido a la copia clonada de la [ejemplo C# proyecto de complemento](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) archivo de proyecto (o su propio proyecto) contiene las API personalizadas, a continuación, generar la DLL del complemento de puerta de enlace personalizada para su uso en los pasos siguientes.

### <a name="deploy-plugin-for-testing"></a>Implementar el complemento de prueba

Probar su complemento de puerta de enlace personalizada DLL cargándola en el proceso de puerta de enlace de Windows Admin Center.

Windows Admin Center busca todos los complementos en un ```plugins``` en la carpeta de datos de la aplicación de la máquina actual (con el valor CommonApplicationData de la enumeración Environment.SpecialFolder). En Windows 10, esta ubicación es ```C:\ProgramData\Server Management Experience```.  Si el ```plugins``` carpeta no existe aún, puede crear la carpeta.

> [!NOTE]
> Puede invalidar la ubicación del complemento en una compilación de depuración al actualizar el valor de configuración "StaticsFolder". Si depura localmente, esta opción está en el archivo App.Config de la solución de escritorio. 

Dentro de la carpeta de complementos (en este ejemplo, ```C:\ProgramData\Server Management Experience\plugins```)

* Cree una carpeta con el mismo nombre que el ```Name``` valor de propiedad de la ```Feature``` en la DLL del complemento de puerta de enlace personalizada (en nuestro proyecto de ejemplo, el ```Name``` es "Uno de ejemplo")
* Copie el archivo DLL de complemento de puerta de enlace personalizada para esta nueva carpeta
* Reinicie el proceso de Windows Admin Center

Una vez reiniciado el proceso de administración de Windows, podrá ejercer las API en la DLL del complemento de puerta de enlace personalizada mediante la emisión de una operación GET, PUT, PATCH, DELETE o publicar en ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Opcional: Adjuntar al complemento para la depuración

En Visual Studio 2017, en el menú Depurar, seleccione "Asociar al proceso". En la siguiente ventana, desplácese por la lista procesos disponibles y haga clic en "Adjuntar" SMEDesktop.exe. Una vez se inicia el depurador, puede colocar un punto de interrupción en el código de función y, a continuación, ejercicio mediante el formato de dirección URL anterior. Para nuestro proyecto de ejemplo (nombre de la característica: "Uno de ejemplo") en la dirección URL es: "http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Crear una extensión de la herramienta con la CLI de Windows Admin Center ##

Ahora es necesario crear una extensión de la herramienta se puede llamar a su complemento de puerta de enlace personalizada.  Cree o busque una carpeta donde desea almacenar los archivos de proyecto, abra un símbolo del sistema y establece dicha carpeta como el directorio de trabajo.  Mediante la CLI de Windows Admin Center que se instaló anteriormente, cree una nueva extensión con la sintaxis siguiente:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de su compañía (con espacios) | ```Contoso Inc``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```Manage Foo Works``` |

Observa el siguiente ejemplo de uso:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Esto crea una nueva carpeta en el directorio de trabajo actual con el nombre especificado para la herramienta, copia todos los archivos de plantilla necesarios en el proyecto y los archivos se configura con el nombre de empresa y la herramienta.  

A continuación, cambie el directorio a la carpeta que acaba de crear, a continuación, instalar las dependencias necesarias de locales, ejecute el comando siguiente:

```
npm install
```

Una vez que se complete, ha configurado todo lo que necesita para cargar la nueva extensión en Windows Admin Center. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Conectar su extensión con el complemento de puerta de enlace personalizada

Ahora que ha creado una extensión con la CLI de Windows Admin Center, está listo para conectar su extensión con el complemento de puerta de enlace personalizada siguiendo estos pasos:

- Agregar un [módulo vacío](guides\add-module.md)
- Use su [complemento de puerta de enlace personalizada](guides\use-custom-gateway-plugin.md) en su extensión
 
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