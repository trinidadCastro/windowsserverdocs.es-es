---
title: Requisitos del sistema para Windows Server Essentials
description: Obtenga información acerca de los requisitos del sistema para Windows Server Essentials.
ms.date: 10/31/2013
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 9a35006330452850dd10def998a7fdf2d8c8e853
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711700"
---
# <a name="system-requirements-for-windows-server-essentials"></a>Requisitos del sistema para Windows Server Essentials

>Se aplica a: Windows Server 2019 Essentials, Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

  El software de servidor de Windows Server Essentials es un sistema operativo solo de 64 bits. En la tabla 1 se definen los requisitos mínimos de hardware recomendados para Windows Server Essentials. En la tabla 2 se definen los requisitos adicionales de hardware y software para el servidor.


## <a name="table-1-system-requirements-for-windows-server-essentials"></a>Tabla 1. Requisitos del sistema para Windows Server Essentials

|Componente|Mínima|Recomendado*|Máxima|
|---------------|-------------|-------------------|-------------|
|Socket de la CPU|1,4 GHz (procesador de 64 bits) o superior en el caso de un núcleo único<br /><br /> 1,3 GHz (procesador de 64 bits) o superior en el caso de varios núcleos|3,1 GHz (procesador de 64 bits) o superior en el caso de varios núcleos|2 zócalos|
|Memoria (RAM)|2 GB<br /><br /> 4 GB si implementa Windows Server Essentials como máquina virtual|16 GB|64 GB|
|Unidades de disco duro y espacio de almacenamiento disponible|Disco duro de 160 GB con una partición del sistema de 60 GB||Sin límite|

 * Los requisitos de hardware recomendados admiten un límite máximo de usuarios y dispositivos.

## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>Tabla 2. Requisitos adicionales de hardware y software para Windows Server Essentials

|Componente|Descripción|
|---------------|-----------------|
|Adaptador de red|Adaptador Ethernet Gigabit (10/100/1000baseT PHY/MAC)|
|Internet|Para algunas funciones puede ser necesario tener acceso a Internet (es posible que se apliquen costes adicionales) o una cuenta de Microsoft.|
|Sistemas operativos de cliente compatibles| Windows 10, Windows 8.1, Windows 8, Windows 7, Macintosh OS X versiones 10,5 a 10,8.<br /><br /> **Nota:** Algunas características requieren ediciones Professional o superior.<br /><br /> 1 GB de espacio disponible en disco (se liberará una parte del disco después de la instalación)|
|Enrutador|Un enrutador o firewall compatible con IPv4 NAT o IPv6|
|Requisitos adicionales|Unidad de DVD-ROM|

 Los requisitos necesarios variarán según la configuración del sistema y las aplicaciones y características que instale. El rendimiento del procesador depende no solo de la frecuencia del reloj de este, sino también del número de núcleos y del tamaño de la memoria caché del procesador. Los requisitos de espacio en disco para la partición del sistema son aproximados. Es posible que sea necesario disponer de espacio adicional si se realiza la instalación a través de la red.

 Para obtener más información acerca de los requisitos de hardware, consulte el [Catálogo de Windows Server](https://www.windowsservercatalog.com/).

 Todo el hardware del servidor debe cumplir los requisitos establecidos para el programa del logotipo de Windows Server 2012 R2 para sistemas. Para obtener más información, consulte [Programa del logotipo de Windows](/previous-versions/windows/hardware/hck/dn641155(v=vs.85)).

> [!IMPORTANT]
> Los discos dinámicos no se admiten en Windows Server Essentials.

## <a name="additional-references"></a>Referencias adicionales

-   [Instalación de Windows Server 2012 Essentials](../install/Install-Windows-Server-Essentials.md)

-   [Requisitos del sistema para Windows Server Essentials](system-requirements.md)