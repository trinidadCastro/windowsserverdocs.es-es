---
title: 'Paso 6: Degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials'
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Paso 6: Degradar y quitar el servidor de origen de la nueva red de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Una vez finalizada la instalación de Windows Server Essentials y completar la migración, debes realizar las siguientes tareas:  
  
1.  [Quitar los servicios de certificados de Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Desconecte impresoras que están conectadas directamente al servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Disminuir al servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Quitar y reutilizar el servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>Quitar los servicios de certificados de Active Directory  
 El procedimiento es ligeramente diferente si tienes varios servicios de rol de servicios de certificados de Active Directory (AD CS) instalados en un solo servidor. Puedes usar el siguiente procedimiento para desinstalar un servicio de rol de AD CS y para conservar otros servicios de rol de AD CS.  
  
 Para completar este procedimiento, debe iniciar sesión con los mismos permisos que el usuario que instaló la entidad de certificación (CA). Si va a desinstalar una CA de empresa, pertenencia al grupo Administradores de empresa o su equivalente es lo mínimo necesario para completar este procedimiento.  
  
#### <a name="to-remove-ad-cs"></a>Para quitar los AD CS  
  
1.  Inicie sesión en el servidor de origen como un administrador de dominio.  
  
2.  Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **administrador del servidor**.  
  
3.  Haz clic en **continuar** en la **Control de cuentas de usuario** cuadro de diálogo.  
  
4.  En la **resumen de Roles** sección, haz clic en **quitar Roles**.  
  
5.  En el Asistente para quitar Roles, haz clic en **siguiente**.  
  
6.  Borrar la **servicios de certificados de Active Directory** y, a continuación, haz clic en **siguiente**.  
  
7.  En la **Confirmar eliminación opciones** página, revisa la información y, a continuación, haz clic en **quitar**.  
  
    > [!NOTE]
    >  Si se ejecuta Internet Information Services (IIS), deberá detener el servicio antes de continuar. Haz clic en **Aceptar**.  
  
    > [!NOTE]
    >  En primer lugar, tienes que quitar **inscripción de Web de entidad de certificación**, si está instalado.  
  
8.  Cuando termine el Asistente para quitar Roles, reiniciar el servidor para completar el proceso de desinstalación.  
  
    > [!IMPORTANT]
    >  Reiniciar el servidor, incluso si no se te pide que lo haga.  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>Desconecte impresoras que están conectadas directamente al servidor de origen  
 Antes de degradar al servidor de origen, desconecte físicamente las impresoras que están conectadas directamente al servidor de origen y que se comparten a través del servidor de origen. No garantizar que ningún objeto de Active Directory para las impresoras que se han conectado directamente al servidor de origen. Las impresoras después puede directamente conectadas al servidor de destino y compartidas desde Windows Server Essentials.  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>Disminuir al servidor de origen  
 Antes de degradar al servidor de origen de la función de controlador de dominio de AD DS para el rol de servidor miembro del dominio, asegúrate de que la configuración de directiva de grupo se aplica a todos los equipos cliente, como se describe en el siguiente procedimiento.  
  
> [!IMPORTANT]
>  El servidor de origen y el servidor de destino deben estar conectados a la red mientras se actualizan los cambios de directiva de grupo en los equipos cliente.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forzar una actualización de directiva de grupo en un equipo cliente  
  
1.  Inicia sesión como administrador en el equipo cliente.  
  
2.  Abre una ventana de símbolo del sistema como administrador.  
  
3.  En el símbolo del sistema, escribe **comando gpupdate/force**, y, a continuación, presione ENTRAR.  
  
4.  El proceso puede requerir que se cierra la sesión y volver a iniciarla para finalizar. Haz clic en **Sí** para confirmar.  
  
 Si vas a migrar de Windows Server Essentials o sus versiones anteriores, para disminuir el servidor, consulta [quitar Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Después de agregar el servidor de origen como un miembro de un grupo de trabajo y se desconecta de la red, debes quitar de AD DS en el servidor de destino.  
  
 Si vas a migrar de Windows Server Essentials, usa el administrador del servidor para quitar la función de los servicios de dominio de Active Directory, lo que degradar el controlador de dominio en el servidor de origen mediante el siguiente procedimiento:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Para quitar el servidor de origen de Active Directory  
  
1.  En el servidor de destino, abre **equipos y usuarios de Active Directory**.  
  
2.  En la **equipos y usuarios de Active Directory** panel de navegación, expande el nombre de dominio y, a continuación, expanda **equipos**.  
  
3.  Si el servidor de origen permanece en la lista de servidores, haz clic en el nombre del servidor de origen, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
4.  Comprueba que el servidor de origen no esté lista y, a continuación, cerrar **equipos y usuarios de Active Directory**.  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>Quitar y reutilizar el servidor de origen  
 Desactivar el servidor de origen y se desconecta de la red. Te recomendamos que no formatea el servidor de origen para al menos una semana para asegurarte de que se migren todos los datos necesarios para el servidor de destino. Después de comprobar que todos los datos se ha migrado, puede reinstalar este servidor en la red como un servidor secundario para otras tareas, si es necesario.  
  
> [!NOTE]
>  Después de degradar y quitar el servidor de origen, reinicia el servidor de destino.  
  
 Después de disminuir al servidor de origen, no está en buen estado. Si quieres reutilizar el servidor de origen, la forma más sencilla es cambiar su formato, instalar un sistema operativo de servidor y, a continuación, configurado para usarlo como un servidor adicional.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Tienes degrada y quita el servidor de origen de la nueva red de Windows Server Essentials. Ahora ve a [paso 7: realizar tareas posteriores a la migración de la migración de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

