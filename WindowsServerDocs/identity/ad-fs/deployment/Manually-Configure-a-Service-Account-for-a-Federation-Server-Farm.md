---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente una cuenta de servicio para una granja de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8240903b3c446d4f02ca93dc053e520480f5e8ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359492"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente una cuenta de servicio para una granja de servidores de federación

Si piensa configurar un entorno de granja de servidores de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1, debe crear y configurar una cuenta de servicio dedicada en Active Directory Domain Services \(AD DS @ no__t-3, donde la granja residirá. Después, configura cada servidor de federación de la granja para que use esta cuenta. Debe completar las siguientes tareas en su organización si desea permitir que los equipos cliente de la red corporativa se autentiquen en cualquiera de los servidores de Federación de una granja de AD FS mediante la autenticación integrada de Windows.  

> [!IMPORTANT]
> A partir de AD FS 3,0 (Windows Server 2012 R2), AD FS admite el uso de una [cuenta de servicio administrada de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA @ no__t-2 como cuenta de servicio.  Esta es la opción recomendada, ya que elimina la necesidad de administrar la contraseña de la cuenta de servicio a lo largo del tiempo.  En este documento se trata el caso alternativo del uso de una cuenta de servicio tradicional, como en los dominios que siguen ejecutando un nivel funcional de dominio de Windows Server 2008 R2 o anterior \(DFL @ no__t-1.

> [!NOTE]  
> Debe realizar las tareas de este procedimiento una sola vez para toda la granja de servidores de federación. Más adelante, cuando cree un servidor de Federación mediante el Asistente para configuración de servidor de Federación de AD FS, debe especificar esta misma cuenta en la página del asistente de **cuenta de servicio** en cada servidor de Federación de la granja.  
  
#### <a name="create-a-dedicated-service-account"></a>Crear una cuenta de servicio dedicada  
  
1.  Cree una cuenta de usuario dedicado @ no__t-0service en el bosque Active Directory que se encuentra en la organización del proveedor de identidades. Esta cuenta es necesaria para que el protocolo de autenticación Kerberos funcione en un escenario de granja de servidores y para permitir la autenticación Pass @ no__t-0through en cada uno de los servidores de Federación. Use esta cuenta solo para los fines de la granja de servidores de Federación.  
  
2.  Edita las propiedades de la cuenta de usuario y activa la casilla **La contraseña nunca expira** . Con esta acción te aseguras de que la función de esta cuenta de servicio no se interrumpa nunca como resultado de los requisitos de cambio de contraseña del dominio.  
  
    > [!NOTE]  
    > Si se usa la cuenta de servicio de red para esta cuenta dedicada, se producirán errores aleatorios al intentar acceder con la autenticación integrada de Windows, porque los vales de Kerberos no se validan de un servidor a otro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para establecer el SPN de la cuenta de servicio  
  
1.  Dado que la identidad del grupo de aplicaciones para el AD FS AppPool se ejecuta como una cuenta de usuario de dominio @ no__t-0service, debe configurar el nombre de entidad de seguridad de servicio \(SPN @ no__t-2 para esa cuenta en el dominio con la herramienta setspn. exe @ no__t-3line. Setspn. exe se instala de forma predeterminada en los equipos que ejecutan Windows Server 2008. Ejecute el siguiente comando en un equipo que esté unido al mismo dominio donde reside la cuenta User @ no__t-0service:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por ejemplo, en un escenario en el que todos los servidores de Federación están agrupados en el sistema de nombres de dominio \(DNS @ no__t-1 nombre de host fs.fabrikam.com y el nombre de la cuenta de servicio que se asigna a la AD FS AppPool se denomina adfs2farm, escriba el comando como se indica a continuación. a continuación, presione ENTRAR:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Solo es necesario realizar esta tarea una vez para esta cuenta.  
  
2.  Una vez que la identidad de AD FS AppPool se cambia a la cuenta de servicio, establezca las listas de control de acceso \(ACLs @ no__t-1 en la base de datos de SQL Server para permitir el acceso de lectura a esta nueva cuenta a fin de que el AD FS AppPool pueda leer los datos de la Directiva.  
  

