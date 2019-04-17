---
title: Publicación de extensiones para Windows Admin Center
description: Publicación de extensiones para Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783757"
---
# Publicar extensiones

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Una vez que hayas desarrollado la extensión, quieres publicarla y hacer que esté disponible para otros usuarios de prueba o usar. Según tu público y el propósito de publicación, hay varias opciones que es una introducción a continuación junto con los pasos y requisitos para la publicación.

## Opciones de publicación

Existen tres opciones principales de orígenes de paquete configurable que admite Windows Admin Center:
* Fuente pública Windows Admin Center NuGet de Microsoft
* Tu propio privada fuente de NuGet
* Local o recurso compartido de red

### Publicar en la extensión de Windows Admin Center de fuente

De manera predeterminada, Windows Admin Center está conectado a una NuGet fuente mantenida por el equipo de producto de Windows Admin Center de Microsoft. Las versiones anteriores de vista previa de nuevas extensiones desarrolladas por Microsoft pueden ser publicados a esta fuente y disponibles para los usuarios de Windows Admin Center. Los desarrolladores externos planificación crear y publicar extensiones públicamente también pueden [Enviar una solicitud](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar a esta fuente.

### Publicar en un NuGet diferentes de fuente

También puede crear tu propio NuGet de fuente para publicar sus extensiones en usando uno de muchos [distintas opciones para configurar un origen privado o con un NuGet que hospeda el servicio](https://docs.microsoft.com/nuget/hosting-packages/overview). La fuente de NuGet debe ser compatible con la API de v2 NuGet. Dado que Windows Admin Center no admite actualmente la autenticación del suministro, la fuente debe estar configurada para permitir el acceso de lectura a cualquier persona.

### Publicar en un recurso compartido de archivos

Para restringir el acceso de la extensión para la organización o a un grupo de personas limitado, puedes usar un recurso compartido de SMB como extensión de fuente. En este caso, se aplicará los permisos de carpeta y recursos compartidos de archivos para permitir el acceso a la fuente.

## Preparar la extensión de versión

Asegúrate de leer y tener en cuenta los siguientes temas de desarrollo:

- [Controlar la visibilidad de la herramienta](guides/dynamic-tool-display.md)
- [Cadenas y localización](guides/strings-localization.md)

### Piensa en lanzar como una versión preliminar

Si estás lanzando una versión preliminar de la extensión para fines de evaluación, te recomendamos que puedes:

- Anexa "(versión preliminar)" al final del título de la extensión en el archivo .nuspec
- Explica las limitaciones en la descripción de la extensión en el archivo .nuspec

## Crear un paquete de extensión

Windows Admin Center usa paquetes de NuGet y fuentes para distribuir y descargar las extensiones.  En el orden para el paquete que se envíen, tendrás que generar un paquete de NuGet que contiene los complementos y extensiones.  Un único paquete puede contener una extensión de la interfaz de usuario así como un complemento de puerta de enlace y en la siguiente sección te guiará por el proceso.

### 1. compilar la extensión

Tan pronto como esté listo para iniciar la extensión de empaquetado, crea un nuevo directorio en el sistema de archivos, abre una consola y CD en él.  Esta será el directorio raíz que se usarán para contener todos los directorios nuspec y el contenido que se componen de nuestro paquete.  Durante la vigencia de este documento se hará referencia a esta carpeta como "Paquete de NuGet".

#### Extensiones de la interfaz de usuario

Para comenzar el proceso de recopilación de todo el contenido necesario para una extensión de la interfaz de usuario, ejecute "gulp compilación" en la herramienta y asegúrese de que la compilación es correcta.  Paquetes de este proceso todos los componentes juntos en una carpeta denominada "conjunto" que se encuentran en el directorio raíz de la extensión (del mismo nivel del directorio src).  Copia este directorio y todo lo del contenido en la carpeta "Paquete de NuGet".

#### Complementos de puerta de enlace

Con la infraestructura de compilación (puede ser tan simple como abrir en Visual Studio y haciendo clic en el botón de compilación), compilar y crear el complemento.  Abre el directorio de salida de compilación y copiar los archivos DLL que representan tu complemento y colocarlos en una nueva carpeta dentro del directorio "Paquete de NuGet" llamado "paquete".  No es necesario copiar el archivo dll FeatureInterface, solo los archivos DLL que representan el código.

### 2. crear el archivo .nuspec

Para crear el paquete de NuGet, debes crear primero un archivo .nuspec. Un archivo .nuspec es un manifiesto XML que contiene metadatos del paquete NuGet. Este manifiesto se usa para crear el paquete y proporcionar información a los consumidores.  Coloque este archivo en la raíz de la carpeta "Paquete de NuGet".

Este es un ejemplo de archivo .nuspec y la lista de propiedades necesarios o recomendadas. Para el esquema completo, consulta la [referencia de .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Guarda el archivo .nuspec en la carpeta del proyecto raíz con un nombre de archivo de tu elección.

> [!IMPORTANT]
> El ```<id>``` valor en el archivo .nuspec debe coincidir con el ```"name"``` valor en el proyecto ```manifest.json``` archivo, o bien la extensión publicada no se pudo cargar correctamente en Windows Admin Center.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### Recomendado propiedades o requerido

| Nombre de propiedad | Necesarios / recomendado | Descripción |
| ---- | ---- | ---- |
| packageType | Obligatorio | Usar "WindowsAdminCenterExtension", que es el tipo de paquete de NuGet definido para las extensiones de Windows Admin Center. |
| id | Obligatorio | Identificador único de paquete dentro de la fuente. Este valor debe coincidir con el valor de "name" en el archivo del proyecto manifest.json.  Para obtener instrucciones, consulta la [elección de un identificador único del paquete](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) . |
| title | Necesarios para la publicación en el centro de administración de Windows de fuente | Nombre descriptivo para el paquete que se muestra en Windows Admin Center extensión Manager. |
| version | Obligatorio | Versión de extensión. Uso de [Control de versiones semántica (SemVer convención)](http://semver.org/spec/v1.0.0.html) se recomienda, aunque no es necesario. |
| autores | Obligatorio | Si se publica en nombre de tu empresa, usa el nombre de la empresa. |
| description | Obligatorio | Proporciona una descripción de la funcionalidad de la extensión. |
| iconUrl | Recomendado cuando se publica en el centro de administración de Windows de fuente | Dirección URL de icono para mostrar en el Administrador de extensiones. |
| projectUrl | Necesarios para la publicación en el centro de administración de Windows de fuente | Dirección URL de sitio Web de la extensión. Si no tienes un sitio Web separado, usa la dirección URL de la página Web de paquete en la fuente de NuGet. |
| licenseUrl | Necesarios para la publicación en el centro de administración de Windows de fuente | Dirección URL de contrato de licencia de usuario final de la extensión. |
| archivos | Obligatorio | Estos dos valores de configuración de la estructura de carpetas que espera Windows Admin Center para las extensiones de la interfaz de usuario y los complementos de puerta de enlace. |

### 3. compilar el paquete de NuGet de extensión

Con el archivo .nuspec creado anteriormente, ahora creará el archivo de .nupkg del paquete de NuGet que puede cargar y publicar en la fuente de NuGet.

1. Descargar la herramienta CLI nuget.exe desde el [sitio Web de herramientas de cliente de NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Ejecuta "nuget.exe pack [nombre de archivo .nuspec]" para crear el archivo .nupkg.

### 4. firmar el paquete de NuGet de extensión

Cualquier archivo .dll que se incluyen en la extensión se necesitan que firmarse con un certificado de una entidad de confianza de certificación (CA). De manera predeterminada, se bloquearán los archivos .dll sin firmar se ejecute cuando Windows Admin Center se ejecuta en modo de producción.

También recomendamos que firmar el paquete de NuGet de extensión para garantizar la integridad del paquete, pero esto no es un paso necesario.

### 5. probar el paquete de NuGet de extensión

El paquete de extensión ahora está listo para las pruebas. Carga el archivo .nupkg a una fuente de NuGet o copiar a un recurso compartido de archivos. Para ver y descargar paquetes de una fuente diferente o un recurso compartido de archivos, tendrás que para [cambiar la configuración de fuente](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para que apunte a la fuente de NuGet o el archivo de compartir. Cuando se prueba, asegúrate de que las propiedades se muestran correctamente en el Administrador de extensiones y puede instalar y desinstalar la extensión correctamente.

## Publicar la extensión en el centro de administración de Windows de fuente

Mediante la publicación en el centro de administración de Windows de fuente, puedes hacer la extensión disponible para cualquier usuario de Windows Admin Center. Dado que el SDK de Windows Admin Center todavía está en vista previa, nos gustaría trabajar estrechamente con usted para ayudar a resolver los problemas de desarrollo y, asegúrese de que eres capaz de proporcionar un producto de calidad y de la experiencia a los usuarios.

Antes de liberar la versión inicial de la extensión, te recomendamos que enviar una solicitud de revisión de extensión a Microsoft al menos 2-3 semanas antes de la versión para garantizar que tenemos tiempo suficiente para revisar y para que pueda realizar cambios en la extensión, si es necesario. Una vez que la extensión está lista para publicarse, tendrás que enviar nos para revisión y, si se aprueba, publicaremos lo a la fuente para TI.

Después, si quieres publicar una actualización de la extensión, tendrás que enviar otra solicitud de revisión. Si bien según el ámbito de cambio, el tiempo de respuesta de opiniones de la actualización por lo general, debe ser más corto.

### Enviar una solicitud de revisión de extensión a Microsoft

Para enviar una solicitud de revisión de la extensión, escribe la siguiente información y enviar por correo electrónico a [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Te responderá a tu correo electrónico dentro de una semana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### Enviar el paquete de extensión de revisión y publicación

Asegúrate de seguir las instrucciones anteriores para [crear un paquete de extensión](#creating-an-extension-package) y el archivo .nuspec se define correctamente y los archivos están firmados. También te recomendamos que tienes un sitio Web de proyecto incluido lo siguiente:

- Descripción detallada de la extensión como capturas de pantalla o vídeo
- Características de sitio Web o la dirección de correo electrónico para recibir comentarios o preguntas

Cuando estés listo para publicar la extensión, enviar un correo electrónico a [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) y proporcionaremos instrucciones sobre cómo enviar el paquete de la extensión. Una vez que recibimos el paquete, revisaremos y si se aprueba, publicar en el centro de administración de Windows de fuente.