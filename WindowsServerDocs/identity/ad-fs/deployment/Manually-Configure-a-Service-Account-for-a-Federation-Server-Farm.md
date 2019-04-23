---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente una cuenta de servicio para una granja de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7d215c80c03236df9479aff8046981741dfc83e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838156"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente una cuenta de servicio para una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si va a configurar un entorno de granja de servidores de federación de Active Directory Federation Services \(AD FS\), debe crear y configurar una cuenta de servicio dedicada en servicios de dominio de Active Directory \(AD DS\) donde residirá la granja de servidores. Después, configura cada servidor de federación de la granja para que use esta cuenta. Debe completar las tareas siguientes en su organización cuando desee permitir que los equipos cliente en la red corporativa se autentiquen en cualquiera de los servidores de federación en una granja de AD FS mediante la autenticación integrada de Windows.  

> [!IMPORTANT]
> A partir de AD FS 3.0 (Windows Server 2012 R2), AD FS admite el uso de un [cuenta de servicio administrada de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\) como la cuenta de servicio.  Esta es la opción recomendada, ya que elimina la necesidad de administrar la contraseña de cuenta de servicio con el tiempo.  Este documento tratan el caso del uso de una cuenta de servicio tradicionales, como en los dominios que sigue en ejecución un Windows Server 2008 R2 o anterior nivel funcional del dominio alternativo \(DFL\).

> [!NOTE]  
> Debe realizar las tareas de este procedimiento una sola vez para toda la granja de servidores de federación. Más adelante, cuando crea un servidor de federación usando el Asistente para configuración del servidor de federación de AD FS, debe especificar esta misma cuenta en el **cuenta de servicio** página del asistente en cada servidor de federación en la granja de servidores.  
  
#### <a name="create-a-dedicated-service-account"></a>Crear una cuenta de servicio dedicada  
  
1.  Crear un usuario dedicado\/cuenta en el bosque de Active Directory que se encuentra en la organización del proveedor de identidad de servicio. Esta cuenta es necesaria para el protocolo de autenticación de Kerberos funcione en un escenario de granja de servidores y para permitir que pase\-mediante la autenticación en cada uno de los servidores de federación. Use esta cuenta solo para los fines de la granja de servidores de federación.  
  
2.  Edita las propiedades de la cuenta de usuario y activa la casilla **La contraseña nunca expira** . Con esta acción te aseguras de que la función de esta cuenta de servicio no se interrumpa nunca como resultado de los requisitos de cambio de contraseña del dominio.  
  
    > [!NOTE]  
    > Si se usa la cuenta de servicio de red para esta cuenta dedicada, se producirán errores aleatorios al intentar acceder con la autenticación integrada de Windows, porque los vales de Kerberos no se validan de un servidor a otro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para establecer el SPN de la cuenta de servicio  
  
1.  Dado que la identidad del grupo de aplicaciones para el grupo de aplicaciones de AD FS se ejecuta como un usuario de dominio\/cuentas de servicio debe configurar el nombre de entidad de servicio \(SPN\) para esa cuenta en el dominio con el comando Setspn.exe\-herramienta de línea. Setspn.exe se instala de forma predeterminada en equipos que ejecutan Windows Server 2008. Ejecute el siguiente comando en un equipo que está unido al mismo dominio donde el usuario\/reside la cuenta de servicio:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por ejemplo, en un escenario en que la federación todos los servidores están agrupados en el sistema de nombres de dominio \(DNS\) nombre de host fs.fabrikam.com y el nombre de cuenta de servicio que se asigna para el grupo de aplicaciones de AD FS se denomina adfs2farm, escriba el comando como sigue y, a continuación, presione ENTRAR:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Solo es necesario realizar esta tarea una vez para esta cuenta.  
  
2.  Después de cambia la identidad del grupo de aplicaciones de AD FS para la cuenta de servicio, establezca las listas de control de acceso \(ACL\) en la base de datos de SQL Server para permitir el acceso de lectura a esta nueva cuenta para que AD FS AppPool pueda leer los datos de la directiva.  
  

