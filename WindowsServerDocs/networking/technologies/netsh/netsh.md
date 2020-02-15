---
title: Shell de red (Netsh)
description: En este tema se proporciona información general de la utilidad de línea de comandos de shell de red (netsh) en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405545"
---
# <a name="network-shell-netsh"></a>Shell de red \(Netsh\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Shell de red (netsh) es una utilidad de línea de comandos que permite configurar y mostrar el estado de varios componentes y roles de servidor de comunicaciones de red tras instalarlos en equipos que ejecutan Windows Server 2016.

Algunas tecnologías de cliente, como el cliente del protocolo de configuración dinámica de host \(DHCP\)y BranchCache, también proporcionan comandos netsh que te permiten configurar equipos cliente que ejecutan Windows 10.

En la mayoría de los casos, los comandos netsh proporcionan la misma funcionalidad que está disponible cuando se usa el complemento Microsoft Management Console \(MMC\) para cada rol de servidor de red o característica de red. Por ejemplo, puedes configurar el Servidor de directivas de redes \(NPS\) mediante el complemento MMC de NPS o los comandos netsh en el contexto de **netsh nps**.

Además, hay comandos netsh para tecnologías de red, como IPv6, puente de red y llamada a procedimiento remoto \(RPC\), que no están disponibles en Windows Server como complemento MMC.

>[!IMPORTANT]
>Se recomienda usar Windows PowerShell para administrar las tecnologías de red en [Windows Server 2016 y Windows 10](https://technet.microsoft.com/library/mt156917.aspx), en lugar de shell de red. No obstante, el shell de red se incluye para la compatibilidad con los scripts y se admite su uso.

## <a name="network-shell-netsh-technical-reference"></a>Referencia técnica de shell de red (netsh)

La referencia técnica de netsh proporciona una referencia completa de comandos netsh, incluida la sintaxis, los parámetros y los ejemplos de comandos netsh. Puedes usar la referencia técnica de netsh para compilar scripts y archivos por lotes mediante comandos netsh para la administración local o remota de tecnologías de red en equipos que ejecutan Windows Server 2016 y Windows 10.  
  
### <a name="content-availability"></a>Disponibilidad del contenido  
  
La referencia técnica del shell de red está disponible para su descarga en el formato \(*.chm\) de la ayuda de Windows desde la Galería de TechNet: [Referencia técnica de netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
