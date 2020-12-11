---
description: Más información acerca de cómo configurar manualmente una cuenta de servicio para una granja de servidores de Federación
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurar manualmente una cuenta de servicio para una granja de servidores de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: afedde422f0a46975fcbb61912773a992c688309
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043333"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente una cuenta de servicio para una granja de servidores de federación

Si piensa configurar un entorno de granja de servidores de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) , debe crear y configurar una cuenta de servicio dedicada en Active Directory Domain Services \( AD DS \) donde residirá la granja de servidores. Después, configura cada servidor de federación de la granja para que use esta cuenta. Debe completar las siguientes tareas en su organización si desea permitir que los equipos cliente de la red corporativa se autentiquen en cualquiera de los servidores de Federación de una granja de AD FS mediante la autenticación integrada de Windows.

> [!IMPORTANT]
> A partir de AD FS 3,0 (Windows Server 2012 R2), AD FS admite el uso de una [cuenta de servicio administrada de grupo](../../../security/group-managed-service-accounts/group-managed-service-accounts-overview.md) \( gMSA \) como cuenta de servicio.  Esta es la opción recomendada, ya que elimina la necesidad de administrar la contraseña de la cuenta de servicio a lo largo del tiempo.  En este documento se trata el caso alternativo del uso de una cuenta de servicio tradicional, como en los dominios que siguen ejecutando un nivel funcional de dominio de Windows Server 2008 R2 o anterior \( nivel funcional \) .

> [!NOTE]
> Debe realizar las tareas de este procedimiento una sola vez para toda la granja de servidores de federación. Más adelante, cuando crees un servidor de federación mediante el Asistente para la configuración de servidores de federación, debes especificar esta misma cuenta en la página **Cuenta de servicio** del asistente en cada servidor de federación de la granja.

#### <a name="create-a-dedicated-service-account"></a>Crear una cuenta de servicio dedicada

1.  Cree una cuenta de \/ servicio de usuario dedicada en el bosque Active Directory que se encuentra en la organización del proveedor de identidades. Esta cuenta es necesaria para que el protocolo de autenticación Kerberos funcione en un escenario de granja de servidores y para permitir la \- autenticación de paso a través en cada uno de los servidores de Federación. Use esta cuenta solo para los fines de la granja de servidores de Federación.

2.  Edita las propiedades de la cuenta de usuario y activa la casilla **La contraseña nunca expira**. Con esta acción te aseguras de que la función de esta cuenta de servicio no se interrumpa nunca como resultado de los requisitos de cambio de contraseña del dominio.

    > [!NOTE]
    > Si se usa la cuenta de servicio de red para esta cuenta dedicada, se producirán errores aleatorios al intentar acceder con la autenticación integrada de Windows, porque los vales de Kerberos no se validan de un servidor a otro.

#### <a name="to-set-the-spn-of-the-service-account"></a>Para establecer el SPN de la cuenta de servicio

1.  Dado que la identidad del grupo de aplicaciones para el AD FS AppPool se ejecuta como una cuenta de servicio de usuario de dominio \/ , debe configurar el SPN de nombre de entidad de seguridad \( de servicio \) para esa cuenta en el dominio con la herramienta de línea de comandos Setspn.exe \- . Setspn.exe se instala de forma predeterminada en los equipos que ejecutan Windows Server 2008. Ejecute el siguiente comando en un equipo que esté unido al mismo dominio donde reside la cuenta de servicio de usuario \/ :

    ```
    setspn -a host/<server name> <service account>
    ```

    Por ejemplo, en un escenario en el que todos los servidores de Federación están agrupados en el nombre de host DNS del sistema de nombres de dominio \( \) FS.fabrikam.com y el nombre de la cuenta de servicio que se asigna al AD FS AppPool se denomina adfs2farm, escriba el comando como se indica a continuación y, a continuación, presione ENTRAR:

    ```
    setspn -a host/fs.fabrikam.com adfs2farm
    ```

    Solo es necesario realizar esta tarea una vez para esta cuenta.

2.  Después de cambiar la identidad de AD FS AppPool a la cuenta de servicio, establezca las \( ACL de las listas de control de acceso \) en la base de datos SQL Server para permitir el acceso de lectura a esta nueva cuenta a fin de que el AD FS AppPool pueda leer los datos de la Directiva.

