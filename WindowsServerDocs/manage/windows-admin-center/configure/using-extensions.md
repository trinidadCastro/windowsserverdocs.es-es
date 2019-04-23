---
title: Instalar y administrar extensiones
description: Instalar y administrar extensiones de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6091edd7aa7f790f6029ca6b6ae402bf1b7e61ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877026"
---
# <a name="install-and-manage-extensions"></a>Instalar y administrar extensiones

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Windows Admin Center se compila como una plataforma extensible donde cada tipo de conexión y la herramienta es una extensión que puede instalar, desinstalar y actualizar individualmente. Puede buscar si hay nuevas extensiones publicadas por Microsoft y otros desarrolladores y puede instalar y actualizarlas sin tener que actualizar toda la instalación de Windows Admin Center. También puede configurar una fuente NuGet independiente o recurso compartido de archivos y distribuir extensiones para uso interno dentro de su organización.

## <a name="installing-an-extension"></a>Instalar una extensión

Windows Admin Center mostrará las extensiones disponibles desde la fuente de NuGet especificado. De forma predeterminada, Windows Admin Center apunta a la fuente de NuGet oficial de Microsoft que hospeda las extensiones publicadas por Microsoft y otros desarrolladores.

1. Haga clic en el **configuración** botón en la parte superior derecha > en el panel izquierdo, haga clic en **extensiones**. 
2. El **extensiones disponibles** ficha enumera las extensiones en la fuente que están disponibles para la instalación.
3. Haga clic en una extensión para ver la descripción de la extensión, versión, publisher y otra información en el **detalles** panel.
4. Haga clic en **instalar** instalar una extensión. Si la puerta de enlace debe ejecutar en modo con privilegios elevados para realizar este cambio, aparecerá una elevación de UAC. Una vez completada la instalación, el explorador se actualizarán automáticamente y se volverá a cargar en Windows Admin Center con la nueva extensión instalada. Si la extensión está intentando instalar es una actualización de una extensión instalada previamente, puede hacer clic en el **actualizar a la versión más reciente** botón para instalar la actualización. También puede ir a la **extensiones instaladas** tabulador para ir a ver instalado Extensiones y vea si hay alguna actualización disponible en el **estado** columna.

## <a name="installing-extensions-from-a-different-feed"></a>Instalación de extensiones de otra fuente

Windows Admin Center es compatible con varias fuentes de distribución y puede ver y administrar los paquetes de más de una fuente a la vez. Fuente de cualquier NuGet compatibles con las API de NuGet V2 o un recurso compartido de archivos se puede agregar a Windows Admin Center para instalar las extensiones de.

1. Haga clic en el **configuración** botón en la parte superior derecha > en el panel izquierdo, haga clic en **extensiones**.
2. En el panel derecho, haga clic en el **fuentes** ficha.
3. Haga clic en el **agregar** para agregar otra fuente. Para una fuente NuGet, escriba la versión de NuGet 2 dirección URL de fuente. Proveedor de fuentes de NuGet o administrador debe ser capaz de proporcionar la información de dirección URL. Un recurso compartido de archivos, escriba la ruta de acceso completa del archivo de recurso compartido en el que se almacenan los archivos de paquete de extensión (archivo .nupkg).
4. Haz clic en **Agregar**. Si la puerta de enlace debe ejecutar en modo con privilegios elevados para realizar este cambio, aparecerá una elevación de UAC.

El **extensiones disponibles** lista mostrará las extensiones de todas las fuentes registradas. Puede comprobar qué fuente cada extensión es de uso de la **la fuente del paquete** columna.

## <a name="uninstalling-an-extension"></a>Desinstalar una extensión

Puede desinstalar las extensiones que ha instalado previamente, o incluso desinstalar las herramientas que se instalaron previamente como parte de la instalación de Windows Admin Center.

1. Haga clic en el **configuración** botón en la parte superior derecha > en el panel izquierdo, haga clic en **extensiones**. 
2. Haga clic en el **extensiones instaladas** pestaña para ver todas las extensiones instaladas.
3. Elija una extensión al desinstalar, a continuación, haga clic en **desinstalar**.

Después de desinstalar esté completo, el explorador se actualizarán automáticamente y se volverá a cargar en Windows Admin Center eliminado la extensión. Si desinstala una herramienta que se instalaron previamente como parte de Windows Admin Center, la herramienta estará disponible para su reinstalación en el **extensiones disponibles** ficha.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Instalar extensiones en un equipo sin conectividad a internet

Si Windows Admin Center está instalado en un equipo que no está conectado a internet o está detrás de un proxy, no podrá tener acceso a e instalar las extensiones de la de Windows Admin Center fuente. Puede descargar los paquetes de extensión manualmente o con un script de PowerShell y configurar Windows Admin Center para recuperar los paquetes desde un recurso compartido de archivos o una unidad local.

### <a name="manually-downloading-extension-packages"></a>Descargar manualmente los paquetes de extensión

1. En otro equipo que tenga conectividad a internet, abra un explorador web y vaya a la dirección URL siguiente: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Es posible que deba crear una cuenta de msft sme.myget.org e inicie sesión para ver los paquetes de extensión.

2. Haga clic en el nombre del paquete que desea instalar para ver la página de detalles del paquete.
3. Haga clic en el **descargar** situado en el panel del lado derecho de la página de detalles del paquete y descargue el archivo .nupkg para la extensión.
4. Repita los pasos 2 y 3 para todos los paquetes que desea descargar.
5. Copie los archivos de paquete a un recurso compartido de archivos que se puede acceder desde el equipo en que Windows Admin Center está instalado, o en el disco local del equipo.
6. [Siga las instrucciones para instalar extensiones desde otra fuente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Descarga de paquetes con un script de PowerShell

Hay muchas secuencias de comandos disponibles en Internet para descargar paquetes de NuGet desde una fuente NuGet. Vamos a usar el [script proporcionado por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), director jefe de programas de Microsoft.

1. Como se describe en el [entrada de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), el script se instala como un paquete de NuGet, o copie y pegue el script en PowerShell ISE.
2. Dirección URL de v2 de la de la edición de que la fuente de la primera línea del script para su NuGet. Si va a descargar los paquetes desde el Windows Admin Center oficial de fuente, utilice la siguiente dirección URL.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Ejecute el script y descargará todos los paquetes de NuGet desde la fuente en la siguiente carpeta local: %USERPROFILE%\Documents\NuGetLocal
4. [Siga las instrucciones para instalar extensiones desde otra fuente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Administrar extensiones con PowerShell

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Versión preliminar de Windows Admin Center incluye un módulo de PowerShell para administrar las extensiones de la puerta de enlace.

>[!IMPORTANT]
>Administrar las extensiones de la puerta de enlace con el módulo de PowerShell solo se admite cuando se implementa Windows Admin Center como un servicio de puerta de enlace de Windows Server.

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Más información sobre la creación de una extensión con el SDK de Windows Admin Center](../extend/extensibility-overview.md).