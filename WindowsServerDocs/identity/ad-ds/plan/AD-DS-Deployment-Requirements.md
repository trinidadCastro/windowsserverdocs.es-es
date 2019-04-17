---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: "Requisitos de implementación de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura de su entorno existente determina la estrategia de implementación de Windows Server 2008 Active Directory Domain Services (AD DS). Si vas a crear un entorno de AD DS y no tiene una estructura de dominio existente, completar el diseño de AD DS antes de empezar a crear el entorno de AD DS. A continuación, puede implementar un dominio raíz del bosque nuevo e implementar el resto de la estructura de dominios según su diseño.  
  
Además, como parte de la implementación de AD DS, decides actualizar y reestructurar su entorno. Por ejemplo, si tu organización tiene una estructura de dominios de Windows 2000, puede realizar una actualización en contexto de algunos dominios y reestructuración de otros. Además, puedes decidir reducir la complejidad de tu entorno reestructuración de dominios entre bosques o reestructuración de dominios en un bosque después de implementar AD DS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implementación de un dominio de raíz del bosque de Windows Server 2008  
Dominio raíz del bosque proporciona la base para la infraestructura de bosque de AD DS. Para implementar AD DS, primero debes implementar un dominio raíz del bosque. Para ello, debe revisar el diseño de AD DS; configurar el servicio DNS para el dominio raíz del bosque; crear el dominio raíz, que consiste en implementar controladores de dominio raíz de bosque, configuración de la topología de sitio para el dominio raíz del bosque y configurar roles de maestro de operaciones (también conocido como operaciones de maestro único flexibles o FSMO); y generar los niveles funcionales de bosque y dominio. La siguiente ilustración muestra el proceso general de la implementación de un dominio raíz del bosque.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Para obtener más información, consulta [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Implementación de dominios regionales de Windows Server 2008  
Después de completar la implementación de dominio raíz del bosque, estás listo para implementar cualquier dominios de Windows Server 2008 regionales nuevas que se especifican en el diseño. Para ello, debes implementar controladores de dominio para cada dominio regional. La siguiente ilustración muestra el proceso de implementación de dominios regionales.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Para obtener más información, consulta [Regional dominios de implementación de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Actualización de dominios de Active Directory para Windows Server 2008  
Actualizar los dominios de Windows 2000 o Windows Server 2003a dominios de Windows Server 2008 es una forma eficaz y sencilla para aprovechar las características adicionales de Windows Server 2008 y funcionalidad. Puedes actualizar dominios para mantener la configuración de red y el dominio actual al tiempo que mejora la seguridad, escalabilidad y facilidad de uso de la infraestructura de red. Actualización de Windows 2000 o Windows Server 2003a Windows Server 2008, requiere una configuración mínima en la red. Actualizar tenga un impacto mínimo en las operaciones de usuario. Para obtener más información, consulta [actualizar dominios de Microsoft Active Directory para Windows Server 2008 y Windows Server 2008 R2 AD DS dominios](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Reestructuración de dominios de AD DS  
Al reestructurar dominios entre bosques de Windows Server 2008 (reestructuración entre bosques), puedes reducir el número de dominios en el entorno y, por tanto, reducir la sobrecarga y complejidad administrativa. Al migrar objetos entre bosques como parte de esta reestructuración de proceso, el origen dominio y destino dominio entornos existen simultáneamente. Esto permite revertir al entorno de origen durante la migración, si es necesario.  
  
Cuando reestructurar dominios de Windows Server 2008 en un bosque de Windows Server 2008 (dentro del bosque reestructuración), puedes consolidar la estructura del dominio y, por tanto, reducir la sobrecarga y complejidad administrativa. Cuando reestructurar dominios en un bosque, las cuentas migradas ya no existen en el dominio de origen.  
  
Para obtener más información sobre cómo usar la herramienta Active Directory migración (ADMT) versión 3.1 (ADMT v3.1) reestructurar dominios, consulta [Guía de migración de ADMT v3.1](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


