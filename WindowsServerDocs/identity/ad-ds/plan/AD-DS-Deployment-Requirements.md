---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Requisitos de implementación de AD DS
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a74f99afaacef050bc828eff8f84a76875c644fd
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941325"
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implementación de AD DS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura del entorno existente determina la estrategia para implementar Windows Server 2008 Active Directory Domain Services (AD DS). Si va a crear un entorno de AD DS y no tiene una estructura de dominio existente, complete el diseño de AD DS antes de empezar a crear el entorno de AD DS. A continuación, puede implementar un nuevo dominio raíz del bosque e implementar el resto de la estructura de dominio de acuerdo con el diseño.

Además, como parte de la implementación de AD DS, puede decidir actualizar y reestructurar el entorno. Por ejemplo, si su organización tiene una estructura de dominio de Windows 2000 existente, puede realizar una actualización en contexto de algunos dominios y reestructurar los demás. Además, puede decidir reducir la complejidad de su entorno mediante la reestructuración de dominios entre bosques o la reestructuración de dominios dentro de un bosque después de implementar AD DS.

## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implementar un dominio raíz del bosque de Windows Server 2008
El dominio raíz del bosque proporciona la base para la infraestructura de AD DS bosque. Para implementar AD DS, primero debe implementar un dominio raíz del bosque. Para ello, debe revisar el diseño del AD DS; configurar el servicio DNS para el dominio raíz del bosque; cree el dominio raíz del bosque, que consiste en la implementación de controladores de dominio raíz del bosque, la configuración de la topología del sitio para el dominio raíz del bosque y la configuración de roles de maestro de operaciones (también conocidos como operaciones de maestro único flexible o FSMO). y elevan los niveles funcionales del bosque y del dominio. En la ilustración siguiente se muestra el proceso general de implementación de un dominio raíz del bosque.

![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)

Para obtener más información, vea [implementar un dominio raíz del bosque de Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="deploying-windows-server-2008-regional-domains"></a>Implementación de dominios regionales de Windows Server 2008
Después de completar la implementación del dominio raíz del bosque, está listo para implementar los nuevos dominios regionales de Windows Server 2008 especificados por el diseño. Para ello, debe implementar controladores de dominio para cada dominio regional. En la ilustración siguiente se muestra el proceso de implementación de dominios regionales.

![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)

Para obtener más información, consulte [implementación de dominios regionales de Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755118(v=ws.10)).

## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Actualización de dominios de Active Directory a Windows Server 2008
La actualización de dominios de Windows 2000 o Windows Server 2003 a dominios de Windows Server 2008 es una manera eficaz y sencilla de aprovechar las características y funcionalidades adicionales de Windows Server 2008. Puede actualizar los dominios para mantener la configuración actual de la red y del dominio a la vez que mejora la seguridad, la escalabilidad y la capacidad de administración de la infraestructura de red. La actualización de Windows 2000 o Windows Server 2003 a Windows Server 2008 requiere una configuración de red mínima. La actualización también tiene poco impacto en las operaciones del usuario. Para obtener más información, consulte [upgrading Active Directory Domains to Windows server 2008 and Windows server 2008 R2 AD DS Domains](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)).

## <a name="restructuring-ad-ds-domains"></a>Reestructurar dominios de AD DS
Cuando reestructure dominios entre bosques de Windows Server 2008 (reestructuración entre bosques), puede reducir el número de dominios del entorno y, por tanto, reducir la complejidad y la complejidad administrativa. Al migrar objetos entre bosques como parte de este proceso de reestructuración, los entornos de dominio de origen y de dominio de destino existen simultáneamente. Esto permite revertir al entorno de origen durante la migración, si es necesario.

Al reestructurar los dominios de Windows Server 2008 dentro de un bosque de Windows Server 2008 (reestructuración intrabosque), puede consolidar la estructura de dominio y, por tanto, reducir la complejidad y la sobrecarga administrativa. Al reestructurar los dominios de un bosque, las cuentas migradas ya no existen en el dominio de origen.

Para obtener más información acerca de cómo usar la herramienta de migración de Active Directory (ADMT) versión 3,1 (ADMT v 3.1) para reestructurar los dominios, vea la [Guía de ADMT: migración y reestructuración de dominios de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)).
