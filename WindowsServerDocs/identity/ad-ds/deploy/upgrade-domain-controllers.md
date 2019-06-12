---
title: Actualizar controladores de dominio a Windows Server 2016
description: Este documento describe cómo actualizar desde Windows Server 2012 R2 a Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6f3907426fd1124c5ed0a411a155490a2a537239
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719682"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Actualizar controladores de dominio a Windows Server 2016

Se aplica a: Windows Server 2016

Este tema proporciona información general sobre servicios de dominio de Active Directory en Windows Server 2016 y explica el proceso para actualizar los controladores de dominio de Windows Server 2012 o Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Requisitos previos
El método recomendado para actualizar un dominio es promover controladores de dominio que ejecuten versiones más recientes de Windows Server y degradación los controladores de dominio anteriores según sea necesario. Ese método es preferible a actualizar el sistema operativo de un controlador de dominios existente. Esta lista describen los pasos generales a seguir antes de promover un controlador de dominio que ejecuta una versión más reciente de Windows Server: 

1. Confirme que el servidor de destino reúne los requisitos del sistema. 
2. Compruebe la compatibilidad de aplicaciones. 
3. Revisar las recomendaciones para migrar a Windows Server 2016 
4. Compruebe la configuración de seguridad. Para obtener más información, consulte [características desusadas y cambios de comportamiento relativos a AD DS en Windows Server 2016](https://docs.microsoft.com/en-us/windows-server/get-started/deprecated-features). 
5. Compruebe la conectividad del servidor de destino desde el equipo donde tenga intención de realizar la instalación. 
6. Compruebe la disponibilidad de los roles de maestro de operaciones necesarios: 
   - Para instalar el primer controlador de dominio que ejecuta Windows Server 2016 en un bosque y dominio existentes, el equipo donde se ejecuta la instalación necesita conectarse a la **maestro de esquema** con el fin de ejecutar adprep /forestprep y el maestro de infraestructura Para poder ejecutar adprep/domainprep. 
   - Para instalar el primer controlador de dominio en un dominio en el que el esquema del bosque ya esté extendido, solo es necesario conectarse al maestro de infraestructura. 
   - Para instalar o quitar un dominio en un bosque existente, es necesario conectarse a la **maestro de nomenclatura de dominio**. 
   - Instalación de cualquier controlador de dominio también requiere conectividad a la **maestro de RID.** 
   - Si instala el primer controlador de dominio de solo lectura en un bosque existente, necesitará conectarse al maestro de infraestructura para cada partición de directorio de aplicaciones, que se conoce también como contexto de nomenclatura no de dominio (o NDNC). 

### <a name="installation-steps-and-required-administrative-levels"></a>Pasos de instalación y niveles administrativos necesarios
En la tabla siguiente proporciona un resumen de los pasos de actualización y los requisitos de permisos para realizar estos pasos.

|Acción de instalación|Requisitos de credenciales|
| ----- | ----- |
|Instalar un bosque nuevo|Administrador local en el servidor de destino|
|Instalar un dominio nuevo en un bosque existente|Administradores de empresas|
|Instalar más controladores de dominio en un dominio existente|Admins. del dominio|
|Ejecutar adprep /forestprep|Administradores de esquema, Administradores de empresas y Admins. del dominio|
|Ejecutar adprep /domainprep|Admins. del dominio|
|Ejecutar adprep /domainprep /gpprep|Admins. del dominio|
|Ejecutar adprep /rodcprep|Administradores de empresas|

Para obtener más información sobre las nuevas características en Windows Server 2016, consulte [cuáles son las novedades en Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Rutas de acceso de actualización en contexto admitidas
Los controladores de dominio que ejecutan versiones de 64 bits de Windows Server 2012 o Windows Server 2012 R2 se pueden actualizar a Windows Server 2016. Actualizaciones de la versión de 64 bits solo se admiten porque Windows Server 2016 solo se incluye en una versión de 64 bits.

|Si ejecuta esta edición:|Puede realizar una actualización a estas ediciones:|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|    

Para obtener más información sobre las rutas de actualización admitidas, consulte [rutas de actualización compatibles](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep y Domainprep
Si está realizando una actualización en contexto de un controlador de dominio existente para el sistema operativo Windows Server 2016, deberá ejecutar adprep /forestprep y adprep /domainprep manualmente.  Adprep/forestprep para ejecutarse una sola vez en el bosque.  Adprep/domainprep debe ejecutarse una vez en cada dominio en el que tiene controladores de dominio que se va a actualizar a Windows Server 2016.

Si desea promocionar a un nuevo servidor de Windows Server 2016 que no es necesario ejecutarlos manualmente.  Estos están integrados en PowerShell y las experiencias del administrador del servidor.

Para obtener más información sobre la ejecución de adprep, consulte [ejecutar Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Requisitos y características de niveles funcionales
Windows Server 2016 requiere un nivel funcional del bosque de Windows Server 2003. Es decir, antes de poder agregar un controlador de dominio que ejecuta Windows Server 2016 en un bosque de Active Directory existente, el nivel funcional del bosque debe ser Windows Server 2003 o superior. Si el bosque contiene controladores de dominio que ejecutan Windows Server 2003 o posterior, pero el nivel funcional de dicho bosque sigue siendo Windows 2000, la instalación se bloqueará igualmente. 

Los controladores de dominio de Windows 2000 deben quitarse antes de agregar controladores de dominio de Windows Server 2016 en el bosque. En este caso, considere el siguiente flujo de trabajo: 


1. Instale controladores de dominio que ejecuten Windows Server 2003 o posterior. Estos controladores de dominio se pueden implementar en una versión de evaluación de Windows Server. Este paso también requiere que se ejecuta adprep.exe para esa versión del sistema operativo como un requisito previo. 
2.  Quite los controladores de dominio de Windows 2000. En concreto, quite a la fuerza o disminuya de nivel correctamente los controladores de dominio de Windows Server 2000 del dominio y utilice Usuarios y equipos de Active Directory para quitar las cuentas de controlador de dominio correspondientes a todos los controladores de dominio eliminados. 
3.  Eleve el nivel funcional del bosque a Windows Server 2003 o posterior. 
4.  Instalar los controladores de dominio que ejecutan Windows Server 2016. 
5.  Quite los controladores de dominio que ejecuten versiones anteriores de Windows Server. 

### <a name="rolling-back-functional-levels"></a>Revertir los niveles funcionales

Después de establecer el nivel funcional del bosque (FFL) en un determinado valor, no puede revertir ni bajar el nivel funcional del bosque, con las siguientes excepciones: 

- Si va a actualizar desde Windows Server 2012 R2 FFL, puede bajarlo a Windows Server 2012 R2. 
- Si va a actualizar desde Windows Server 2008 R2 FFL, puede bajarlo a Windows Server 2008 R2.

Después de establecer el nivel funcional del dominio en un determinado valor, no puede revertir ni bajar el nivel funcional de dominio, con las siguientes excepciones: 

- Cuando eleve el nivel funcional del dominio a Windows Server 2016 y si el nivel funcional del bosque es Windows Server 2012 o inferior, tiene la opción de revertir el nivel funcional del dominio hacer una copia de Windows Server 2012 o Windows Server 2012 R2 

Para obtener más información sobre las características disponibles a niveles funcionales inferiores, consulte [Descripción de los niveles funcionales de Servicios de dominio de Active Directory (AD DS)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilidad de AD DS con otros roles del servidor y sistemas operativos Windows
AD DS no es compatible con los siguientes sistemas operativos: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

AD DS no se puede instalar en un servidor donde también se ejecuten los siguientes roles o servicios del servidor: 

- Microsoft Hyper-V Server 2016
- Agente de conexión a Escritorio remoto 

## <a name="administration-of-windows-server-2016-servers"></a>Administración de servidores de Windows Server 2016
Use las herramientas de administración remota Server para Windows 10 para administrar controladores de dominio y otros servidores que ejecuten Windows Server 2016. Puede ejecutar Windows Server 2016 Server herramientas de administración remota en un equipo que ejecuta Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Paso a paso para actualizar a Windows Server 2016
El siguiente es un ejemplo sencillo de actualizar el bosque de Contoso desde Windows Server 2012 R2 a Windows Server 2016.

![Actualizar versión](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. Únase a la nueva versión de Windows Server 2016 en el bosque. Reinicie cuando se le solicite. 
   ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2. Inicie sesión en el nuevo Windows Server 2016 con una cuenta de administrador de dominio.
3. En **administrador del servidor**, en **agregar Roles y características**, instalar **Active Directory Domain Services** en Windows Server 2016. Esto ejecutará automáticamente adprep en el dominio y bosque de R2 2012.
   ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4. En **administrador del servidor**, haga clic en el triángulo amarillo y en la lista desplegable, haga clic en **promover el servidor a controlador de dominio**. 
   ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5. En el **configuración de implementación** pantalla, seleccione **agregar un controlador de dominio a un bosque existente** y haga clic en siguiente. 
   ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6. En el **opciones del controlador de dominio** pantalla, escriba el **el modo de restauración de servicios de directorio (DSRM)** contraseña y haga clic en siguiente. 
7. Para el resto de las pantallas, haga clic en **siguiente**. 
8. En el **comprobación de requisitos previos** pantalla, haga clic en **instalar**. Una vez que ha completado el reinicio se puede volver inicio de sesión.
9. En el servidor de Windows Server 2012 R2, en **administrador del servidor**, en herramientas, seleccione **módulo Active Directory para Windows PowerShell**. 
   ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. En windows PowerShell use el ADDirectoryServerOperationMasterRole Move para mover los roles FSMO. Puede escribir el nombre de cada OperationMasterRole - o use números para especificar los roles. Para obtener más información, consulte [ADDirectoryServerOperationMasterRole de movimiento](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![Actualizar versión](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Compruebe los roles se han movido para ello, vaya al servidor de Windows Server 2016, en **administrador del servidor**, en **herramientas**, seleccione **módulo Active Directory para Windows PowerShell**. Use la `Get-ADDomain` y `Get-ADForest` cmdlets para ver los titulares del rol FSMO.
    ![Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
    ![actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Disminuir de nivel y quitar el controlador de dominio de Windows Server 2012 R2. Para obtener información acerca de degradar un controlador de dominio, consulte [degradación de controladores de dominio y dominios](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Una vez que se degradan y quitado el servidor puede generar el funcional del bosque y los niveles funcionales de dominio a Windows Server 2016.


## <a name="next-steps"></a>Pasos siguientes
-   [Novedades sobre la instalación y eliminación de Active Directory Domain Services (AD DS)](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Instalar servicios de dominio de Active Directory &#40;nivel 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Niveles funcionales de Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
