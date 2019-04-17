---
title: Actualizar controladores de dominio a Windows Server 2016
description: "Este documento describe cómo actualizar de Windows Server 2012 R2 a Windows Server 2016"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187972c2f44a4d7f91b1b3ac1c905529564cfa6d
ms.sourcegitcommit: f748c6c4ce700b0787ffdd1fca620c21c4331fd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Actualizar controladores de dominio a Windows Server 2016

Se aplica a: Windows Server 2016

Este tema proporciona información general sobre los servicios de dominio de Active Directory en Windows Server 2016 y explica el proceso para actualizar los controladores de dominio de Windows Server 2012 o Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Requisitos previos
El procedimiento recomendado para actualizar un dominio es promover controladores de dominio que ejecutan versiones más recientes de Windows Server y degradación los controladores de dominio anteriores según sea necesario. Ese método es preferible actualizar el sistema operativo de un controlador de dominio. Esta lista describe los pasos generales que seguir antes de promover un controlador de dominio que ejecute una versión más reciente de Windows Server: 

1.  Comprueba que el servidor de destino cumple los requisitos del sistema. 
2.  Comprueba la compatibilidad de la aplicación. 
3.  Revisa las recomendaciones para pasar a Windows Server 2016 
4.  Comprueba la configuración de seguridad. Para obtener más información, consulta [relacionados con los cambios de comportamiento y características de desuso en AD DS en Windows Server 2016](../../../get-started\deprecated-features.md). 
5.  Comprobar la conectividad al servidor de destino desde el equipo donde tiene previsto ejecutar la instalación. 
6.  Comprobar la disponibilidad de funciones de maestro de operación necesarias: 
    - Para instalar el primer controlador de dominio que ejecuta Windows Server 2016 en un bosque y dominio de existente, el equipo donde se ejecuta la instalación debe conectividad a la **maestro de esquema** para poder ejecutar adprep /forestprep y el maestro de infraestructura para poder ejecutar adprep /domainprep. 
    - Para instalar el primer controlador de dominio en un dominio donde ya ha ampliado el esquema del bosque, sólo necesita conectividad a maestro de infraestructura. 
    - Para instalar o quitar un dominio en un bosque existente, necesita conectividad a la **maestro nombres de dominio**. 
    - Cualquier instalación del controlador de dominio también requiere conectividad a la **maestro RID.** 
    - Si vas a instalar el primer controlador de dominio de solo lectura en un bosque existente, debes tener conectividad con el patrón de infraestructura para cada partición de directorio de aplicación, también conocido como un contexto de nomenclatura no del dominio o NDS NDNC. 

### <a name="installation-steps-and-required-administrative-levels"></a>Pasos de instalación y niveles administrativos necesarios
La siguiente tabla proporciona un resumen de los pasos de actualización y los requisitos de permiso para realizar estos pasos

|Acción de instalación|Requisitos de credenciales|
| ----- | ----- |
|Instala un bosque nuevo|Administrador local en el servidor de destino|
|Instalar un nuevo dominio en un bosque existente|Administradores de empresa|
|Instalar un controlador de dominio adicional en un dominio existente|Administradores de dominio|
|Ejecutar adprep /forestprep|Administradores de dominio, administradores de empresa y administradores de esquema|
|Ejecutar adprep /domainprep|Administradores de dominio|
|Ejecutar adprep /domainprep /gpprep|Administradores de dominio|
|Ejecutar adprep /rodcprep|Administradores de empresa|

Para obtener más información sobre nuevas características de Windows Server 2016, consulta [Novedades en Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>En lugar de actualización admitidas
Controladores de dominio que ejecutan versiones de 64 bits de Windows Server 2012 o Windows Server 2012 R2 se pueden actualizar a Windows Server 2016. Solo las actualizaciones de versión de 64 bits son compatibles porque Windows Server 2016 solo está disponible en una versión de 64 bits.

|Si ejecutas esta edición:|Puedes actualizar a estas ediciones:|
| ----- | ----- |   
|Estándar de Windows Server 2012|Windows Server 2016 Standard o Datacenter|   
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Grupo de trabajo de Windows Storage Server 2012|Grupo de trabajo de Windows Storage Server 2016|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Grupo de trabajo de Windows Storage Server 2012 R2|Grupo de trabajo de Windows Storage Server 2016|    

Para obtener más información sobre las rutas de actualización admitidas, consulta [admite rutas de actualización](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep y Domainprep
Si está realizando una actualización en contexto de un controlador de dominio existente al sistema operativo Windows Server 2016, deberás ejecutar adprep /forestprep y adprep /domainprep manualmente.  Adprep /forestprep se debe ejecutar solo una vez en el bosque.  Adprep /domainprep debe ejecutarse una vez en cada dominio en el que tienes los controladores de dominio que se va a actualizar a Windows Server 2016.

Si la promoción de nuevo un servidor de Windows Server 2016 no es necesario ejecutar estos manualmente.  Estos están integrados en PowerShell y experiencias de administrador del servidor.

Para obtener más información sobre cómo ejecutar adprep consulta [Adprep ejecutando](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Requisitos y características de nivel funcionales
Windows Server 2016 requiere un nivel funcional del bosque de Windows Server 2003. Es decir, antes de poder agregar un controlador de dominio que ejecute Windows Server 2016 a un bosque de Active Directory existente, el nivel funcional del bosque debe ser Windows Server 2003 o superior. Si el bosque contiene controladores de dominio que ejecutan Windows Server 2003 o posterior pero el funcional del bosque nivel sigue siendo Windows 2000, también se bloquea la instalación. 

Controladores de dominio de Windows 2000 deben quitarse antes de agregar controladores de dominio de Windows Server 2016 en el bosque. En este caso, ten en cuenta el flujo de trabajo siguiente: 


1. Instalar controladores de dominio que ejecutan Windows Server 2003 o posterior. Estos controladores de dominio se pueden implementar en una versión de evaluación de Windows Server. Este paso también requiere la ejecución de adprep.exe para esa versión del sistema operativo como requisito previo. 
2.  Quita los controladores de dominio de Windows 2000. Específicamente, correctamente degradar o forzar la eliminación de controladores de dominio de Windows Server 2000 del dominio y usadas usuarios de Active Directory y equipos para quitar las cuentas de controlador de dominio para todos los controladores de dominio eliminado. 
3.  Aumentar el nivel funcional del bosque en Windows Server 2003 o superior. 
4.  Instalar controladores de dominio que ejecutan Windows Server 2016. 
5.  Quitar controladores de dominio que ejecutan versiones anteriores de Windows Server. 

### <a name="rolling-back-functional-levels"></a>Revertir niveles funcionales

Después de establecer el nivel funcional del bosque (FFL) en un valor determinado, no se puede revertir o reducir el nivel funcional del bosque, con las siguientes excepciones: 

- Si estás actualizando desde Windows Server 2012 R2 FFL, puede reducir volver a Windows Server 2012 R2. 
- Si estás actualizando desde Windows Server 2008 R2 FFL, puede reducir volver a Windows Server 2008 R2.

Después de establecer el nivel funcional del dominio en un valor determinado, no se puede revertir o bajar el nivel funcional de dominio, con las siguientes excepciones: 

- Cuando se eleva el nivel funcional de dominio a Windows Server 2016 y si el nivel funcional del bosque es Windows Server 2012 o inferior, tienes la opción de revertir el nivel funcional del dominio vuelve a Windows Server 2012 o Windows Server 2012 R2 

Para obtener más información sobre las características que están disponibles en los niveles inferiores de funcionales, consulta [niveles funcionales de conocimiento Active Directory Domain Services (AD DS)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilidad de AD DS con otros roles de servidor y sistemas operativos Windows
AD DS no se admite en los siguientes sistemas operativos de Windows: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

No se puede instalar AD DS en un servidor que también se ejecuta los siguientes roles de servidor o los servicios de rol: 

- Microsoft Hyper-V Server 2016
- Agente de conexión a Escritorio remoto 

## <a name="administration-of-windows-server-2016-servers"></a>Administración de los servidores de Windows Server 2016
Usar las herramientas de administración remota servidor para Windows 10 para administrar controladores de dominio y otros servidores que ejecutan Windows Server 2016. Puedes ejecutar Windows Server 2016 Remote Server Administration Tools en un equipo que ejecute Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Paso a paso para la actualización a Windows Server 2016
El siguiente es un ejemplo sencillo de actualización del bosque Contoso de Windows Server 2012 R2 a Windows Server 2016.

![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  Únete a la nueva versión de Windows Server 2016 en el bosque. Reiniciar cuando se te solicite. 
![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  Inicia sesión en el nuevo Windows Server 2016 con una cuenta de administrador de dominio.
3.  En **administrador del servidor**, en **agregar Roles y características**, instalar **los servicios de dominio de Active Directory** en el nuevo Windows Server 2016. Esto ejecutará automáticamente adprep en el 2012 R2 bosque y dominio.
![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  En **administrador del servidor**, haz clic en el triángulo amarillo y en la lista desplegable, haz clic en **promover el servidor a un controlador de dominio**. 
![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  En la **configuración de implementación** pantalla, selecciona **agregar un controlador de dominio a un bosque existente** y haz clic en siguiente. 
![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  En la **opciones de controlador de dominio** de pantalla, escribe el **el modo de restauración de servicios de directorio (DSRM)** contraseña y haz clic en siguiente. 
7.  Haga clic en el resto de las pantallas **siguiente**. 
8.  En la **comprobar requisitos previos** de pantalla, haz clic en **instalar**. Una vez que finalice el reinicio se vuelva a iniciar sesión.
9.  En el servidor de Windows Server 2012 R2, en **administrador del servidor**, en herramientas, selecciona **módulo Active Directory para Windows PowerShell**. 
![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. En windows PowerShell usa el movimiento-ADDirectoryServerOperationMasterRole para mover las funciones FSMO. Puedes escribir el nombre de cada OperationMasterRole - o números se usa para especificar los roles. Los números. Para obtener más información, consulta [ADDirectoryServerOperationMasterRole de movimiento](https://technet.microsoft.com/library/hh852302.aspx)

``` powershell
Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
```

![Actualización](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Comprueba que se han movido yendo al servidor de Windows Server 2016, en las funciones **administrador del servidor**, en **herramientas**, selecciona **módulo Active Directory para Windows PowerShell**. Usa el `Get-ADDomain` y `Get-ADForest` cmdlets para ver los titulares de rol FSMO.
![Actualizar] (media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
! [Actualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Degrade y quitar el controlador de dominio de Windows Server 2012 R2. Para obtener información sobre la degradación de un controlador de dominio, consulta el tema [degradar controladores de dominio y dominios](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Una vez que se degrada y quita el servidor puede provocar la funcional del bosque y niveles funcionales de dominio a Windows Server 2016.


## <a name="next-steps"></a>Pasos siguientes
-   [Novedades en el dominio de Active Directory de servicios de instalación y desinstalación](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Instalar servicios de dominio de Active Directory & #40; Nivel 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Niveles funcionales de Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
