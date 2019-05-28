---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: El papel de los almacenes de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bec3ebf1bd12b260dbbb245a6a905277ff0d749f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188542"
---
# <a name="the-role-of-attribute-stores"></a>El papel de los almacenes de atributos
Servicios de federación de Active Directory usa el término "almacenes de atributos" para hacer referencia a directorios o bases de datos que una organización usa para almacenar sus cuentas de usuario y sus valores de atributo asociados. Una vez que se configura en una organización del proveedor de identidad, AD FS recupera estos valores de atributo en el almacén y crea notificaciones basadas en esa información para que una aplicación Web o servicio que se hospeda en una organización de usuario de confianza pueda tomar adecuado las decisiones de autorización cada vez que un usuario federado \(un usuario cuya cuenta se almacena en la organización del proveedor de identidades\) intenta obtener acceso a la aplicación o servicio.  
  
Para obtener más información sobre cómo se generan notificaciones, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Cómo se adaptan los almacenes de atributos a sus objetivos de implementación de AD FS  
La ubicación del almacén de atributos de usuario y la ubicación desde la que los usuarios se autentican determinan el modo de diseño de AD FS para admitir las identidades de usuario. Dependiendo de dónde se encuentra el almacén de atributos y donde los usuarios tendrán acceso a la aplicación \(en una intranet o en Internet\), puede usar uno de los objetivos de implementación siguientes:  
  
-   [Proporcionar su Active Directory Users Access a Your Claims-Aware Applications and Services](https://technet.microsoft.com/library/dd807071.aspx): en este objetivo, los usuarios de su organización acceso a una aplicación de AD FS protegida o servicio \(su propia aplicación o un servicio o un aplicación o el servicio del socio\) cuando los usuarios inician sesión Active Directory en la intranet corporativa.  
  
-   [Proporcione su Active Directory Users Access a las aplicaciones y servicios de otras organizaciones](https://technet.microsoft.com/library/dd807123.aspx): en este objetivo, los usuarios de su organización acceso a una aplicación de AD FS protegida o servicio \(su propia aplicación o un servicio o un aplicación o el servicio del socio\) cuando los usuarios inician sesión un almacén de atributos en la intranet corporativa y cuando inician sesión remotamente desde Internet.  
  
-   [Proporcionar a los usuarios de otra organización acceso a Your Claims-Aware Applications and Services](https://technet.microsoft.com/library/dd807099.aspx): en este objetivo, las cuentas de usuario de otra organización que se encuentran en un almacén de atributos en la intranet corporativa de la organización deben tener acceso a un servidor de AD FS: aplicación protegida en su organización. Este objetivo también funciona al consumidor\-las cuentas de usuario basada en que se encuentran en un almacén de atributos en la red perimetral de su organización deben proporcionarse con acceso a una instancia de AD FS: protege la aplicación en su organización.  
  
Según la ubicación del almacén de atributos y otros requisitos de su organización, puede combinar varios de estos objetivos de implementación para completar el diseño de la implementación de AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Almacenes de atributos compatibles con AD FS  
AD FS admite una amplia gama de directorio y la base de datos almacena que puede usar para extraer el administrador\-define los valores de atributo y rellenar notificaciones con esos valores. AD FS es compatible con cualquiera de los siguientes directorios o bases de datos como almacenes de atributos:  
  
-   Active Directory en Windows Server 2003, servicios de dominio de Active Directory \(AD DS\) en Windows Server 2008, AD DS en Windows Server 2012 y 2012 R2 y Windows Server 2016. 
  
-   Todas las ediciones de Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 y SQL Server 2016  
  
-   Almacenes de atributos personalizados  
  

