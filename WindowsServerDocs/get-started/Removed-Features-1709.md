---
title: Características eliminadas o planificadas para su reemplazo a partir de Windows Server (versión 1709)
description: Características y funcionalidades se han quitado o planeado para su eliminación en las versiones.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/05/2018
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 37970f3bee2070cffc77bff855a8f28641196b24
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452860"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>Características eliminadas o planificadas para su reemplazo a partir de Windows Server, versión 1709

>Se aplica a: Windows Server, versión 1709

A continuación se enumeran las características y funcionalidades de Windows Server, versión 1709 que se quitaron del producto en dicha versión o se está empezando a considerar su posible reemplazo en versiones posteriores. Este artículo está orientado a profesionales de TI que actualizan sus sistemas operativos en un entorno comercial. **Esta lista está sujeta a cambios en versiones posteriores y puede no incluir todas las características afectadas o funcionalidad.** 

## <a name="features-removed-from-windows-server-version-1709"></a>Características quitadas de Windows Server, versión 1709
Windows Server, versión 1709 incluye las mismas características presentes en Windows Server 2016. Sin embargo, esta versión ofrece opciones diferentes de instalación que Windows Server 2016:

- Como lanzamiento del Canal semianual, Windows Server, versión 1709 ofrece solo la opción de instalación Server Core. Para obtener más información, consulte [comparación de canales de mantenimiento](../get-started-19/servicing-channels-19.md).
- A partir de esta versión, Nano Server no está disponible como sistema operativo host instalable. En cambio, Nano Server está disponible como un sistema operativo de contenedor. Consulta [Cambios de Nano Server de Windows Server, versión 1709](nano-in-semi-annual-channel.md).
- Bloque de mensajes del servidor (SMB) versión 1 a partir de esta versión, ya no se instala de forma predeterminada. Para obtener más información, consulte [SMBv1 no está instalado de forma predeterminada en Windows 10 Fall Creators Update y Windows Server, versión 1709 y versiones posteriores](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>Características consideradas para su reemplazo a partir de las versiones siguientes

Se está considerando reemplazar las siguientes características y funcionalidades a partir de las versiones posteriores a Windows Server, versión 1709. Con el tiempo, pueden quitarse por completo de la imagen de producto instalada y reemplazarse por otras características o funcionalidades (o instalarse desde otras fuentes), pero siguen estando disponibles en esta versión, a veces con determinadas funcionalidades quitadas. Debes comenzar a planificar el empleo de métodos alternativos o el reemplazo futuro para cualquier aplicación, código o uso que dependa de estas características.

Si tienes comentarios para compartir sobre el reemplazo propuesto de cualquiera de estas características, puedes usar la [aplicación Centro de opiniones](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Aunque esta aplicación se ejecuta en Windows 10, puedes usarla para enviarnos tus comentarios sobre el producto de Windows Server (y la documentación).

### <a name="iis-6-management-compatibility"></a>Compatibilidad con la administración de IIS 6
Las características específicas que deben considerarse para su reemplazo son:

- Compatibilidad con IIS 6 Metabase (Web-Metabase)
- Consola de administración de IIS 6 (Web-Lgcy-Mgmt-Console)
- Herramientas de scripting de IIS 6 (Web-Lgcy-Scripting)
- Compatibilidad con IIS 6 WMI (Web-WMI)

En lugar de la compatibilidad con IIS 6 Metabase (que actúa como una capa de emulación entre scripts de metabase basados en IIS 6 y la configuración basada en archivos que usa IIS 7 o versiones más recientes) debes empezar a migrar secuencias de comandos de administración a la configuración de destino basada en archivos IIS directamente mediante el uso de herramientas, tales como el espacio de nombres Microsoft.Web.Administration.

También debes iniciar la migración desde IIS 6.0 o versiones anteriores y pasar a la versión más reciente de IIS, que siempre está disponible en la versión más reciente de Windows Server.


### <a name="iis-digest-authentication"></a>Autenticación implícita IIS
Este método de autenticación está previsto para su reemplazo. En su lugar, debes empezar a usar otros métodos de autenticación, como por ejemplo, la asignación de certificados de cliente (consulta [Configurar asignaciones de certificados de cliente de uno a uno](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) o la autenticación de Windows (consulta [Configuración de la aplicación](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### <a name="internet-storage-name-service-isns"></a>Servicio de nombres de almacenamiento de Internet (iSNS)
Se está considerando el reemplazo de iSNS. La característica de bloque de mensajes de servidor (SMB) ofrece básicamente la misma funcionalidad con características adicionales. Consulta [Información general de Bloque de mensajes del servidor](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx) para obtener información detallada sobre esta característica.

### <a name="rsaaes-encryption-for-iis"></a>Cifrado de RSA/AES para IIS 
Este método de cifrado que se está considerando para el reemplazo porque la API de criptografía de nivel superior: Método de Next Generation (CNG) ya está disponible. Si quieres obtener más información sobre el cifrado de CNG, consulta [Acerca de CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
Esta versión anterior de Windows PowerShell se ha reemplazado por varias versiones más recientes. Para obtener las mejores características y un rendimiento excepcional, migra a Windows PowerShell 5.0 o versión posterior. Consulta la [Documentación de PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1) para obtener más información.

