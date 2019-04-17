---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: "Configurar manualmente una cuenta de servicio para una granja de servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurar manualmente una cuenta de servicio para una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si quieres configurar un entorno de granja de servidores de federación de los servicios de federación de Active Directory \(AD FS\), debes crear y configurar una cuenta de servicio dedicada en \(AD DS\) los servicios de dominio de Active Directory donde reside el conjunto. A continuación, configuras cada servidor de federación de la granja para usar esta cuenta. Debes completar las siguientes tareas de la organización cuando quieras que los equipos cliente en la red corporativa para autenticar a cualquiera de los servidores de federación de una batería de AD FS mediante la autenticación integrada de Windows.  
  
> [!NOTE]  
> Tienes que realizar las tareas de este procedimiento solo una vez para la granja de servidores de federación todo. Más adelante, cuando creas un servidor de federación usando el Asistente para configuración del servidor de federación de AD FS, debes especificar esta misma cuenta en la **cuenta de servicio** página del asistente en cada servidor de federación de la granja de servidores.  
  
#### <a name="create-a-dedicated-service-account"></a>Crear una cuenta de servicio dedicado  
  
1.  Crear una cuenta de servicio/user\ dedicada en el bosque de Active Directory que se encuentra en la organización de proveedor de identidad. Esta cuenta es necesaria para el protocolo de autenticación Kerberos para permitir pass\ a través de la autenticación en cada uno de los servidores de federación y trabajar en un escenario de granja de servidores. Usa esta cuenta únicamente para los fines de la granja de servidores de federación.  
  
2.  Modificar las propiedades de la cuenta de usuario y seleccione la **contraseña nunca expira** casilla de verificación. Esta acción garantiza que la función de la cuenta de servicio no se interrumpe como resultado de los requisitos de cambio de contraseña de dominio.  
  
    > [!NOTE]  
    > Usando la cuenta de servicio de red para esta cuenta dedicada producirá errores aleatorios cuando se intente el acceso mediante la autenticación integrada de Windows, como resultado de los vales Kerberos no validar desde un servidor a otro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Para establecer el SPN de la cuenta de servicio  
  
1.  Porque la identidad del grupo de AD FS AppPool se ejecuta como una cuenta de servicio/user\ de dominio, debes configurar el \(SPN\) nombre Principal del servicio de esa cuenta en el dominio con la herramienta de línea de Web\ Setspn.exe. Setspn.exe se instala de forma predeterminada en equipos que ejecutan Windows Server 2008. Ejecuta el siguiente comando en un equipo que está unido al mismo dominio donde reside la cuenta de servicio/user\:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Por ejemplo, en un escenario en el que todos los servidores de federación están agrupados en la fs.fabrikam.com de nombre de host de sistema de nombres de dominio \(DNS\) y el nombre de cuenta de servicio que se asigna a AD FS AppPool se denomina adfs2farm, escribe el comando siguiente y, a continuación, presiona ENTRAR:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Es necesario completar esta tarea solo una vez para esta cuenta.  
  
2.  Después de cambia la identidad de AD FS AppPool a la cuenta de servicio, Establece el acceso control listas \(ACLs\) en la base de datos de SQL Server para permitir el acceso de lectura a esta nueva cuenta para que AD FS AppPool puedan leer los datos de la directiva.  
  

