---
title: Shell de red (Netsh)
description: Este tema proporciona una descripción general de la herramienta de línea de comandos de Shell de red (netsh) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>\(Netsh\) Shell de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

El shell de red (netsh) es una utilidad de línea de comandos que te permite configurar y mostrar el estado de los diversos roles de servidor de comunicaciones de red y componentes después de que están instalados en equipos que ejecutan Windows Server 2016.

>[!NOTE]
>Además de este tema, el siguiente contenido de Shell de red está disponible.
>
> - [Sintaxis de comandos Netsh, los contextos y formato](netsh-contexts.md)
> - [Archivo por lotes de red (Netsh) Shell ejemplo](netsh-wins.md)
> - [Referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

Algunas tecnologías de cliente, como cliente del protocolo de configuración dinámica de Host \(DHCP\) y BranchCache, también proporcionan comandos netsh que permiten configurar los equipos cliente que ejecutan Windows 10.

En la mayoría de los casos, los comandos netsh proporcionan la misma funcionalidad que está disponible cuando usas la \(MMC\) snap\ Microsoft Management Console-en para cada característica de rol o redes del servidor de red. Por ejemplo, puedes configurar el servidor de directivas de red \(NPS\) mediante el complemento de MMC NPS o los comandos netsh en la **netsh nps** contexto.

Además, hay comandos netsh para tecnologías de red, como para IPv6, puente de red y \(RPC\) llamada a procedimiento remoto, no están disponibles en Windows Server como un complemento de MMC.

>[!IMPORTANT]
>Se recomienda que uses Windows PowerShell para administrar las tecnologías de redes en [Windows Server 2016 y Windows 10](https://technet.microsoft.com/library/mt156917.aspx) en lugar de Shell de red. Sin embargo, el Shell de red se incluye para la compatibilidad con los scripts, y su uso es compatible.

## <a name="network-shell-netsh-technical-reference"></a>Referencia técnica de red (Netsh) Shell

La referencia técnica de Netsh proporciona una referencia de comandos netsh completa, incluida la sintaxis, parámetros y ejemplos de comandos netsh. Puedes usar la referencia técnica de Netsh generar scripts y archivos por lotes mediante los comandos netsh para la administración local o remota de tecnologías de red en equipos que ejecutan Windows Server 2016 y Windows 10.  
  
### <a name="content-availability"></a>Disponibilidad de contenido  
  
La referencia técnica de Shell de red está disponible para descarga en formato de \(*.chm\) la Ayuda de Windows desde la Galería de TechNet: [referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  

