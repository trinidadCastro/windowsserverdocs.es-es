---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: El papel de los almacenes de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a1543f2c935c2ef76ea014567b18bfc778c7401
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407382"
---
# <a name="the-role-of-attribute-stores"></a>El papel de los almacenes de atributos
Servicios de federación de Active Directory (AD FS) usa el término "almacenes de atributos" para hacer referencia a directorios o bases de datos que usa una organización para almacenar sus cuentas de usuario y sus valores de atributo asociados. Una vez configurada en una organización del proveedor de identidades, AD FS recupera estos valores de atributo del almacén y crea notificaciones basadas en esa información para que una aplicación web o un servicio hospedado en una organización de usuario de confianza pueda hacer lo adecuado decisiones de autorización cada vez que \(un usuario federado tiene un usuario cuya cuenta se almacena en\) la organización del proveedor de identidades intenta tener acceso a la aplicación o servicio.  
  
Para obtener más información sobre cómo se generan notificaciones, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Cómo se adaptan los almacenes de atributos a sus objetivos de implementación de AD FS  
La ubicación del almacén de atributos de usuario y la ubicación desde la que los usuarios se autentican determinan cómo diseñar AD FS para admitir las identidades de usuario. En función de dónde se encuentre el almacén de atributos y de que los usuarios accedan a la aplicación \(en una intranet o en Internet\), puede usar uno de los siguientes objetivos de implementación:  
  
-   [Proporcionar a los usuarios de Active Directory acceso a sus aplicaciones y servicios para notificaciones](https://technet.microsoft.com/library/dd807071.aspx): en este objetivo, los usuarios de la organización acceden a una aplicación \(o servicio protegido por AD FS, ya sea su propia aplicación o servicio o de un asociado aplicación o servicio\) cuando los usuarios inician sesión en Active Directory en la intranet corporativa.  
  
-   [Proporcionar a los usuarios Active Directory acceso a las aplicaciones y servicios de otras organizaciones](https://technet.microsoft.com/library/dd807123.aspx): en este objetivo, los usuarios de la organización acceden a una aplicación o \(servicio protegido por AD FS, ya sea su propia aplicación o servicio o un aplicación o servicio\) del asociado cuando los usuarios inician sesión en un almacén de atributos de la intranet corporativa y cuando inician sesión de forma remota desde Internet.  
  
-   [Proporcionar a los usuarios de otra organización acceso a las aplicaciones y servicios habilitados para notificaciones](https://technet.microsoft.com/library/dd807099.aspx): en este objetivo, las cuentas de usuario de otra organización que se encuentran en un almacén de atributos de la intranet corporativa de la organización deben tener acceso a un AD FS: aplicación protegida en su organización. Este objetivo también funciona cuando se\-debe proporcionar a las cuentas de usuario basadas en el consumidor que se encuentran en un almacén de atributos de la red perimetral de la organización acceso a una aplicación protegida por AD FS de la organización.  
  
En función de la ubicación del almacén de atributos y otros requisitos de su organización, puede combinar varios de estos objetivos de implementación para completar el diseño de la implementación de AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Almacenes de atributos compatibles con AD FS  
AD FS admite una amplia gama de almacenes de directorios y bases de datos que puede usar para extraer\-valores de atributo definidos por el administrador y rellenar notificaciones con esos valores. AD FS admite cualquiera de los siguientes directorios o bases de datos como almacenes de atributos:  
  
-   Active Directory en Windows Server 2003, Active Directory Domain Services \(AD DS\) en Windows Server 2008, AD DS en Windows Server 2012 y 2012 R2 y Windows Server 2016. 
  
-   Todas las ediciones de Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 y SQL Server 2016  
  
-   Almacenes de atributos personalizados  
  

