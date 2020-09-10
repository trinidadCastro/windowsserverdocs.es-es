---
title: Requisitos de hardware para el equipo de destino
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c20b06b9-ce0d-4c20-bf49-257c3f5dc01b
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 1672c8168dd206c166ca44392ca290ebb311dcc6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623510"
---
# <a name="hardware-requirements-for-the-target-computer"></a>Requisitos de hardware para el equipo de destino

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En esta sección se proporcionan los requisitos de hardware para Windows Server Essentials.

## <a name="hardware-requirements-for-windows-server-essentials"></a>Requisitos de hardware para Windows Server Essentials
 Debe instalar Windows Server Essentials en el equipo de destino que cumpla los requisitos de la tabla siguiente.

### <a name="table-1--system-requirements-for-windows-server-essentials"></a>Tabla 1: requisitos del sistema para Windows Server Essentials

|Componente|Mínima|Recomendado*|Máxima|
|---------------|-------------|-------------------|-------------|
|Socket de la CPU|1,4 GHz (procesador de 64 bits) o superior en el caso de un núcleo único<br /><br /> 1,3 GHz (procesador de 64 bits) o superior en el caso de varios núcleos|3,1 GHz (procesador de 64 bits) o superior en el caso de varios núcleos|2 zócalos|
|Memoria (RAM)|2 GB|16 GB|64 GB|
|Unidades de disco duro y espacio de almacenamiento disponible|Unidad de disco duro de 160 GB con una partición de sistema de 60 GB||Sin límite|

 * Requisitos de hardware recomendados para admitir el límite máximo de usuarios y dispositivos de Windows Server Essentials.

## <a name="additional-hardware-and-software-requirements"></a>Requisitos adicionales de hardware y software
 En la tabla siguiente se incluyen los requisitos adicionales de hardware y software.

|Componente|Descripción|
|---------------|-----------------|
|Adaptador de red|Adaptador Ethernet Gigabit (10/100/1000baseT PHY/MAC)|
|Internet|Algunas funciones pueden requerir acceso a Internet (pueden aplicarse tarifas) o una cuenta de Windows Live &reg; ID.|
|Sistemas operativos de cliente compatibles|-Windows 7<br />-Windows 8<br />-Macintosh OS X 10,5 a 10,8.<br /><br /> **Nota:** Algunas características requieren ediciones Professional o superior.<br /><br /> 1 GB de espacio disponible en disco (se liberará una parte del disco después de la instalación)|
|Enrutador|Un enrutador o firewall compatible con IPv4 NAT|
|Requisitos adicionales|Unidad de DVD-ROM|

 Los requisitos necesarios variarán según la configuración del sistema y las aplicaciones y características que instale. El rendimiento del procesador depende no solo de la frecuencia del reloj de este, sino también del número de núcleos y del tamaño de la memoria caché del procesador. Los requisitos de espacio en disco para la partición del sistema son aproximados. Es posible que sea necesario disponer de espacio adicional si se realiza la instalación a través de la red.

 Para obtener más información acerca de los requisitos de hardware, consulte el [Catálogo de Windows Server](https://www.windowsservercatalog.com).

 Todo el hardware del servidor debe cumplir los requisitos establecidos para el programa del logotipo de Windows Server 2012 para sistemas. Para obtener más información, consulte [Programa del logotipo de Windows](https://www.microsoft.com/whdc/winlogo/hwrequirements.mspx).

## <a name="see-also"></a>Consulte también

 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)

