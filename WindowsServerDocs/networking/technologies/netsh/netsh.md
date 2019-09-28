---
title: Shell de red (Netsh)
description: En este tema se proporciona información general de la utilidad de línea de comandos de Shell de red (netsh) en Windows Server 2016.
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
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405545"
---
# <a name="network-shell-netsh"></a>Shell de red \(Netsh @ no__t-1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Shell de red (netsh) es una utilidad de línea de comandos que permite configurar y mostrar el estado de varios componentes y roles de servidor de comunicaciones de red una vez instalados en los equipos que ejecutan Windows Server 2016.

Algunas tecnologías de cliente, como el cliente de protocolo de configuración dinámica de host \(DHCP @ no__t-1 y BranchCache, también proporcionan comandos Netsh que le permiten configurar equipos cliente que ejecutan Windows 10.

En la mayoría de los casos, los comandos Netsh proporcionan la misma funcionalidad que está disponible cuando se usa Microsoft Management Console \(MMC @ no__t-1 Snap @ no__t-2in para cada rol de servidor de red o característica de red. Por ejemplo, puede configurar el servidor de directivas de redes \(NPS @ no__t-1 mediante el complemento MMC de NPS o los comandos Netsh en el contexto **netsh NPS** .

Además, hay comandos Netsh para tecnologías de red, como IPv6, puente de red y llamada a procedimiento remoto \(RPC @ no__t-1, que no están disponibles en Windows Server como complemento MMC.

>[!IMPORTANT]
>Se recomienda usar Windows PowerShell para administrar las tecnologías de red en [Windows Server 2016 y Windows 10](https://technet.microsoft.com/library/mt156917.aspx) en lugar de en el shell de red. No obstante, se incluye el shell de red para la compatibilidad con los scripts y se admite su uso.

## <a name="network-shell-netsh-technical-reference"></a>Referencia técnica de Shell de red (netsh)

La referencia técnica de Netsh proporciona una referencia de comando netsh completa, incluida la sintaxis, los parámetros y los ejemplos de comandos Netsh. Puede usar la referencia técnica de Netsh para compilar scripts y archivos por lotes mediante comandos Netsh para la administración local o remota de tecnologías de red en equipos que ejecutan Windows Server 2016 y Windows 10.  
  
### <a name="content-availability"></a>Disponibilidad del contenido  
  
La referencia técnica del shell de red está disponible para su descarga en el formato de la ayuda de Windows \( *. chm @ no__t-1 de la galería de TechNet: [Referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
