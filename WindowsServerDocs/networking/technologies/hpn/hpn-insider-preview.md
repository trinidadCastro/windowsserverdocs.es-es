---
ms.assetid: ''
title: Versión preliminar de Insider para características de HPN en Windows Server 2019
description: Obtenga información acerca de las nuevas características de red de alto rendimiento en Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 7098e81f486a5b0b4974c19b47e2d48c6f98832b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355367"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>VRSS y VMMQ dinámicos

>Se aplica a: Windows Server 2019

En el pasado, las colas de máquinas virtuales y las múltiples colas de máquinas virtuales permitían un rendimiento mucho mayor en máquinas virtuales individuales a medida que los rendimientos de red alcanzaban por primera vez la marca 10GbE y más allá. Desafortunadamente, el planeamiento, la línea de la línea, el ajuste y la supervisión necesarios para el éxito se convirtió en una gran empresa; a menudo, más que el administrador de ti ha previsto gastar. 

Windows Server 2019 mejora estas optimizaciones al extender y ajustar dinámicamente el procesamiento de cargas de trabajo de red según sea necesario. Windows Server 2019 garantiza la eficacia máxima y elimina la carga de configuración para los administradores de ti.

Para obtener más información, vea:

-   [Blog de anuncios](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guía de validación para profesionales de ti](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Fusión de segmentos de recepción (RSC) en el vSwitch

>Se aplica a: Windows Server 2019 y Windows 10, versión 1809

La fusión de segmentos de recepción (RSC) en el vSwitch es una mejora que combina varios segmentos TCP en un segmento más grande antes de que los datos atraviesen el vSwitch. El segmento grande mejora el rendimiento de la red para cargas de trabajo virtuales.

Anteriormente, esta era una descarga implementada por la NIC. Desgraciadamente, se deshabilitó el momento en que adjuntó el adaptador a un conmutador virtual. RSC en el vSwitch en Windows Server 2019 y Windows 10 de octubre de 2018 elimina esta limitación.

De forma predeterminada, RSC está habilitado en los conmutadores virtuales externos.

Para obtener más información, vea:

-  [Blog de anuncios](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guía de validación para profesionales de ti](https://aka.ms/RSC-Validation)
