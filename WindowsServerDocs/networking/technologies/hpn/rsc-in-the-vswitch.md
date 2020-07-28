---
title: Fusión de segmentos de recepción (RSC) en el vSwitch
description: La fusión de segmentos de recepción (RSC) en el vSwitch es una característica de Windows Server 2019 y la actualización de Windows 10 de octubre de 2018 que ayuda a reducir el uso de la CPU del host y aumenta el rendimiento de las cargas de trabajo virtuales mediante la combinación de varios segmentos TCP en menos segmentos, pero más grandes. El procesamiento de menos segmentos de gran tamaño (fusionados) es más eficaz que el procesamiento de numerosos segmentos pequeños.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: e7db6656bd9331e5cc6c2deaee143b3602ef1239
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181821"
---
# <a name="rsc-in-the-vswitch"></a>RSC en vSwitch
>Se aplica a: Windows Server 2019

La fusión de segmentos de recepción (RSC) en el vSwitch es una característica de Windows Server 2019 y la actualización de Windows 10 de octubre de 2018 que ayuda a reducir el uso de la CPU del host y aumenta el rendimiento de las cargas de trabajo virtuales mediante la combinación de varios segmentos TCP en menos segmentos, pero más grandes. El procesamiento de menos segmentos de gran tamaño (fusionados) es más eficaz que el procesamiento de numerosos segmentos pequeños.

Windows Server 2012 y versiones posteriores incluían una versión de descarga de hardware (implementada en el adaptador de red físico) de la tecnología, también conocida como fusión de segmentos de recepción. Esta versión descargada de RSC todavía está disponible en versiones posteriores de Windows. Sin embargo, no es compatible con las cargas de trabajo virtuales y se ha deshabilitado una vez que se ha conectado un adaptador de red físico a un vSwitch. Para obtener más información sobre la versión solo de hardware de RSC, consulte [Receive Segment COALESCE (RSC)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Escenarios que se benefician de RSC en el vSwitch

Las cargas de trabajo cuya ruta de acceso atraviesa un conmutador virtual se benefician de esta característica.

Por ejemplo:

-   Hospedar NIC virtuales, incluidos:

    -   Red definida por software

    -   Host de Hyper-V

    -   Espacios de almacenamiento directo

-   NIC virtuales de invitado de Hyper-V

-   Puertas de enlace GRE de redes definidas por software

-   Contenedor

Las cargas de trabajo que no son compatibles con esta característica incluyen:

-   Puertas de enlace IPSEC de redes definidas por software

-   NIC virtuales habilitadas para SR-IOV

-   SMB directo

## <a name="configure-rsc-in-the-vswitch"></a>Configuración de RSC en el vSwitch


De forma predeterminada, en vSwitches externos, RSC está habilitado.

**Vea la configuración actual:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Habilitar o deshabilitar RSC en el vSwitch**


>[!IMPORTANT]
>Importante: RSC en el vSwitch se puede habilitar y deshabilitar sobre la marcha sin que ello afecte a las conexiones existentes.


**Deshabilitar RSC en el vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Volver a habilitar RSC en el vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Para obtener más información, consulte [set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
