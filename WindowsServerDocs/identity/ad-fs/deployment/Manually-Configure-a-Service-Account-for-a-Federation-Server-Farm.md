---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente una cuenta de servicio para una granja de servidores de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c30215f5f8e39bb97452fccaaef8d1bb0469dc31
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855348"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente una cuenta de servicio para una granja de servidores de federación

Si piensa configurar un entorno de granja de servidores de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS\), debe crear y configurar una cuenta de servicio dedicada en Active Directory Domain Services \(AD DS\) en la que residirá la granja de servidores. Después, configura cada servidor de federación de la granja para que use esta cuenta. Debe completar las siguientes tareas en su organización si desea permitir que los equipos cliente de la red corporativa se autentiquen en cualquiera de los servidores de Federación de una granja de AD FS mediante la autenticación integrada de Windows.  

> [!IMPORTANT]
> A partir de AD FS 3,0 (Windows Server 2012 R2), AD FS admite el uso de una [cuenta de servicio administrada de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\) como cuenta de servicio.  Esta es la opción recomendada, ya que elimina la necesidad de administrar la contraseña de la cuenta de servicio a lo largo del tiempo.  En este documento se describe el caso alternativo del uso de una cuenta de servicio tradicional, como en los dominios que siguen ejecutando un nivel funcional de dominio de Windows Server 2008 R2 o anterior \(nivel funcional\).

> [!NOTE]  
> Debe realizar las tareas de este procedimiento una sola vez para toda la granja de servidores de federación. Más adelante, cuando crees un servidor de federación mediante el Asistente para la configuración de servidores de federación, debes especificar esta misma cuenta en la página **Cuenta de servicio** del asistente en cada servidor de federación de la granja.  
  
#### <a name="create-a-dedicated-service-account"></a>Crear una cuenta de servicio dedicada  
  
1.  Cree una cuenta de servicio de\/de usuario dedicada en el bosque de Active Directory que se encuentra en la organización del proveedor de identidades. Esta cuenta es necesaria para que el protocolo de autenticación Kerberos funcione en un escenario de granja de servidores y para permitir el paso\-a través de la autenticación en cada uno de los servidores de Federación. Use esta cuenta solo para los fines de la granja de servidores de Federación.  
  
2.  Edita las propiedades de la cuenta de usuario y selecciona la casilla **La contraseña nunca caduca**. Esta acción garantiza que esta función de cuenta de servicio no se interrumpe debido al requisito de cambio de contraseña de dominio.  
  
    > [!NOTE]  
    > Si se usa la cuenta de servicio de red para esta cuenta dedicada, se producirán errores aleatorios al intentar acceder con la autenticación integrada de Windows, porque los vales de Kerberos no se validan de un servidor a otro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para establecer el SPN de la cuenta de servicio  
  
1.  Dado que la identidad del grupo de aplicaciones para el AD FS AppPool se está ejecutando como un usuario de dominio\/cuenta de servicio, debe configurar el nombre de la entidad de seguridad de servicio \(SPN\) para esa cuenta en el dominio con la herramienta de línea de\-de comandos setspn. exe. Setspn. exe se instala de forma predeterminada en los equipos que ejecutan Windows Server 2008. Ejecute el siguiente comando en un equipo que esté unido al mismo dominio en el que reside el usuario\/cuenta de servicio:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por ejemplo, en un escenario en el que todos los servidores de Federación están agrupados en el sistema de nombres de dominio \(DNS\) nombre de host y el nombre de la cuenta de servicio que se asigna al AD FS AppPool se denomina adfs2farm, escriba el comando como se indica a continuación y, a continuación, presione ENTRAR:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Solo es necesario realizar esta tarea una vez para esta cuenta.  
  
2.  Después de cambiar la identidad de AD FS AppPool a la cuenta de servicio, establezca las listas de control de acceso \(ACL\) en la base de datos de SQL Server para permitir el acceso de lectura a esta nueva cuenta, de modo que el AD FS AppPool pueda leer los datos de la Directiva.  
  

