---
title: Hardware compatible con la protección basada en Windows Server Virtualization de integridad de código
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820376"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Hardware compatible con la protección basada en Windows Server Virtualization de integridad de código

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 introdujo un nuevo grupo de protección de código basada en virtualización para ayudar a proteger físicos y máquinas virtuales frente a ataques que modifican el código del sistema. Para lograr este nivel de protección alto, Microsoft trabaja junto con los fabricantes de hardware del equipo (fabricantes de equipos originales, o los OEM) para evitar escrituras malintencionados en el código de ejecución del sistema. Esta protección se puede aplicar a cualquier sistema y que se sirve como uno de los bloques de creación para implementar el estado del host de Hyper-V para máquinas virtuales blindadas (máquinas virtuales). 

Al igual que con cualquier protección basada en hardware, algunos sistemas no pueden ser compatibles debido a problemas como el marcado de páginas de memoria incorrecto como archivos ejecutables o intenta modificar el código en tiempo de ejecución, lo que puede provocar errores inesperados, incluida la pérdida de datos o un azul error de la pantalla (también denominado un error de detención). 

Para que sean compatibles y totalmente compatible con la nueva característica de seguridad, los OEM deben implementar la tabla de direcciones de memoria definidos en UEFI 2.6, que se publicó en enero de 2016. La adopción de la nueva UEFI estándar puede tardar; mientras tanto, para evitar que a los clientes encontrar problemas, queremos proporcionar información sobre los sistemas y las configuraciones que hemos probado este conjunto de características, así como los sistemas que sabemos que no sea compatible. 

## <a name="non-compatible-systems"></a>Sistemas no compatibles

Las siguientes configuraciones se sabe que no es compatible con la protección basada en virtualización de integridad de código y no se puede usar como un host de las máquinas virtuales blindadas:

- Los servidores Dell PowerEdge ejecutando PERC H330 RAID controladores para obtener más información, consulte el siguiente artículo de soporte de Dell [H330: activación "Compatibilidad de Hyper-V de guardián de Host" o "Device Guard" en el sistema operativo de 2016 de Win provoca errores de arranque del SO](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Sistemas compatibles

Estos son los sistemas de que nuestros socios hemos probado en nuestro entorno. Asegúrese de que compruebe que el sistema funciona según lo previsto en su entorno: 

- Virtual Machines: puede habilitar la protección basada en virtualización de integridad de código en las máquinas virtuales que se ejecutan en un host de Hyper-V a partir de Windows Server 2016.



