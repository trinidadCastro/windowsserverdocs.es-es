---
description: 'Más información sobre: diseño de SSO Web'
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Diseño de SSO web
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e61402f12af51036c3eea739288ddd94e1644e5e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049343"
---
# <a name="web-sso-design"></a>Diseño de SSO web

En el diseño de inicio de sesión único Web de \- \- SSO en \( \) servicios de Federación de Active Directory (AD FS) \( AD FS \) , los usuarios deben autenticarse una sola vez para tener acceso a varias \- aplicaciones o servicios protegidos de AD FS. En este diseño todos los usuarios son externos y no existe ninguna confianza de federación porque no hay organizaciones asociadas. Normalmente, se implementa este diseño cuando se desea proporcionar acceso individual al cliente o al consumidor a una o AD FS varias aplicaciones o servicios protegidos a través de Internet, tal como se muestra en la siguiente ilustración.

![diseño de SSO Web](media/adfs2_WebSSODesign.gif)

Con el diseño de SSO Web, una organización que normalmente hospeda una AD FS \- aplicación o servicio protegido en una red perimetral puede mantener un almacén independiente de cuentas de cliente en la red perimetral, lo que facilita aislar las cuentas de clientes de las cuentas de empleados.

Puede administrar las cuentas locales de los clientes de la red perimetral mediante Active Directory Domain Services \( AD DS \) , SQL Server o un almacén de atributos personalizado.

Este diseño coincide con el objetivo de implementación de [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).

Para obtener una lista de tareas detalladas que puedes utilizar para planear e implementar el diseño SSO Web, consulta [Checklist: Implementing a Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
