---
title: Shell de red (Netsh)
description: En este tema se proporciona información general de la utilidad de línea de comandos de Shell de red (netsh) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849366"
---
# <a name="network-shell-netsh"></a>Shell de red \(Netsh\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Shell de red (netsh) es una utilidad de línea de comandos que le permite configurar y mostrar el estado de varios roles de servidor de comunicaciones de red y componentes después de instalarlos en equipos que ejecutan Windows Server 2016.

Algunas tecnologías de cliente, como el protocolo de configuración dinámica de Host \(DHCP\) cliente y BranchCache, también proporcionan comandos netsh que le permiten configurar los equipos cliente que ejecutan Windows 10.

En la mayoría de los casos, los comandos de netsh proporcionan la misma funcionalidad que está disponible cuando se usa la consola de administración de Microsoft \(MMC\) ajustar\-en para cada característica de rol o la conexión de red del servidor de red. Por ejemplo, puede configurar el servidor de directivas de red \(NPS\) mediante el complemento MMC NPS o los comandos de netsh en el **netsh nps** contexto.

Además, hay comandos netsh para tecnologías de red, como para IPv6, el puente de red y la llamada a procedimiento remoto \(RPC\), que no están disponibles en Windows Server como un complemento de MMC.

>[!IMPORTANT]
>Se recomienda que use Windows PowerShell para administrar tecnologías de red en [Windows Server 2016 y Windows 10](https://technet.microsoft.com/library/mt156917.aspx) en lugar del Shell de red. Sin embargo, se incluye por compatibilidad con las secuencias de comandos Shell de red y su uso se admite.

## <a name="network-shell-netsh-technical-reference"></a>Referencia técnica del Shell (Netsh) de red

La referencia técnica de Netsh proporciona una referencia de comandos de netsh completa, incluida la sintaxis, parámetros y ejemplos de comandos de netsh. Puede usar la referencia técnica de Netsh para generar scripts y archivos por lotes mediante los comandos netsh para la administración local o remota de las tecnologías de red en equipos que ejecutan Windows Server 2016 y Windows 10.  
  
### <a name="content-availability"></a>Disponibilidad del contenido  
  
La referencia técnica del Shell de red está disponible para su descarga en la Ayuda de Windows \(*.chm\) formato desde la Galería de TechNet: [Referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
