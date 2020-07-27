---
title: Recomendaciones para cambiar a Windows Server 2016
description: Recomendaciones para cambiar a Windows Server 2016
ms.prod: windows-server
ms.date: 10/18/2016
ms.technology: server-general
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 478c967e9ce769f184d72d3534ed99c84924daed
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963947"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>Recomendaciones para cambiar a Windows Server 2016

>Se aplica a: Windows Server 2016


|Si ejecuta:|Windows Server 2012 o Windows Server 2012 R2|Windows Server 2008 o Windows Server 2008 R2|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Infraestructura de roles de Windows Server**|Elija actualización o migración según las [instrucciones específicas del rol](./migrate-roles-and-features.md).|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas características nuevas funcionan mejor en un host físico de Windows Server 2016 que ejecute Hyper-V. <br>- Siga las [instrucciones específicas del rol](./migrate-roles-and-features.md).|
|**Administración de servidores de Microsoft y cargas de trabajo de aplicaciones**|- Las actualizaciones de aplicaciones deberían incluir la *migración* a Windows Server 2016. Consulte la [lista de compatibilidad](Server-Application-Compatibility.md). <br>- Las actualizaciones a Windows Server 2016 solo (es decir, sin actualizar aplicaciones) deben utilizar instrucciones específicas de la aplicación.|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas características nuevas funcionan mejor en un host físico de Windows Server 2016 que ejecute Hyper-V. Siga las guías de migración según corresponda. <br>- También puede mantener su SO actual y usar una máquina virtual que se ejecute en un host de Windows Server 2016 o Microsoft Azure. Póngase en contacto con su revendedor de EA, TAM o Microsoft para conocer las opciones de soporte técnico extendido a través de [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabajo de aplicaciones de ISV**|-Las actualizaciones a Windows Server 2016 deben utilizar instrucciones específicas de la aplicación. <br>- Para obtener más información sobre la compatibilidad de Windows Server con aplicaciones que no son de Microsoft, visite el [portal de certificación del logotipo de Windows Server](https://azure.microsoft.com/publish-your-app/).|- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas características nuevas funcionan mejor en un host físico de Windows Server 2016 que ejecute Hyper-V. Siga las guías de migración según corresponda. <br>- También puede mantener su SO actual y usar una máquina virtual que se ejecute en un host de Windows Server 2016 o Microsoft Azure. Póngase en contacto con su revendedor de EA, TAM o Microsoft para conocer las opciones de soporte técnico extendido a través de [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabajo de aplicaciones personalizadas**|- Consulte a los desarrolladores de las aplicaciones sobre su compatibilidad con Windows Server 2016 y solicíteles las instrucciones de actualización. <br>- Aproveche Microsoft Azure para probar la aplicación en Windows Server 2016 antes de cambiar. <br>- Vea todas las opciones en la sección siguiente.|- Consulte con sus desarrolladores de aplicaciones sobre la compatibilidad con Windows Server 2016 e instrucciones de actualización. <br>- Aproveche Microsoft Azure para probar la aplicación en Windows Server 2016 antes de cambiar. <br>- Para sacar partido de las nuevas características de Windows Server 2016, implementar hardware nuevo o instalar Windows Server 2016 en una máquina virtual en un host existente. Algunas características nuevas funcionan mejor en un host físico de Windows Server 2016 que ejecute Hyper-V. <br>- Vea todas las opciones en la sección siguiente.|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>Opciones completas para migrar servidores que ejecutan aplicaciones personalizadas o internas en versiones de Windows Server anteriores a Windows Server 2016

Hay más opciones que nunca para ayudarle a usted y a sus clientes a aprovechar las características de Windows Server 2016 con una afectación mínima a sus servicios y cargas de trabajo actuales.

- Pruebe el sistema operativo más reciente con su aplicación mediante la descarga de la versión de evaluación de [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) para realizar pruebas en sus instalaciones. Una vez realizadas las pruebas y confirmada la calidad, puede realizar una conversión de licencia simple con una clave de licencia de minorista (requiere reiniciar).

- [Microsoft Azure](https://azure.microsoft.com) también se puede utilizar en versiones de prueba para garantizar que su aplicación personalizada funciona en el sistema operativo de servidor más reciente. Una vez realizadas las pruebas y confirmada la calidad, [migre a la versión más reciente de Windows Server](./installation-and-upgrade.md#upgrade) en sus instalaciones. 

- También puede, una vez realizadas las pruebas y confirmada la calidad, usar [Microsoft Azure](https://azure.microsoft.com) como ubicación permanente para su servicio o aplicación personalizados. Esto permite que el servidor antiguo permanezca disponible hasta que esté listo para cambiar al nuevo servidor en Azure.

    - Si ya tiene Software Assurance para Windows Server, ahorre dinero mediante la implementación con la [ventaja de uso híbrido de Microsoft Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- En la mayoría de los casos, [Microsoft Azure](https://azure.microsoft.com) puede usarse para hospedar la misma aplicación en la versión anterior de Windows Server que se está ejecutando actualmente. Migre la aplicación y la carga de trabajo a una máquina virtual con el sistema operativo de su elección mediante imágenes de [Azure Marketplace](https://azure.microsoft.com/marketplace/).

    - Si ya tiene Software Assurance para Windows Server, ahorre dinero mediante la implementación con la [ventaja de uso híbrido de Microsoft Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- El programa [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx) para Windows Server proporciona derechos de nueva versión como ventaja. Junto a otras ventajas, los servidores con Software Assurance pueden actualizarse a la última versión de Windows Server cuando llegue el momento sin tener que adquirir una licencia. 

## <a name="additional-resources"></a>Recursos adicionales

- [Características eliminadas o en desuso en Windows Server 2016](deprecated-features.md)
- Para opciones de actualización y migración de servidor generales, visite [Opciones de actualización y conversión para Windows Server 2016](Supported-Upgrade-Paths.md).
- Para obtener más información acerca del ciclo de vida del producto y los niveles de soporte técnico, consulte [Directiva de ciclo de vida de soporte técnico: preguntas más frecuentes](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq).
