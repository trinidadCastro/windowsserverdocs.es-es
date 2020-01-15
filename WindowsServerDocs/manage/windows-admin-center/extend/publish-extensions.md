---
title: Publicar extensiones para el centro de administración de Windows
description: Publicar extensiones para el centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0182c4097ec3bc4432e2ba408d701a72d82a7c8d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950083"
---
# <a name="publishing-extensions"></a>Extensiones de publicación

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Una vez que haya desarrollado la extensión, querrá publicarla y ponerla a disposición de otros usuarios para que la prueben o usen. En función de la audiencia y el propósito de la publicación, existen algunas opciones que se presentan a continuación, junto con los pasos y los requisitos para la publicación.

## <a name="publishing-options"></a>Opciones de publicación

Existen tres opciones principales para los orígenes de paquetes configurables que el centro de administración de Windows admite:
* Fuente de NuGet del centro de administración público de Windows de Microsoft
* Su propia fuente de NuGet privada
* Recurso compartido de archivos de red o local

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publicar en la fuente de extensión del centro de administración de Windows

De forma predeterminada, el centro de administración de Windows está conectado a una fuente de NuGet mantenida por el equipo de productos del centro de administración de Windows en Microsoft. Las primeras versiones preliminares de las nuevas extensiones desarrolladas por Microsoft pueden publicarse en esta fuente y estar disponibles para los usuarios del centro de administración de Windows. Los desarrolladores externos que planean compilar y liberar extensiones públicamente también pueden [enviar una solicitud](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar en esta fuente.

### <a name="publishing-to-a-different-nuget-feed"></a>Publicación en una fuente de NuGet diferente

También puede crear su propia fuente de NuGet para publicar sus extensiones en usando una de las muchas [opciones diferentes para configurar una fuente privada o mediante un servicio de hospedaje de NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). La fuente de NuGet debe admitir la API de NuGet V2. Puesto que el centro de administración de Windows no admite actualmente la autenticación de fuentes, la fuente debe estar configurada para permitir el acceso de lectura a todos los usuarios.

### <a name="publishing-to-a-file-share"></a>Publicar en un recurso compartido de archivos

Para restringir el acceso de la extensión a su organización o a un grupo limitado de personas, puede usar un recurso compartido de archivos SMB como una fuente de extensión. En este caso, se aplicarán los permisos de carpetas y recursos compartidos de archivos para permitir el acceso a la fuente.

## <a name="preparing-your-extension-for-release"></a>Preparar la extensión para la versión

Asegúrese de leer y tener en cuenta los siguientes temas de desarrollo:

- [Controlar la visibilidad de la herramienta](guides/dynamic-tool-display.md)
- [Cadenas y localización](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Considere la posibilidad de publicar como versión preliminar

Si está lanzando una versión preliminar de la extensión para fines de evaluación, se recomienda:

- Anexe "(vista previa)" al final del título de la extensión en el archivo. nuspec.
- Explicar las limitaciones de la descripción de la extensión en el archivo. nuspec

## <a name="creating-an-extension-package"></a>Crear un paquete de extensión

El centro de administración de Windows emplea paquetes y fuentes de NuGet para distribuir y descargar extensiones.  Para poder enviar el paquete, debe generar un paquete de NuGet que contenga los complementos y las extensiones.  Un solo paquete puede contener una extensión de interfaz de usuario, así como un complemento de puerta de enlace, y la siguiente sección le guiará a través del proceso.

### <a name="1-build-your-extension"></a>1. cree su extensión

En cuanto esté listo para empezar a empaquetar la extensión, cree un nuevo directorio en el sistema de archivos, abra una consola de y el CD en él.  Este será el directorio raíz que se utilizará para contener todos los directorios de contenido y nuspec que compondrán el paquete.  Haremos referencia a esta carpeta como "paquete NuGet" mientras dure este documento.

#### <a name="ui-extensions"></a>Extensiones de la interfaz de usuario

Para comenzar el proceso de recopilación de todo el contenido necesario para una extensión de interfaz de usuario, ejecute "Gulp Build" en la herramienta y asegúrese de que la compilación se ha realizado correctamente.  Este proceso empaqueta todos los componentes en una carpeta denominada "bundle" que se encuentra en el directorio raíz de la extensión (en el mismo nivel del directorio SRC).  Copie este directorio y todo su contenido en la carpeta "NuGet Package".

#### <a name="gateway-plugins"></a>Complementos de puerta de enlace

Con la infraestructura de compilación (esto podría ser tan sencillo como abrir Visual Studio y hacer clic en el botón compilar), compilar y compilar el complemento.  Abra el directorio de salida de la compilación y copie los archivos DLL que representan el complemento, y colóquelos en una nueva carpeta dentro del directorio del "paquete NuGet" denominado "paquete".  No es necesario copiar el archivo dll de FeatureInterface, solo los archivos DLL que representan el código.

### <a name="2-create-the-nuspec-file"></a>2. crear el archivo. nuspec

Para crear el paquete NuGet, primero debe crear un archivo. nuspec. Un archivo. nuspec es un manifiesto XML que contiene metadatos del paquete NuGet. Este manifiesto se usa para crear el paquete y para proporcionar información a los consumidores.  Coloque este archivo en la raíz de la carpeta "paquete NuGet".

A continuación se muestra un archivo. nuspec de ejemplo y la lista de propiedades necesarias o recomendadas. Para obtener el esquema completo, consulte la [referencia de. nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Guarde el archivo. nuspec en la carpeta raíz del proyecto con un nombre de archivo de su elección.

> [!IMPORTANT]
> El valor ```<id>``` del archivo. nuspec debe coincidir con el valor ```"name"``` del archivo ```manifest.json``` del proyecto o, de lo contrario, la extensión publicada no se cargará correctamente en el centro de administración de Windows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="https://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
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

#### <a name="required-or-recommended-properties"></a>Propiedades necesarias o recomendadas

| Nombre de propiedad | Requerido/recomendado | Descripción |
| ---- | ---- | ---- |
| packageType | Necesario | Use "WindowsAdminCenterExtension", que es el tipo de paquete de NuGet definido para las extensiones del centro de administración de Windows. |
| id | Necesario | Identificador de paquete único dentro de la fuente. Este valor debe coincidir con el valor "Name" en el archivo manifest. JSON del proyecto.  Vea [Choosing a unique package identifier](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) (Elegir un identificador de paquete único) para obtener instrucciones. |
| title | Se requiere para la publicación en la fuente del centro de administración de Windows | Nombre descriptivo para el paquete que se muestra en el administrador de extensiones del centro de administración de Windows. |
| Versión de | Necesario | Versión de la extensión. Se recomienda usar el [control de versiones semántico (Convención SemVer)](http://semver.org/spec/v1.0.0.html) , pero no es necesario. |
| authors | Necesario | Si publica en nombre de su empresa, use el nombre de su empresa. |
| descripción | Necesario | Proporcione una descripción de la funcionalidad de la extensión. |
| iconUrl | Recomendado al publicar en la fuente del centro de administración de Windows | Dirección URL del icono que se va a mostrar en el administrador de extensiones. |
| projectUrl | Se requiere para la publicación en la fuente del centro de administración de Windows | Dirección URL del sitio web de la extensión. Si no tiene un sitio web independiente, utilice la dirección URL de la página web del paquete en la fuente de NuGet. |
| licenseUrl | Se requiere para la publicación en la fuente del centro de administración de Windows | Dirección URL del contrato de licencia para el usuario final de la extensión. |
| archivos | Necesario | Estos dos valores configuran la estructura de carpetas que el centro de administración de Windows espera para las extensiones de la interfaz de usuario y los complementos de puerta de enlace. |

### <a name="3-build-the-extension-nuget-package"></a>3. compilar el paquete NuGet de la extensión

Con el archivo. nuspec que creó anteriormente, ahora creará el archivo. nupkg de paquete NuGet que puede cargar y publicar en la fuente de NuGet.

1. Descargue la herramienta de la CLI de Nuget. exe desde el [sitio web de herramientas de cliente de Nuget](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Ejecute "Nuget. exe Pack [. nuspec File name]" para crear el archivo. nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. firmar el paquete NuGet de la extensión

Los archivos. dll incluidos en la extensión deben estar firmados con un certificado de una entidad de certificación (CA) de confianza. De forma predeterminada, los archivos. dll sin firmar se bloquearán para que no se ejecuten cuando el centro de administración de Windows se ejecute en modo de producción.

También recomendamos encarecidamente que firme el paquete NuGet de la extensión para garantizar la integridad del paquete, pero este no es un paso necesario.

### <a name="5-test-your-extension-nuget-package"></a>5. probar el paquete NuGet de la extensión

El paquete de extensión ya está listo para las pruebas. Cargue el archivo. nupkg en una fuente de NuGet o cópielo en un recurso compartido de archivos. Para ver y descargar paquetes de una fuente o recurso compartido de archivos diferente, deberá [cambiar la configuración de la fuente](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para que apunte a la fuente de NuGet o al recurso compartido de archivos. Al realizar las pruebas, asegúrese de que las propiedades se muestran correctamente en el administrador de extensiones y puede instalar y desinstalar correctamente la extensión.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Publicar la extensión en la fuente del centro de administración de Windows

Al publicar en la fuente del centro de administración de Windows, puede hacer que la extensión esté disponible para cualquier usuario del centro de administración de Windows. Dado que el SDK del centro de administración de Windows todavía está en versión preliminar, nos gustaría trabajar en estrecha colaboración con usted para ayudarle a resolver los problemas de desarrollo y asegurarse de que puede ofrecer un producto de calidad y una experiencia a los usuarios.

Antes de publicar la versión inicial de la extensión, se recomienda que envíe una solicitud de revisión de extensión a Microsoft al menos 2-3 semanas antes del lanzamiento para asegurarse de que tenemos tiempo suficiente para revisar y realizar cambios en la extensión si es necesario. Una vez que la extensión esté lista para publicarse, deberá enviarla a nosotros para su revisión y, si la aprueba, la publicaremos en la fuente.

Posteriormente, si desea publicar una actualización de la extensión, deberá enviar otra solicitud de revisión. Aunque según el ámbito del cambio, el tiempo de respuesta para las revisiones de actualización debe ser generalmente más corto.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Enviar una solicitud de revisión de extensión a Microsoft

Para enviar una solicitud de revisión de extensión, escriba la siguiente información y envíela como un correo electrónico a [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Responderemos a su correo electrónico dentro de una semana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>Enviar el paquete de extensión para su revisión y publicación

Asegúrese de seguir las instrucciones anteriores para [crear un paquete de extensión](#creating-an-extension-package) y de que el archivo. nuspec esté definido correctamente y los archivos estén firmados. También se recomienda que tenga un sitio web de proyecto que incluya lo siguiente:

- Descripción detallada de la extensión, incluidas las capturas de pantalla o el vídeo
- Dirección de correo electrónico o característica de sitio web para recibir comentarios o preguntas

Cuando esté listo para publicar la extensión, envíe un correo electrónico a [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) y proporcionaremos instrucciones sobre cómo enviarnos el paquete de extensión. Una vez que recibamos el paquete, revisaremos y, si lo aprueba, publicaremos en la fuente del centro de administración de Windows.