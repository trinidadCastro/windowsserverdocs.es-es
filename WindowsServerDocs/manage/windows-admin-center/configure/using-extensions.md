---
title: Instalar y administrar extensiones
description: Instalar y administrar las extensiones en Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296707"
---
# Instalar y administrar extensiones

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Windows Admin Center se ha creado como una plataforma extensible donde cada tipo de conexión y la herramienta es una extensión que se puede instalar, desinstalar y actualizar de forma individual. Puede buscar nuevas extensiones publicadas por Microsoft y otros desarrolladores e instalar y actualizar ellos individualmente sin tener que actualizar toda la instalación de Windows Admin Center. También puedes configurar una fuente de NuGet independiente o recurso compartido de archivos y distribuir las extensiones para uso interno dentro de la organización.

## Instalar una extensión

Windows Admin Center se mostrarán extensiones disponibles desde la fuente de NuGet especificado. De manera predeterminada, Windows Admin Center apunta a la NuGet oficial de Microsoft de fuentes que hospeda las extensiones publicadas por Microsoft y otros desarrolladores.

1. Haz clic en el botón de **configuración** en la parte superior derecha > en el panel izquierdo, haz clic en **las extensiones**. 
2. La pestaña **Extensiones disponibles** la lista de las extensiones en la fuente que están disponibles para su instalación.
3. Haz clic en una extensión para ver la descripción de la extensión, versión, publisher y otra información en el panel de **Detalles** .
4. Haz clic en **instalar** para instalar una extensión. Si la puerta de enlace debe ejecutarse en modo con privilegios elevados para realizar este cambio, aparecerá un mensaje de elevación de UAC. Una vez completada la instalación, se actualizarán automáticamente el explorador y se volverá a cargar en Windows Admin Center con la nueva extensión instalada. Si la extensión que está intentando instalar es una actualización de una extensión previamente instalada, puedes hacer clic en el botón **actualizar a la última versión** para instalar la actualización. También puede ir a la pestaña **Extensiones instaladas** para ver extensiones instaladas y comprobar si una actualización está disponible en la columna **estado** .

## Instalar las extensiones de una fuente diferente

Windows Admin Center es compatible con varias fuentes y puedes ver y administrar paquetes de más de una fuente a la vez. Fuente de cualquier NuGet que admite la API de NuGet V2 o un recurso compartido de archivos se puede agregar al centro de administración de Windows para la instalación de las extensiones de.

1. Haz clic en el botón de **configuración** en la parte superior derecha > en el panel izquierdo, haz clic en **las extensiones**.
2. En el panel derecho, haz clic en la pestaña de **fuentes** .
3. Haz clic en el botón de **Agregar** para agregar otra fuente. Una fuente de NuGet, escribe el V2 NuGet dirección URL de fuente. El NuGet fuente a proveedor o administrador debe ser capaz de proporcionar la información de la dirección URL. Para un recurso compartido de archivo, escribe la ruta de acceso completa del archivo de recurso compartido en el que se almacenan los archivos del paquete de extensión (.nupkg).
4. Haz clic en **Agregar**. Si la puerta de enlace debe ejecutarse en modo con privilegios elevados para realizar este cambio, aparecerá un mensaje de elevación de UAC.

La lista de **Extensiones disponibles** mostrará las extensiones de todas las fuentes registradas. Puedes comprobar qué fuente cada extensión es desde mediante la columna de **Paquete de la fuente** .

## Desinstalar una extensión

Puedes desinstalar las extensiones que se instaló anteriormente, o incluso desinstalar las herramientas que se instalaron previamente como parte de la instalación de Windows Admin Center.

1. Haz clic en el botón de **configuración** en la parte superior derecha > en el panel izquierdo, haz clic en **las extensiones**. 
2. Haz clic en la pestaña **Extensiones instaladas** para ver las extensiones instaladas todas.
3. Elegir una extensión para desinstalar y luego haga clic en **desinstalar**.

Después de la desinstalación, se actualizarán automáticamente el explorador y de Windows Admin Center se volverá a cargar sin la extensión. Si se desinstala una herramienta que instaló previamente como parte de Windows Admin Center, la herramienta estará disponible para volver a instalar en la pestaña **Extensiones disponibles** .

## Instalar las extensiones en un equipo sin conectividad a internet

Si está instalado Windows Admin Center en un equipo que no está conectado a internet o está detrás de un servidor proxy, no puede tener acceso a instalar las extensiones de la fuente de Windows Admin Center. Puedes descargar paquetes de extensión manualmente o con un script de PowerShell y configurar Windows Admin Center para recuperar paquetes de un recurso compartido de archivos o una unidad local.

### Descargar manualmente los paquetes de extensión

1. En otro equipo que tiene conectividad a internet, abre un navegador web y navegar a la siguiente dirección URL:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Debes crear una cuenta en msft-sme.myget.org e inicio de sesión para ver los paquetes de extensión.

2. Haz clic en el nombre del paquete que quieres instalar para ver la página de detalles del paquete.
3. Haz clic en el vínculo de **descarga** en el panel del lado derecho de la página de detalles del paquete y descargar el archivo .nupkg para la extensión.
4. Repite los pasos 2 y 3 para todos los paquetes que quieres descargar.
5. Copiar los archivos del paquete en un recurso compartido de archivo que se puede acceder desde el equipo que está instalado Windows Admin Center, o en el disco local del equipo.
6. [Sigue las instrucciones para instalar las extensiones de una fuente diferente](#installing-extensions-from-a-different-feed).

### Descargar paquetes con un script de PowerShell

Hay muchas secuencias de comandos disponible en Internet para descargar paquetes de NuGet desde una fuente de NuGet. Vamos a utilizar el [script proporcionado por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Administrador de programas Senior de Microsoft.

1. Como se describe en la [entrada de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instalar la secuencia de comandos como un paquete de NuGet o copiar y pegar el script en PowerShell ISE.
2. Editar que la primera línea de la secuencia para tu NuGet fuente 's URL v2. Si va a descargar paquetes desde el centro de administración de Windows oficial de fuente, usa la siguiente dirección URL.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Ejecuta el script y todos los paquetes de NuGet que descargue desde la fuente en la siguiente carpeta local: %USERPROFILE%\Documents\NuGetLocal
4. [Sigue las instrucciones para instalar las extensiones de una fuente diferente](#installing-extensions-from-a-different-feed).

## Administrar extensiones con PowerShell

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Versión preliminar de Windows Admin Center incluye un módulo de PowerShell para administrar las extensiones de puerta de enlace.

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### [Más información sobre la creación de una extensión con el SDK de Windows Admin Center](../extend/extensibility-overview.md).