---
ms.assetid: ''
title: Versión preliminar de Insider características HPN en Windows Server 2019
description: Obtenga información sobre las nuevas características de redes de alto rendimiento en Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886276"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>VRSS dinámico y VMMQ

>Se aplica a: Windows Server 2019

En el pasado, las colas de máquina Virtual y las colas de varias máquinas virtuales habilitado mucho un mayor rendimiento a las máquinas virtuales individuales como rendimientos red alcanzados primero la marca de 10 GbE y mucho más. Desafortunadamente, planeación, la línea de base, requiere de éxito se convirtió en una empresa grande; optimización y supervisión a menudo más que el Administrador de TI diseñado para utilizarlos. 

Windows Server 2019 mejora estas optimizaciones repartir de forma dinámica y optimización del procesamiento de cargas de trabajo de red según sea necesario. Windows Server 2019 garantiza su máxima eficacia y elimina la carga de configuración para los administradores de TI.

Para obtener más información, vea:

-   [Blog del anuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guía de validación para la TI Pro](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Fusión de segmentos de recepción (RSC) en el vSwitch

>Se aplica a: 2019 de Windows Server y Windows 10, versión 1809

Recibir segmento Coalescing (RSC) en el vSwitch es una mejora que combina varios segmentos TCP en un segmento mayor antes de atravesar el vSwitch de datos. El segmento de gran tamaño mejora el rendimiento de red para cargas de trabajo virtuales.

Anteriormente, esto era una descarga implementada por la NIC. Lamentablemente, esto se ha deshabilitado el momento en que ha adjuntado el adaptador a un conmutador virtual. RSC en el vSwitch en 2019 de Windows Server y Windows 10 de octubre de 2018 actualización elimina esta limitación.

De forma predeterminada, RSC en el vSwitch está habilitado en conmutadores virtuales externos.

Para obtener más información, vea:

-  [Blog del anuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guía de validación para la TI Pro](https://aka.ms/RSC-Validation)
