---
title: Hardware compatible con la protección basada en la virtualización de Windows Server de la integridad del código
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 616718b2b2a5ce3c30655d3fef37da5ee2ea25e5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971362"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Hardware compatible con la protección basada en la virtualización de Windows Server de la integridad del código

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 presentó una nueva protección de código basada en la virtualización para ayudar a proteger las máquinas virtuales y físicas de los ataques que modifican el código del sistema.
Para lograr este alto nivel de protección, Microsoft trabaja conjuntamente con los fabricantes de hardware de los equipos (fabricantes de equipos originales o OEM) para evitar escrituras malintencionadas en el código de ejecución del sistema.
Esta protección se puede aplicar a cualquier sistema y se utiliza como uno de los bloques de creación para implementar el estado del host de Hyper-V para máquinas virtuales blindadas (VM).

Como con cualquier protección basada en hardware, algunos sistemas podrían no ser compatibles debido a problemas como el marcado incorrecto de páginas de memoria como archivos ejecutables o al intentar modificar código en tiempo de ejecución, lo que puede producir errores inesperados, como la pérdida de datos o un error de pantalla azul (también denominado error de detención).

Para ser compatible y totalmente compatible con la nueva característica de seguridad, los OEM deben implementar la tabla de direcciones de memoria definida en UEFI 2,6, que se publicó en enero de 2016.
La adopción del nuevo estándar UEFI lleva tiempo; mientras tanto, para evitar que los clientes encuentren problemas, queremos proporcionar información sobre los sistemas y las configuraciones que hemos probado en este conjunto de características con, así como los sistemas que sabemos que no son compatibles.

## <a name="non-compatible-systems"></a>Sistemas no compatibles

Se sabe que las siguientes configuraciones no son compatibles con la protección basada en la virtualización de la integridad del código y no se pueden usar como host para las máquinas virtuales blindadas:

- Servidores Dell PowerEdge que ejecutan controladores RAID de PERC H330 para obtener más información, consulte el siguiente artículo del soporte técnico de Dell [H330, la habilitación de la compatibilidad con Hyper-V de protección de host o la protección de dispositivos en Win 2016 os causa un error de arranque del sistema operativo](http://www.dell.com/Support/Article/us/en/19/QNA44045).


## <a name="compatible-systems"></a>Sistemas compatibles

Estos son los sistemas que nosotros y nuestros asociados han estado probando en nuestro entorno.
Asegúrese de comprobar que el sistema funciona según lo previsto en el entorno:

- Virtual Machines: puede habilitar la protección basada en la virtualización de la integridad de código en máquinas virtuales que se ejecutan en un host de Hyper-V a partir de Windows Server 2016.



