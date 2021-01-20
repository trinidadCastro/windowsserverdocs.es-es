---
title: Instalar y administrar extensiones
description: Instalar y administrar extensiones en Windows Admin Center (proyecto Honolulu)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.date: 01/14/2021
ms.openlocfilehash: 20e2c1098aaf77559c32cca8f457a3ec5e3080f8
ms.sourcegitcommit: 17571fb06af2634dbf0163a2c08014cd6e9f9762
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98206064"
---
# <a name="install-and-manage-extensions"></a>Instalar y administrar extensiones

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Windows Admin Center se ha creado como una plataforma extensible en que cada herramienta y tipo de conexión es una extensión que se puede instalar, desinstalar y actualizar de forma individual. Puedes buscar nuevas extensiones publicadas por Microsoft y otros desarrolladores, así como instalarlas y actualizarlas de forma individual sin tener que actualizar toda la instalación de Windows Admin Center. También puedes configurar un recurso compartido de archivos o fuente de NuGet independiente y distribuir extensiones para usarlas internamente en tu organización.

## <a name="installing-an-extension"></a>Instalación de una extensión

Windows Admin Center mostrará las extensiones disponibles de la fuente de NuGet especificada. De forma predeterminada, Windows Admin Center apunta a la fuente de NuGet oficial de Microsoft, que incluye las extensiones publicadas por Microsoft y otros desarrolladores.

1. Haz clic en el botón **Configuración** de la parte superior derecha. A continuación, en el panel izquierdo, haz clic en **Extensiones**.
2. En la pestaña **Extensiones disponibles** se enumeran las extensiones de la fuente disponibles para la instalación.
3. Haz clic en una extensión para ver la descripción de la extensión, la versión, el editor y otra información en el panel **Detalles**.
4. Haz clic en **Instalar** para instalar una extensión. Si la puerta de enlace debe ejecutarse en modo con privilegios elevados para realizar este cambio, se te presentará una petición de elevación de UAC. Una vez completada la instalación, el explorador se actualizará automáticamente y Windows Admin Center se volverá a cargar con la nueva extensión instalada. Si la extensión que estás intentando instalar es una actualización de una extensión instalada previamente, puedes hacer clic en el botón **Update to latest** (Actualizar a la versión más reciente) para instalar la actualización. También puedes ir a la pestaña **Extensiones instaladas** para ver las extensiones instaladas y ver si hay alguna actualización disponible en la columna **Estado**.

## <a name="installing-extensions-from-a-different-feed"></a>Instalación de extensiones desde una fuente diferente

Windows Admin Center admite varias fuentes y puede ver y administrar paquetes de más de una fuente a la vez. Cualquier fuente de NuGet que admita las API de NuGet V2 o un recurso compartido de archivos se puede agregar a Windows Admin Center como origen para la instalación de extensiones.

1. Haz clic en el botón **Configuración** de la parte superior derecha. A continuación, en el panel izquierdo, haz clic en **Extensiones**.
2. En el panel de la derecha, haz clic en la pestaña **Fuentes**.
3. Haz clic en el botón **Agregar** para agregar otra fuente. Para una fuente de NuGet, escribe la dirección URL de la fuente de NuGet V2. El proveedor o el administrador de la fuente de NuGet debe poder proporcionar la información de la dirección URL. En un recurso compartido de archivos, escribe la ruta de acceso completa del recurso compartido de archivos en el que se almacenan los archivos de paquete de extensión (.nupkg).
4. Haga clic en **Agregar**. Si la puerta de enlace debe ejecutarse en un modo con privilegios elevados para realizar este cambio, se te presentará una petición de elevación de UAC. Este mensaje solo se mostrará si está ejecutando Windows Admin Center en modo de escritorio.

La lista **Extensiones disponibles** mostrará las extensiones de todas las fuentes registradas. Puede comprobar de qué fuente es cada extensión en la columna **Fuente del paquete**.

## <a name="uninstalling-an-extension"></a>Desinstalar una extensión

Puedes desinstalar cualquier extensión que hayas instalado anteriormente o, incluso, desinstalar las herramientas que se instalaron previamente como parte de la instalación de Windows Admin Center.

1. Haz clic en el botón **Configuración** de la parte superior derecha. A continuación, en el panel izquierdo, haz clic en **Extensiones**.
2. Haz clic en la pestaña **Extensiones instaladas** para ver todas las extensiones instaladas.
3. Elige una extensión para desinstalarla y, a continuación, haz clic en **Desinstalar**.

Una vez completada la desinstalación, el explorador se actualizará automáticamente y Windows Admin Center se volverá a cargar con la nueva extensión eliminada. Si has desinstalado una herramienta que se instaló previamente como parte de Windows Admin Center, la herramienta estará disponible para su reinstalación en la pestaña **Extensiones disponibles**.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Instalación de extensiones en un equipo sin conectividad a Internet

Si Windows Admin Center está instalado en un equipo que no está conectado a Internet o que está detrás de un proxy, es posible que no pueda obtener acceso a las extensiones e instalarlas desde la fuente de Windows Admin Center. Puedes descargar paquetes de extensión manualmente o con un script de PowerShell y configurar Windows Admin Center para que recupere paquetes de un recurso compartido de archivos o una unidad local.

### <a name="manually-downloading-extension-packages"></a>Descarga manual de paquetes de extensión

1. En otro equipo que tenga conectividad a Internet, abre un explorador web y navega a la dirección URL siguiente: [https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC](https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC)

   * Es posible que tenga que crear una cuenta de Microsoft e iniciar sesión para ver los paquetes de extensión.

2. Haz clic en el nombre del paquete que quieras instalar para ver la página de detalles del paquete.
3. Haga clic en el vínculo **Descargar** en el panel de la derecha de la página de detalles del paquete y descargue el archivo .nupkg para la extensión.
4. Repite los pasos 2 y 3 para todos los paquetes que quieras descargar.
5. Copie los archivos de paquete en un recurso compartido de archivos al que se pueda acceder desde el equipo en el que está instalado Windows Admin Center o en el disco local de dicho equipo.
6. [Sigue las instrucciones para instalar extensiones desde otra fuente](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Descarga de paquetes con un script de PowerShell

Hay muchos scripts disponibles en Internet para descargar paquetes de NuGet desde una fuente de NuGet. Usaremos el [script proporcionado por Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), administrador de programas sénior en Microsoft.

1. Como se describe en la [entrada de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), instala el script como un paquete de NuGet, o copia y pega el script en PowerShell ISE.
2. Edita la primera línea del script en la dirección URL de la fuente de NuGet V2. Si estás descargando paquetes desde la fuente oficial de Windows Admin Center, usa la dirección URL siguiente.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Ejecuta el script y se descargarán todos los paquetes de NuGet de la fuente en la siguiente carpeta local: %USERPROFILE%\Documents\NuGetLocal.
4. [Sigue las instrucciones para instalar extensiones desde otra fuente](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Administrar extensiones con PowerShell

La versión preliminar de Windows Admin Center incluye un módulo de PowerShell para administrar las extensiones de puerta de enlace.

[!INCLUDE [ps-extensions](../includes/ps-extensions.md)]

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdk"></a>[Más información sobre la creación de una extensión con el SDK de Windows Admin Center](../extend/extensibility-overview.md).
