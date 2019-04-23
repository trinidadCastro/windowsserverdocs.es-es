---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Requisitos de implementación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d21491f5ce9c15ecc514e4be24a91de28b0fd0ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890976"
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura de su entorno existente determina la estrategia para la implementación de Windows Server 2008 Active Directory Domain Services (AD DS). Si va a crear un entorno de AD DS y no tiene una estructura de dominios existente, completar el diseño de AD DS antes de comenzar a crear el entorno de AD DS. A continuación, puede implementar un dominio raíz del bosque y aplicar el resto de la estructura de dominios según su diseño.  
  
Además, como parte de la implementación de AD DS, decide actualizar y reestructurar el entorno. Por ejemplo, si su organización tiene una estructura de dominio de Windows 2000 existente, puede realizar una actualización en contexto de algunos dominios y reestructuración de otros. Además, puede decidir reducir la complejidad de su entorno mediante la reestructuración de dominios entre bosques o reestructurar dominios en un bosque después de implementar AD DS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implementación de un dominio raíz de bosque de Windows Server 2008  
El dominio raíz del bosque, proporciona la base para la infraestructura de bosque de AD DS. Para implementar AD DS, primero debe implementar un dominio raíz del bosque. Para ello, debe revisar el diseño de AD DS; configurar el servicio DNS para el dominio raíz del bosque; crear el dominio raíz del bosque, que consta de la implementación de controladores de dominio raíz del bosque, configurar la topología del sitio para el dominio raíz del bosque y configurar roles de maestro de operaciones (también conocido como operaciones de maestro únicas flexible o FSMO); y elevar los niveles funcionales de bosque y dominio. La siguiente ilustración muestra el proceso general de la implementación de un dominio raíz del bosque.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Para obtener más información, consulte [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Implementar dominios regionales de Windows Server 2008  
Después de completar la implementación del dominio raíz del bosque, está listo para implementar cualquier nueva dominios regionales de Windows Server 2008 que se especifican el diseño. Para ello, debe implementar controladores de dominio para cada dominio regional. La siguiente ilustración muestra el proceso de implementación de dominios regionales.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Para obtener más información, consulte [implementar Windows Server 2008 dominios regionales](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Actualización de dominios de Active Directory a Windows Server 2008  
Actualizar los dominios de Windows 2000 o Windows Server 2003 a dominios de Windows Server 2008 es una forma eficaz y sencilla para aprovechar la funcionalidad y características adicionales de Windows Server 2008. Puede actualizar dominios para mantener la configuración de red y el dominio actual al tiempo que mejora la seguridad, escalabilidad y facilidad de uso de la infraestructura de red. Actualizar desde Windows 2000 o Windows Server 2003 a Windows Server 2008 requiere una configuración mínima en la red. También se actualizar tiene poco impacto en las operaciones de usuario. Para obtener más información, consulte [actualizar dominios de Active Directory para Windows Server 2008 y Windows Server 2008 R2 AD DS dominios](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Reestructuración de dominios de AD DS  
Al reestructurar dominios entre bosques de Windows Server 2008 (reestructuración entre bosques), puede reducir el número de dominios en su entorno y, por tanto, reducir la sobrecarga y complejidad administrativa. Al migrar objetos entre bosques como parte de este proceso de reestructuración, tanto el origen destino dominio entornos de dominios y existen simultáneamente. Esto permite revertir al entorno de origen durante la migración, si es necesario.  
  
Al reestructurar dominios de Windows Server 2008 en un bosque de Windows Server 2008 (reestructuración), puede consolidar la estructura de dominios y, por tanto, reducir la sobrecarga y complejidad administrativa. Al reestructurar dominios de un bosque, las cuentas migradas ya no existen en el dominio de origen.  
  
Para obtener más información sobre cómo usar la herramienta Active Directory Migration (ADMT) versión 3.1 (ADMT v3.1) para reestructurar los dominios, vea [Guía de migración de ADMT v3.1](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


