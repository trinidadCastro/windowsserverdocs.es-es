---
title: Actualizar a AD FS en Windows Server 2016 con SQL Server
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Actualizar a AD FS en Windows Server 2016 con SQL Server

>Se aplica a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Cambio de un conjunto de Windows Server 2012 R2 AD FS a una granja de servidores de AD FS de Windows Server 2016  
El siguiente documento describe cómo actualizar el conjunto de AD FS Windows Server 2012 R2 a AD FS en Windows Server 2016 al usar un servidor SQL Server para la base de datos de AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>AD FS de la actualización a Windows Server 2016 FBL  
La característica de nivel de comportamiento de granja (FBL) es nueva en AD FS de Windows Server 2016.   Esta función es el conjunto de ancho y determina las características que puede usar la granja de servidores de AD FS.   De manera predeterminada, la FBL en un conjunto de Windows Server 2012 R2 AD FS está en la FBL de Windows Server 2012 R2.  

Un servidor de Windows Server 2016 AD FS puede agregarse a un conjunto de Windows Server 2012 R2 y funcionará en el mismo FBL como un Windows Server 2012 R2.  Cuando tengas un servidor de Windows Server 2016 AD FS operen en escenarios de este modo, el conjunto se dice que "mixed".  Sin embargo, no podrás sacar provecho de las nuevas características de Windows Server 2016 hasta que se ha aumentado la FBL a Windows Server 2016.  Con una granja mixta:  

-   Los administradores pueden agregar nuevas, los servidores de federación de Windows Server 2016 a un conjunto existente de Windows Server 2012 R2.  Como resultado, la batería está en "modo mixto" y opera el nivel de comportamiento de granja de servidores de Windows Server 2012 R2.  Para garantizar un comportamiento coherente entre la granja de servidores, nuevas características de Windows Server 2016 no se pueden configurar o usan en este modo.  

-   Una vez que se han quitado todos los servidores de federación de Windows Server 2012 R2 de la granja de modo mixto y en el caso de una granja de servidores WID, uno de los servidores de federación de Windows servir 2016 nuevo ha ascendido a la función del nodo principal, el administrador, a continuación, puede provocar la FBL de Windows Server 2012 R2 para Windows Server 2016.  Como resultado, las nuevas características de AD FS Windows Server 2016 después puede configurar y usar.  

-   Como resultado de la característica de granja mixto, AD FS Windows Server 2012 R2, las organizaciones que desean para actualizar a Windows Server 2016 no tendrá que implementar un conjunto totalmente nuevo, exportar e importar datos de configuración.  En su lugar, puede agregar nodos de Windows Server 2016 a un conjunto existente mientras está en línea y solo incurrir en el tiempo de inactividad relativamente breve que participan en el generan FBL.  

Ten en cuenta que en el modo mixto granja de servidores, el conjunto de AD FS no es capaz de las nuevas características o la funcionalidad introducida en AD FS en Windows Server 2016.  Esto significa que las organizaciones que desean probar nuevas características no pueden hacer esto hasta que se genera el FBL.  Por lo tanto, si la organización está buscando para probar las nuevas características antes de rasing la FBL, tendrás que implementar una batería independiente para hacer esto.  

El resto de la es documento proporciona los pasos para agregar un servidor de federación de Windows Server 2016 a un entorno de Windows Server 2012 R2 y, a continuación, generar el FBL a Windows Server 2016.  Estos pasos se realizan en un entorno de prueba descrito en el siguiente diagrama de arquitectura.  

> [!NOTE]  
> Para poder mover a AD FS en Windows Server 2016 FBL, debes quitar todos los nodos de Windows 2012 R2.  Simplemente no se puede actualizar un sistema operativo de Windows Server 2012 R2 a Windows Server 2016 y que se convertirá en un nodo de 2016.  Necesitarás quitarla y reemplazarlo por un nuevo nodo de 2016.  

En el siguiente diagrama de arquitectura muestra la configuración que se usó para validar y grabar los pasos siguientes.

![Arquitectura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Unir el servidor de Windows 2016 AD FS a la granja de servidores de AD FS

1.  Usar el administrador del servidor instalar el rol de servicios de federación de Active Directory en el equipo con Windows Server 2016  

2.  Usar al Asistente para configuración de AD FS, incorpora el nuevo servidor de Windows Server 2016 al conjunto de AD FS existente.  En la **bienvenida** pantalla clic **siguiente**.
 ![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  En la **conectarse a servicios de dominio de Active Directory** pantalla, s**especificar una cuenta de administrador** con permisos para realizar la configuración de servicios de federación y haz clic en **siguiente**.
4.  En la **especificar granja** de pantalla, escribe el nombre de SQL server y de instancia y, a continuación, haz clic en **siguiente**.
![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  En la **especificar el certificado SSL** de pantalla, especificar el certificado y haz clic en **siguiente**.
![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  En la **especificar la cuenta de servicio** de pantalla, especificar la cuenta de servicio y haz clic en **siguiente**. 
7.  En la **Revisar opciones** de pantalla, revisa las opciones y haz clic en **siguiente**. 
8.  En la **requisitos previos comprueba** de pantalla, asegúrate de que todas las comprobaciones de requisitos previo han pasado y haz clic en **configurar**.
9.  En la **resultados** de pantalla, asegúrate de ese servidor se ha configurado correctamente y haz clic en **cerrar**.
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Quitar el servidor de AD FS de Windows Server 2012 R2

>[!NOTE]
>No es necesario configurar el servidor de AD FS principal mediante conjunto AdfsSyncProperties-rol al usar SQL como la base de datos.  Esto es porque todos los nodos se consideran principales en esta configuración.

1.  En el servidor de Windows Server 2012 R2 AD FS en uso del administrador del servidor **quitar Roles y características** en **administrar**. 
![Quitar el servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  En la **antes de comenzar** de pantalla, haz clic en **siguiente**.
3.  En la **selección de servidor** pantalla, haz clic en **siguiente**.
4.  En la **Roles de servidor** de pantalla, quita la marca de verificación junto a **los servicios de federación de Active Directory** y haz clic en **siguiente**.
![Quitar el servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  En la **características** pantalla, haz clic en **siguiente**.
6.  En la **confirmación** pantalla, haz clic en **quitar**.
7.  Una vez completado esto, reiniciar el servidor.
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentar el nivel de comportamiento de granja (FBL)
Antes de este paso, debes asegurarte de que se han ejecutado forestprep y domainprep en el entorno de Active Directory y que Active Directory tiene el esquema de Windows Server 2016.  Este documento había iniciado con un controlador de dominio de 2016 de Windows y no requiere con los siguientes porque se ejecutaron al instala AD.

1. Ahora en el servidor de Windows Server 2016, abre PowerShell y ejecuta lo siguiente: **$cred = Get-Credential** y presione ENTRAR.
2. Escribe las credenciales que tienen privilegios de administrador de SQL Server.
3. Ahora en PowerShell, escribe lo siguiente: **Invoke AdfsFarmBehaviorLevelRaise-$cred de credenciales**
2. Cuando se te pide, escribe **Y**. Esto empezará a generar el nivel.  Cuando se complete correctamente haber generado el FBL.  
![Finalizar la actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Ahora, si vas a la administración de AD FS, verás los nuevos nodos que se han agregado de AD FS en Windows Server 2016  
4. De igual modo, puedes usar la cmdlt PowerShell: Get-AdfsFarmInformation para mostrar el FBL actual.  
![Finalizar la actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
