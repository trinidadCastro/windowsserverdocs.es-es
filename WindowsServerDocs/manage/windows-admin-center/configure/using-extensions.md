---
title: Instalación y administración de extensiones
description: Instalar y administrar extensiones en el centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357243"
---
# <a name="install-and-manage-extensions"></a>Instalación y administración de extensiones

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

El centro de administración de Windows se crea como una plataforma extensible en la que cada tipo de conexión y herramienta es una extensión que se puede instalar, desinstalar y actualizar de forma individual. Puede buscar nuevas extensiones publicadas por Microsoft y otros desarrolladores, así como instalarlas y actualizarlas de forma individual sin tener que actualizar toda la instalación del centro de administración de Windows. También puede configurar una fuente de NuGet independiente o un recurso compartido de archivos y distribuir extensiones para usarlos internamente en su organización.

## <a name="installing-an-extension"></a>Instalación de una extensión

El centro de administración de Windows mostrará las extensiones disponibles en la fuente de NuGet especificada. De forma predeterminada, el centro de administración de Windows apunta a la fuente de NuGet oficial de Microsoft que hospeda extensiones publicadas por Microsoft y otros desarrolladores.

1. Haga clic en el botón **configuración** en la parte superior derecha > en el panel izquierdo y, a continuación, haga clic en **extensiones**. 
2. En la pestaña **extensiones disponibles** se enumeran las extensiones de la fuente que están disponibles para la instalación.
3. Haga clic en una extensión para ver la descripción de la extensión, la versión, el publicador y otra información en el panel de **detalles** .
4. Haga clic en **instalar** para instalar una extensión. Si la puerta de enlace debe ejecutarse en modo con privilegios elevados para realizar este cambio, se le presentará una petición de elevación de UAC. Una vez completada la instalación, el explorador se actualizará automáticamente y el centro de administración de Windows se volverá a cargar con la nueva extensión instalada. Si la extensión que está intentando instalar es una actualización de una extensión instalada previamente, puede hacer clic en el botón **actualizar a la versión más reciente** para instalar la actualización. También puede ir a la pestaña **extensiones instaladas** para ver las extensiones instaladas y ver si hay una actualización disponible en la columna **Estado** .

## <a name="installing-extensions-from-a-different-feed"></a>Instalación de extensiones desde una fuente diferente

El centro de administración de Windows admite varias fuentes y puede ver y administrar paquetes de más de una fuente a la vez. Cualquier fuente de NuGet que admita las API de NuGet V2 o un recurso compartido de archivos se puede Agregar al centro de administración de Windows para instalar las extensiones de.

1. Haga clic en el botón **configuración** en la parte superior derecha > en el panel izquierdo y, a continuación, haga clic en **extensiones**.
2. En el panel derecho, haga clic en la pestaña **fuentes** .
3. Haga clic en el botón **Agregar** para agregar otra fuente. Para una fuente de NuGet, escriba la dirección URL de la fuente de NuGet V2. El proveedor o el administrador de fuentes de NuGet deben ser capaces de proporcionar la información de la dirección URL. En un recurso compartido de archivos, escriba la ruta de acceso completa del recurso compartido de archivos en el que se almacenan los archivos de paquete de extensión (. nupkg).
4. Haz clic en **Agregar**. Si la puerta de enlace debe ejecutarse en modo con privilegios elevados para realizar este cambio, se le presentará una petición de elevación de UAC.

La lista **extensiones disponibles** mostrará las extensiones de todas las fuentes registradas. Puede comprobar en qué fuente se encuentra cada extensión mediante la columna **fuente de paquetes** .

## <a name="uninstalling-an-extension"></a>Desinstalación de una extensión

Puede desinstalar las extensiones que haya instalado anteriormente o, incluso, desinstalar las herramientas que se instalaron previamente como parte de la instalación del centro de administración de Windows.

1. Haga clic en el botón **configuración** en la parte superior derecha > en el panel izquierdo y, a continuación, haga clic en **extensiones**. 
2. Haga clic en la pestaña **extensiones instaladas** para ver todas las extensiones instaladas.
3. Elija una extensión para desinstalar y, a continuación, haga clic en **desinstalar**.

Una vez completada la desinstalación, el explorador se actualizará automáticamente y el centro de administración de Windows se volverá a cargar con la extensión quitada. Si ha desinstalado una herramienta que se instaló previamente como parte del centro de administración de Windows, la herramienta estará disponible para su reinstalación en la pestaña **extensiones disponibles** .

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Instalación de extensiones en un equipo sin conectividad a Internet

Si el centro de administración de Windows está instalado en un equipo que no está conectado a Internet o que está detrás de un proxy, es posible que no pueda obtener acceso e instalar las extensiones desde la fuente del centro de administración de Windows. Puede descargar paquetes de extensión manualmente o con un script de PowerShell, y configurar el centro de administración de Windows para recuperar paquetes desde un recurso compartido de archivos o una unidad local.

### <a name="manually-downloading-extension-packages"></a>Descarga manual de paquetes de extensión

1. En otro equipo que tenga conectividad a Internet, abra un explorador Web y vaya a la dirección URL siguiente: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Es posible que tenga que crear una cuenta en msft-sme.myget.org e iniciar sesión para ver los paquetes de extensión.

2. Haga clic en el nombre del paquete que desea instalar para ver la página de detalles del paquete.
3. Haga clic en el vínculo **Descargar** en el panel derecho de la página de detalles del paquete y descargue el archivo. nupkg para la extensión.
4. Repita los pasos 2 y 3 para todos los paquetes que desee descargar.
5. Copie los archivos de paquete en un recurso compartido de archivos al que se pueda tener acceso desde el equipo en el que está instalado el centro de administración de Windows o en el disco local del equipo.
6. [Siga las instrucciones para instalar extensiones desde una fuente diferente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Descarga de paquetes con un script de PowerShell

Hay muchos scripts disponibles en Internet para descargar paquetes NuGet desde una fuente de NuGet. Usaremos el [script proporcionado por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Director de programas en Microsoft.

1. Tal como se describe en la [entrada de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instale el script como un paquete de NuGet, o copie y pegue el script en PowerShell ISE.
2. Edite la primera línea del script en la dirección URL V2 de la fuente de NuGet. Si está descargando paquetes desde la fuente oficial del centro de administración de Windows, use la dirección URL siguiente.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Ejecute el script y se descargarán todos los paquetes NuGet de la fuente en la siguiente carpeta local:%USERPROFILE%\Documents\NuGetLocal
4. [Siga las instrucciones para instalar extensiones desde una fuente diferente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Administrar extensiones con PowerShell

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

La versión preliminar del centro de administración de Windows incluye un módulo de PowerShell para administrar las extensiones de puerta de enlace.

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Obtenga más información sobre la creación de una extensión con el SDK del centro de administración de Windows](../extend/extensibility-overview.md).