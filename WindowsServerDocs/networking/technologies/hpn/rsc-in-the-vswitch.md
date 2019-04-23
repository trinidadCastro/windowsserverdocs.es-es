---
title: Fusión de segmentos de recepción (RSC) en el vSwitch
description: Recibir segmento Coalescing (RSC) en el vSwitch es una característica de 2019 de Windows Server y Windows 10 de octubre de 2018 Update que ayuda a reduce la utilización de CPU del host y aumenta el rendimiento para cargas de trabajo virtuales mediante el uso combinado de varios segmentos TCP en menos, pero más grandes segmentos. Procesar menos segmentos grandes (fusionadas) es más eficaz que el procesamiento de numerosas y segmentos pequeños.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827786"
---
# <a name="rsc-in-the-vswitch"></a>RSC en el vSwitch
>Se aplica a: Windows Server 2019

Recibir segmento Coalescing (RSC) en el vSwitch es una característica de 2019 de Windows Server y Windows 10 de octubre de 2018 Update que ayuda a reduce la utilización de CPU del host y aumenta el rendimiento para cargas de trabajo virtuales mediante el uso combinado de varios segmentos TCP en menos, pero más grandes segmentos. Procesar menos segmentos grandes (fusionadas) es más eficaz que el procesamiento de numerosas y segmentos pequeños.

Windows Server 2012 y versiones posterior incluye una versión de hardware de solo descarga (implementada en el adaptador de red física) de la tecnología que también se conoce como la fusión de segmentos de recepción. Esta versión descarga de RSC sigue estando disponible en versiones posteriores de Windows. Sin embargo, no es compatible con cargas de trabajo virtuales y se ha deshabilitado una vez que un adaptador de red físico está conectado a un vSwitch. Para obtener más información sobre la versión de RSC sólo hardware, consulte [Receive segmento Coalescing (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Escenarios que se benefician de RSC en el vSwitch

Esta característica se beneficia de las cargas de trabajo cuya ruta de datos atraviesa un conmutador virtual.

Por ejemplo:

-   Host NIC virtuales incluidos:

    -   Redes definidas por software

    -   Host de Hyper-V

    -   Espacios de almacenamiento directo

-   NIC Virtual de invitado de Hyper-V

-   Las puertas de enlace GRE de redes definidas por software

-   Contenedor

Cargas de trabajo que no son compatibles con esta característica se incluyen:

-   Las puertas de enlace IPSEC de redes definidas por software

-   SR-IOV habilitada NIC virtuales

-   SMB directo

## <a name="configure-rsc-in-the-vswitch"></a>Configurar RSC en el vSwitch


De forma predeterminada, en vSwitches externo, RSC está habilitado.

**Ver la configuración actual:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Habilitar o deshabilitar RSC en el vSwitch**


>[!IMPORTANT]
>Importante: RSC en el vSwitch se puede habilitar y deshabilitar sobre la marcha sin afectar a las conexiones existentes.


**Deshabilitar RSC en el vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Volver a habilitar RSC en el vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Para obtener más información, consulte [Set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
