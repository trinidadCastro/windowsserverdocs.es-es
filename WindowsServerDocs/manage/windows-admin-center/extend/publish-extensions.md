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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820426"
---
# <a name="publishing-extensions"></a>Publicación de extensiones

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Una vez que ha desarrollado su extensión, desea publicarlo y ponerla a disposición de otros usuarios para probar o utilizar. Dependiendo de su audiencia y propósito de la publicación, hay algunas opciones que presentaremos a continuación, junto con los pasos y requisitos para la publicación.

## <a name="publishing-options"></a>Opciones de publicación

Hay tres opciones principales para orígenes de paquetes configurables que admita Windows Admin Center:
* Fuente de NuGet pública Windows Admin Center de Microsoft
* Su propia fuente NuGet privado
* Local o recurso compartido de red

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publicar en la extensión de Windows Admin Center fuente

De forma predeterminada, Windows Admin Center está conectado a NuGet fuente mantenida por el equipo del producto Windows Admin Center en Microsoft. Las primeras versiones de vista previa de las extensiones nuevas desarrolladas por Microsoft pueden ser publicado a esta fuente y está disponible para los usuarios de Windows Admin Center. Los desarrolladores externos, planeación de la compilación y versión extensiones públicamente, es posible que también [enviar una solicitud](#publishing-your-extension-to-the-windows-admin-center-feed) para publicar en esta fuente.

### <a name="publishing-to-a-different-nuget-feed"></a>Publicación en NuGet diferentes fuentes de distribución

También puede crear su propia fuente NuGet para publicar sus extensiones a uno de los muchos [diferentes opciones para configurar un origen privado o mediante un servicio de hospedaje de NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). La fuente NuGet debe admitir la API de NuGet v2. Puesto que Windows Admin Center no es compatible con las fuentes autenticación, la fuente debe configurarse para permitir el acceso de lectura a cualquier persona.

### <a name="publishing-to-a-file-share"></a>Publicar en un recurso compartido de archivos

Para restringir el acceso de la extensión de la organización o a un grupo limitado de personas, puede usar un recurso compartido de archivos SMB como una extensión de fuente. En este caso, se aplicará a los permisos de carpetas y recursos compartidos de archivos para permitir el acceso a la fuente.

## <a name="preparing-your-extension-for-release"></a>Preparar la extensión para su lanzamiento

Asegúrese de leer y tenga en cuenta los siguientes temas de desarrollo:

- [Controlar la visibilidad de la herramienta](guides/dynamic-tool-display.md)
- [Las cadenas y localización](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Considere la posibilidad de liberar como una versión preliminar

Si va a publicar una versión de vista previa de la extensión para fines de evaluación, se recomienda:

- Anexe "(versión preliminar)" al final del título de la extensión en el archivo .nuspec
- Explica las limitaciones en la descripción de la extensión en el archivo .nuspec

## <a name="creating-an-extension-package"></a>Creación de un paquete de extensión

Windows Admin Center utiliza paquetes de NuGet y las fuentes de distribución y la descarga de extensiones.  En orden para el paquete que se van a enviar, deberá generar un paquete de NuGet que contiene sus complementos y extensiones.  Un solo paquete puede contener una extensión de interfaz de usuario así como un complemento de puerta de enlace y la siguiente sección le guiará a través del proceso.

### <a name="1-build-your-extension"></a>1. Compilar la extensión

Tan pronto como esté listo para comenzar a empaquetar la extensión, cree un nuevo directorio en el sistema de archivos, abra una consola y CD en él.  Este será el directorio raíz que se usará para contener todos los directorios que formarán parte de nuestro paquete de nuspec y contenido.  Esta carpeta se hará referencia como "Paquete de NuGet" para la duración de este documento.

#### <a name="ui-extensions"></a>Extensiones de interfaz de usuario

Para comenzar el proceso de recopilación de todo el contenido necesario para una extensión de interfaz de usuario, ejecute "gulp compilación" en la herramienta y asegúrese de que la compilación es correcta.  Paquetes de este proceso todos los componentes juntos en una carpeta denominada "agrupar" se encuentran en el directorio raíz de la extensión (en el mismo nivel del directorio src).  Copie este directorio y todo lo del contenido en la carpeta "Paquete de NuGet".

#### <a name="gateway-plugins"></a>Complementos de la puerta de enlace

Con la infraestructura de compilación (puede ser tan sencillo como abrir Visual Studio y hacer clic en el botón de compilación), compile y compilar el complemento.  Abra el directorio de salida de compilación y copiar los archivos DLL que representan el complemento y colocarlas en una carpeta nueva dentro del directorio "Paquete de NuGet" denominado "paquete".  No es necesario copiar la dll FeatureInterface, solo los archivos DLL que representan el código.

### <a name="2-create-the-nuspec-file"></a>2. Crear el archivo .nuspec

Para crear el paquete de NuGet, deberá crear primero un archivo .nuspec. Un archivo .nuspec es un manifiesto XML que contiene los metadatos del paquete NuGet. Este manifiesto se usa para generar el paquete y proporcionar información a los consumidores.  Coloque este archivo en la raíz de la carpeta "Paquete de NuGet".

Este es un archivo .nuspec de ejemplo y la lista de propiedades necesarias o recomendadas. Para el esquema completo, vea el [referencia de .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Guarde el archivo .nuspec a la carpeta del proyecto raíz con un nombre de archivo de su elección.

> [!IMPORTANT]
> El ```<id>``` debe coincidir con el valor en el archivo .nuspec el ```"name"``` valor en el proyecto ```manifest.json``` archivo, o bien su extensión publicada no se pudo cargar correctamente en Windows Admin Center.

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

#### <a name="required-or-recommended-properties"></a>Necesarios o recomendados propiedades

| Nombre de la propiedad | Se recomienda / necesarios | Descripción |
| ---- | ---- | ---- |
| packageType | Requerido | Use "WindowsAdminCenterExtension", que es el tipo de paquete de NuGet definido para las extensiones de Windows Admin Center. |
| id | Requerido | Identificador de paquete único dentro de la fuente. Este valor debe coincidir con el valor "name" en el archivo manifest.json del proyecto.  Consulte [elección de un identificador de paquete único](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) para obtener instrucciones. |
| title | Necesario para la publicación en el de Windows Admin Center fuente | Nombre descriptivo para el paquete que se muestra en el Administrador de extensiones de Windows Admin Center. |
| version | Requerido | Versión de la extensión. Uso de [Versionamiento semántico (SemVer convention)](http://semver.org/spec/v1.0.0.html) es recomendable, pero no es necesario. |
| Autores | Requerido | Si se publica en el nombre de su empresa, use el nombre de la empresa. |
| description | Requerido | Proporcione una descripción de la funcionalidad de la extensión. |
| iconUrl | Recomendado cuando se publica en el de Windows Admin Center fuente | Dirección URL de icono para mostrar en el Administrador de extensiones. |
| projectUrl | Necesario para la publicación en el de Windows Admin Center fuente | Dirección URL al sitio Web de la extensión. Si no tiene un sitio Web independiente, use la dirección URL de la página Web de paquete en la fuente de NuGet. |
| licenseUrl | Necesario para la publicación en el de Windows Admin Center fuente | Dirección URL para el contrato de licencia de usuario final de la extensión. |
| archivos | Requerido | Estos dos valores de configuración de la estructura de carpetas que Windows Admin Center espera para las extensiones de interfaz de usuario y los complementos de puerta de enlace. |

### <a name="3-build-the-extension-nuget-package"></a>3. Crear el paquete de extensión de NuGet

Mediante el archivo .nuspec que creó anteriormente, ahora creará el archivo .nupkg del paquete NuGet que puede cargar y publicar en la fuente de NuGet.

1. Descargue la herramienta CLI de nuget.exe desde el [sitio Web de herramientas de cliente de NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Para crear el archivo .nupkg, ejecute "nuget.exe pack [nombre de archivo .nuspec]".

### <a name="4-signing-your-extension-nuget-package"></a>4. Firma el paquete NuGet de extensión

Cualquier archivo .dll que se incluyen en la extensión se debe estar firmado con un certificado de una autoridad de certificado de confianza (CA). De forma predeterminada, se bloqueará los archivos .dll sin signo que se ejecute cuando se está ejecutando Windows Admin Center en modo de producción.

También recomendamos que firmar el paquete de extensión de NuGet para garantizar la integridad del paquete, pero esto no es un paso necesario.

### <a name="5-test-your-extension-nuget-package"></a>5. Probar el paquete NuGet de extensión

Paquete de extensión ya está listo para probarlo. Cargue el archivo .nupkg en una fuente NuGet o cópielo en un recurso compartido de archivos. Para ver y descargar paquetes desde una fuente diferente o el recurso compartido de archivos, deberá [cambiar la configuración de fuente](../configure/using-extensions.md#installing-extensions-from-a-different-feed) para que señale a la fuente de NuGet o recurso compartido de archivos. Al realizar pruebas, asegúrese de que las propiedades se muestran correctamente en el Administrador de extensiones y puede instalar y desinstalar la extensión correctamente.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Publicar la extensión en el Windows Admin Center fuente

Al realizar la publicación a la de Windows Admin Center fuente, puede hacer la extensión disponible para cualquier usuario de Windows Admin Center. Puesto que el SDK de Windows Admin Center todavía está en versión preliminar, nos gustaría trabajar estrechamente con usted para ayudar a resolver problemas de desarrollo, asegúrese de que puede proporcionar un producto de calidad y experiencia a los usuarios.

Antes de lanzar la versión inicial de la extensión, se recomienda enviar una solicitud de revisión de la extensión a Microsoft al menos 2-3 semanas antes de la versión para asegurarse de que tiene suficiente tiempo para revisar y para que pueda realizar cambios en la extensión, si es necesario. Una vez que la extensión está lista para publicarse, deberá enviarla para su revisión y, si se aprueba, se podrá publicar en la fuente para.

Después, si desea publicar una actualización a la extensión, deberá enviar otra solicitud para su revisión. Mientras según el ámbito de cambio, el tiempo de respuesta para las revisiones de actualización debe ser generalmente más corto.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Enviar una solicitud de revisión de la extensión a Microsoft

Para enviar una solicitud de revisión de la extensión, escriba la siguiente información y enviar un correo electrónico a [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Responderemos a su correo electrónico dentro de una semana.

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

Asegúrese de seguir las instrucciones anteriores para [crear un paquete de extensión](#creating-an-extension-package) y el archivo .nuspec se ha definido correctamente y se firman los archivos. Se recomienda también que tiene un sitio Web del proyecto incluido lo siguiente:

- Descripción detallada de la extensión como capturas de pantalla o de vídeo
- Característica de dirección o el sitio Web de correo electrónico para recibir comentarios o preguntas

Cuando esté listo para publicar su extensión, envíe un correo electrónico a [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) y proporcionaremos instrucciones sobre cómo enviar el paquete de extensión. Una vez que recibamos su paquete, revisaremos y si se aprueba, publicar en el de Windows Admin Center fuente.