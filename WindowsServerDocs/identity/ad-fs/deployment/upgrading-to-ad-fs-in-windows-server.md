---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Actualizar a AD FS en Windows Server 2016
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Actualizar a AD FS en Windows Server 2016 con una base de datos WID

>Se aplica a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Cambio de un conjunto de Windows Server 2012 R2 AD FS a una granja de servidores de AD FS de Windows Server 2016  
El siguiente documento describe cómo actualizar el conjunto de AD FS Windows Server 2012 R2 a AD FS en Windows Server 2016 cuando estés usando una base de datos WID.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>AD FS de la actualización a Windows Server 2016 FBL  
La característica de nivel de comportamiento de granja (FBL) es nueva en AD FS de Windows Server 2016.   Esta función es el conjunto de ancho y determina las características que puede usar la granja de servidores de AD FS.   De manera predeterminada, la FBL en un conjunto de Windows Server 2012 R2 AD FS está en la FBL de Windows Server 2012 R2.  

Un servidor de Windows Server 2016 AD FS puede agregarse a un conjunto de Windows Server 2012 R2 y funcionará en el mismo FBL como un Windows Server 2012 R2.  Cuando tengas un servidor de Windows Server 2016 AD FS operen en escenarios de este modo, el conjunto se dice que "mixed".  Sin embargo, no podrás sacar provecho de las nuevas características de Windows Server 2016 hasta que se ha aumentado la FBL a Windows Server 2016.  Con una granja mixta:  

-   Los administradores pueden agregar nuevas, los servidores de federación de Windows Server 2016 a un conjunto existente de Windows Server 2012 R2.  Como resultado, la batería está en "modo mixto" y opera el nivel de comportamiento de granja de servidores de Windows Server 2012 R2.  Para garantizar un comportamiento coherente entre la granja de servidores, nuevas características de Windows Server 2016 no se pueden configurar o usan en este modo.  

-   Una vez que se han quitado todos los servidores de federación de Windows Server 2012 R2 de la granja de modo mixto y en el caso de una granja de servidores WID, uno de los servidores de federación de Windows Server 2016 nuevo ha ascendido a la función del nodo principal, el administrador, a continuación, puede provocar la FBL de Windows Server 2012 R2 para Windows Server 2016.  Como resultado, las nuevas características de AD FS Windows Server 2016 después puede configurar y usar.  

-   Como resultado de la característica de granja mixto, AD FS Windows Server 2012 R2, las organizaciones que desean para actualizar a Windows Server 2016 no tendrá que implementar un conjunto totalmente nuevo, exportar e importar datos de configuración.  En su lugar, puede agregar nodos de Windows Server 2016 a un conjunto existente mientras está en línea y solo incurrir en el tiempo de inactividad relativamente breve que participan en el generan FBL.  

Ten en cuenta que en el modo mixto granja de servidores, el conjunto de AD FS no es capaz de las nuevas características o la funcionalidad introducida en AD FS en Windows Server 2016.  Esto significa que las organizaciones que desean probar nuevas características no pueden hacer esto hasta que se genera el FBL.  Por lo tanto, si la organización está buscando para probar las nuevas características antes de rasing la FBL, tendrás que implementar una batería independiente para hacer esto.  

El resto de la es documento proporciona los pasos para agregar un servidor de federación de Windows Server 2016 a un entorno de Windows Server 2012 R2 y, a continuación, generar el FBL a Windows Server 2016.  Estos pasos se realizan en un entorno de prueba descrito en el siguiente diagrama de arquitectura.  

> [!NOTE]  
> Para poder mover a AD FS en Windows Server 2016 FBL, debes quitar todos los nodos de Windows 2012 R2.  Simplemente no se puede actualizar un sistema operativo de Windows Server 2012 R2 a Windows Server 2016 y que se convertirá en un nodo de 2016.  Necesitarás quitarla y reemplazarlo por un nuevo nodo de 2016.
>
> Actualizar la FBL al usar SQL para almacenar la configuración de AD FS crea una nueva base de datos de administración "AdfsConfigurationV3".

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>Para actualizar el conjunto de AD FS a nivel de comportamiento de Windows Server 2016 granja  

1.  Usar el administrador del servidor instalar el rol de servicios de federación de Active Directory en el equipo con Windows Server 2016  

2.  Usar al Asistente para configuración de AD FS, incorpora el nuevo servidor de Windows Server 2016 al conjunto de AD FS existente.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  En el servidor de federación de Windows Server 2016, abre Administración de AD FS.    Ten en cuenta que no hay nada aparece como un servidor de federación de este no es el servidor principal.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Una vez completada, en el servidor de Windows Server 2016 la unión, abre PowerShell y ejecuta el siguiente cmdlt: conjunto AdfsSyncProperties-PrimaryComputer de rol  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  En el servidor de AD FS Windows Server 2012 R2 original, abre PowerShell y ejecuta el siguiente cmdlt: conjunto AdfsSyncProperties-rol SecondaryComputer - PrimaryComputerName {FQDN}  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  En el Proxy de aplicación Web, abrir PowerShell y ejecuta el cmdlt followoing: instalación WebApplicationProxy - CertificateThumbprint {SSLCert} - fsname fsname - TrustCred $trustcred  

7.  Ahora en el servidor de federación de Windows Server 2016 abre Administración de AD FS.  Ten en cuenta que ahora todos los nodos aparecen porque la función principal se ha transferido a este servidor.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  Con los medios de instalación de Windows Server 2016, abre un símbolo del sistema y navega al directorio support\adprep.  Ejecuta lo siguiente: adprep /forestprep.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. Una vez que finaliza ejecutar adprep/preparar  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. Ahora en el servidor de Windows Server 2016 abre PowerShell y ejecuta el siguiente cmdlt: AdfsFarmBehaviorLevelRaise invocar  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. Cuando se te pide, escriba Y. Esto empezará a generar el nivel.  Cuando se complete correctamente haber generado el FBL.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> Si los servidores de AD FS utilizan SQL para la configuración, ahora se creó una nueva base de datos manamgement con el nombre "AdfsConfiguraionV3". 

12. Ahora, si vas a la administración de AD FS, verás los nuevos nodos que se han agregado de AD FS en Windows Server 2016  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. De igual modo, puedes usar la cmdlt PowerShell: Get-AdfsFarmInformation para mostrar el FBL actual.  

    ![actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
