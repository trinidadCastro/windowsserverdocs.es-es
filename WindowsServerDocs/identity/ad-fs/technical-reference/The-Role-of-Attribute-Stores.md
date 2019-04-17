---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: El rol de tiendas de atributo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
 >Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>El rol de tiendas de atributo
Los servicios de federación de Active Directory usa el término "tiendas atributo" para hacer referencia a los directorios o bases de datos que una organización se usa para almacenar sus cuentas de usuario y sus valores de atributo asociado. Después de que se configura en una organización de proveedor de identidad, AD FS recupera estos valores de atributo de la tienda y crea reclamaciones basados en esa información para que una aplicación Web o servicio que está hospedado en una organización de terceros de confianza puede tomar las decisiones de autorización adecuada cada vez que un usuario federado \ (es decir, un usuario cuya cuenta se almacena en la organization\ del proveedor de identidad) intenta obtener acceso a la aplicación o servicio.  
  
Para obtener más información acerca de cómo se generan las notificaciones, consulta [el rol de reclamaciones](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>¿Cómo se almacena atributo encaja en tus objetivos de la implementación de AD FS  
La ubicación de la tienda de atributo de usuario y la ubicación desde el que los usuarios se autentican determinar el diseño de AD FS para admitir las identidades de usuario. Dependiendo de dónde se encuentra el almacén de atributo y donde los usuarios tendrán acceso a la aplicación \ (en una intranet o en la Internet\), puedes usar uno de los siguientes objetivos de implementación:  
  
-   [Proporcionar tu Active Directory a los usuarios acceso a tus aplicaciones para notificaciones y los servicios](https://technet.microsoft.com/library/dd807071.aspx): en este objetivo, los usuarios de tu organización, obtener acceso a una aplicación de AD FS seguro o servicio \ (tu propia aplicación o servicio o de un asociado aplicación o service\) cuando los usuarios han iniciado sesión en Active Directory en la intranet corporativa.  
  
-   [Proporcionar tu Active Directory a los usuarios acceso a las aplicaciones y servicios de otras organizaciones](https://technet.microsoft.com/library/dd807123.aspx): en este objetivo, los usuarios de tu organización, obtener acceso a una aplicación de AD FS seguro o servicio \ (tu propia aplicación o servicio o de un asociado aplicación o service\) cuando los usuarios han iniciado sesión a un almacén de atributo en la intranet corporativa y cuando inicien sesión remotamente desde Internet.  
  
-   [Proporcionar a los usuarios en otra organización acceso a tus aplicaciones para notificaciones y los servicios](https://technet.microsoft.com/library/dd807099.aspx): en este objetivo, cuentas de usuario en otra organización que se encuentran en un almacén de atributo de la intranet corporativa de la organización deben tener acceso a un AD FS seguro de la aplicación de la organización. Este objetivo también funciona cuando cuentas de usuario basada en consumer\ que se encuentran en un almacén de atributo en la red del perímetro de la organización deben proporcionarse con acceso a un AD FS seguro de la aplicación de la organización.  
  
Según la ubicación de la tienda de atributo y otros requisitos de la organización, puedes combinar varios de estos objetivos de implementación para completar el diseño de la implementación de AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Atributo tiendas que son compatibles con AD FS  
Almacenes de base de datos que puedes usar para extraer los valores de atributo definido por el administrator\ y llenar reclamaciones con esos valores y AD FS admite una amplia gama de directorio. AD FS admite ninguno de los siguientes directorios o bases de datos como almacenes de atributo:  
  
-   Active Directory en Windows Server 2003, servicios de dominio de Active Directory \(AD DS\) en Windows Server 2008, AD DS en Windows Server 2012 y 2012 R2 y Windows Server 2016. 
  
-   Todas las ediciones de Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 y SQL Server 2016  
  
-   Almacenes de atributo personalizado  
  

