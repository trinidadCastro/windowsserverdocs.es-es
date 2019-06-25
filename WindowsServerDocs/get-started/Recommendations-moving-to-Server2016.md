---
title: Recomendaciones para migrar a Windows Server 2016
description: Recomendaciones para migrar a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/18/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 08a92188446d018edd36a638abb30745721bd601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831586"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>Recomendaciones para migrar a Windows Server 2016

>Se aplica a: Windows Server 2016


|Si ejecuta:|Windows Server 2012 o Windows Server 2012 R2|Windows Server 2008 o Windows Server 2008 R2|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Infraestructura de rol de Windows Server**|Elija actualización o migración según las [instrucciones específicas del rol](https://technet.microsoft.com/windowsserver/jj554790).|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas nuevas características funcionan mejor en un host físico de Windows Server 2016 que ejecuten Hyper-V. <br>- Siga [instrucciones específicas del rol](https://technet.microsoft.com/windowsserver/jj554790).|
|**Microsoft server administración y aplicación de las cargas de trabajo**|- Las actualizaciones de aplicaciones deberían incluir la *migración* a Windows Server 2016. Consulte la [lista de compatibilidad](Server-Application-Compatibility.md). <br>- Las actualizaciones a Windows Server 2016 solo (es decir, sin actualizar aplicaciones) deben utilizar instrucciones específicas de la aplicación.|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas nuevas características funcionan mejor en un host físico de Windows Server 2016 que ejecuten Hyper-V. Siga a las guías de migración según corresponda. <br>- También puede mantener su SO actual y ejecutar en una máquina virtual en un host de Windows Server 2016 o Microsoft Azure. Póngase en contacto con su revendedor de EA, TAM o Microsoft para conocer las opciones de soporte técnico extendido a través de [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabajo de aplicación de ISV**|-Las actualizaciones a Windows Server 2016 deben utilizar instrucciones específicas de la aplicación. <br>- Para obtener más información sobre la compatibilidad de Windows Server con aplicaciones que no son de Microsoft, visite el [portal de certificación de logotipos de Windows Server](https://msdn.microsoft.com/enterprisecloudcertified).|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas nuevas características funcionan mejor en un host físico de Windows Server 2016 que ejecuten Hyper-V. Siga a las guías de migración según corresponda. <br>- También puede mantener su SO actual y ejecutar en una máquina virtual en un host de Windows Server 2016 o Microsoft Azure. Póngase en contacto con su revendedor de EA, TAM o Microsoft para conocer las opciones de soporte técnico extendido a través de [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabajo de aplicación personalizada**|- Consulte con los desarrolladores de aplicaciones sobre la compatibilidad con Windows Server 2016 e instrucciones de actualización. <br>- Aproveche Microsoft Azure para probar la aplicación en Windows Server 2016 antes de cambiar. <br>- Vea todas las opciones en la sección siguiente.|- Consulte con sus desarrolladores de aplicaciones sobre la compatibilidad con Windows Server 2016 e instrucciones de actualización. <br>- Aproveche Microsoft Azure para probar la aplicación en Windows Server 2016 antes de cambiar. <br>- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas nuevas características funcionan mejor en un host físico de Windows Server 2016 que ejecuten Hyper-V. <br>- Vea todas las opciones en la sección siguiente.|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>Opciones completas para migrar servidores que ejecutan aplicaciones personalizadas o "internas" en versiones anteriores de Windows Server a Windows Server 2016

Hay más opciones que nunca para ayudarle a usted y a sus clientes a aprovechar las características de Windows Server 2016 con una afectación mínima a sus servicios y cargas de trabajo actuales.

- Pruebe el sistema operativo más reciente con su aplicación mediante la descarga de la versión de evaluación de [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) para realizar pruebas en sus instalaciones. Una vez realizadas las pruebas y confirmada la calidad, puede realizar una conversión de licencia simple con una clave de licencia (requiere reiniciar).

- [Microsoft Azure](https://azure.microsoft.com) también se puede utilizar en versiones de prueba para garantizar que su aplicación personalizada funciona en el sistema operativo de servidor más reciente. Una vez realizadas las pruebas y confirmada la calidad, [migre a la versión más reciente de Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade#upgrade) en sus instalaciones. 

- También puede, una vez realizadas las pruebas y confirmada la calidad, usar [Microsoft Azure](https://azure.microsoft.com) como ubicación permanente para su servicio o aplicación personalizada. Esto permite que servidor antiguo permanezca disponibles hasta que esté listo para cambiar al nuevo servidor en Azure.

    - Si ya tiene Software Assurance para Windows Server, ahorrar dinero mediante la implementación con [Ventaja de uso híbrido de Microsoft Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- En la mayoría de los casos, [Microsoft Azure](https://azure.microsoft.com) puede usarse para hospedar la misma aplicación en la versión anterior de Windows Server que se está ejecutando actualmente. Migre la aplicación y la carga de trabajo a una máquina virtual con el sistema operativo de su elección mediante imágenes de [Azure Marketplace](https://azure.microsoft.com/marketplace/).

    - Si ya tiene Software Assurance para Windows Server, ahorrar dinero mediante la implementación con [Ventaja de uso híbrido de Microsoft Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- El programa [Software Assurance](https://www.microsoft.com/en-us/Licensing/licensing-programs/software-assurance-default.aspx) para Windows Server proporciona nuevas ventajas de derechos de nueva versión. Junto a otras ventajas, los servidores con Software Assurance pueden actualizarse a la última versión de Windows Server cuando llegue el momento sin tener que adquirir una licencia. 

## <a name="additional-resources"></a>Recursos adicionales

- [Funciones quitadas u obsoletas en Windows Server 2016](deprecated-features.md)
- Para opciones de actualización y migración de servidor generales, visite [Opciones de actualización y conversión para Windows Server 2016](Supported-Upgrade-Paths.md).
- Para obtener más información acerca del ciclo de vida del producto y los niveles de compatibilidad, consulte las [preguntas frecuentes de directiva del ciclo de vida de soporte general](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq).
