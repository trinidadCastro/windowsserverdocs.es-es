---
title: 'Paso 6: Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials'
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3df8b2901ca2ca3d38066c592aaeb326d7471ba0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318734"
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Paso 6: Disminuir de nivel y quitar el servidor de origen de la nueva red de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Después de finalizar la instalación de Windows Server Essentials y completar la migración, debe realizar las siguientes tareas:  
  
1.  [Quitar Active Directory servicios de Certificate Server](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Desconectar las impresoras conectadas directamente al servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Disminuir de nivel el servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Quitar y reasignar el servidor de origen](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="remove-active-directory-certificate-services"></a><a name="BKMK_ADCS"></a>Quitar Active Directory servicios de Certificate Server  
 El procedimiento es ligeramente distinto si tiene varios servicios de rol de servicios de certificados de Active Directory (AD CS) instalados en un solo servidor. Con el procedimiento siguiente puede desinstalar un servicio de rol de AD CS y conservar los demás.  
  
 Para completar este procedimiento, inicie sesión con los mismos permisos que el usuario que ha instalado la entidad de certificación (CA). Para desinstalar una CA empresarial, el requisito mínimo es pertenecer al grupo Administradores de organización o equivalente.  
  
#### <a name="to-remove-ad-cs"></a>Para quitar AD CS  
  
1.  Inicie sesión como administrador de dominio en el servidor de origen.  
  
2.  Haga clic en **Inicio**, **Herramientas administrativas** y **Administrador del servidor**.  
  
3.  Haga clic en **Continuar**, en el cuadro de diálogo **Control de cuentas de usuario**.  
  
4.  En la sección **Resumen de roles**, haga clic en **Quitar roles**.  
  
5.  En el Asistente para quitar roles, haga clic en **Siguiente**.  
  
6.  Desactive la casilla **Servicios de certificados de Active Directory** y haga clic en **Siguiente**.  
  
7.  En la página **Confirmar opciones de eliminación**, revise la información y haga clic en **Quitar**.  
  
    > [!NOTE]
    >  Si ejecuta Internet Information Services (IIS), se le solicitará que detenga el servicio antes de continuar. Haga clic en **Aceptar**.  
  
    > [!NOTE]
    >  En primer lugar, deberá quitar la **Inscripción web de entidad de certificación**, si está instalada.  
  
8.  Cuando finalice el Asistente para quitar roles, reinicie el servidor para completar el proceso de desinstalación.  
  
    > [!IMPORTANT]
    >  Reinicie el servidor aunque no se le solicite.  
  
##  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a>Desconectar las impresoras conectadas directamente al servidor de origen  
 Antes de disminuir de nivel el servidor de origen, desconecte físicamente las impresoras que están conectadas directamente a este y se comparten a través de él. Asegúrese de que no haya ningún objeto de Active Directory en las impresoras que estaban conectadas directamente al servidor de origen. Las impresoras pueden conectarse directamente al servidor de destino y compartirse con Windows Server Essentials.  
  
##  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a>Disminuir de nivel el servidor de origen  
 Antes de disminuir de nivel el servidor de origen (desde el rol del controlador de dominio de AD DS hasta el rol de un servidor miembro de dominio), asegúrese de que la configuración de directiva de grupo se aplica a todos los equipos cliente, tal como se describe en el procedimiento siguiente.  
  
> [!IMPORTANT]
>  Los servidores de origen y destino deben estar conectados a la red mientras se actualizan los cambios de la directiva de grupo en los equipos cliente.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forzar una actualización de directiva de grupo en un equipo cliente  
  
1. Inicie sesión en el equipo cliente como administrador.  
  
2. Abra una ventana del símbolo del sistema como administrador.  
  
3. En el símbolo del sistema, escriba **gpupdate /force** y, después, presione ENTRAR.  
  
4. El proceso puede requerir que cierre la sesión y vuelva a iniciarla para finalizar. Haga clic en **Sí** para confirmar.  
  
   Si va a migrar desde Windows Server Essentials o desde sus versiones anteriores, para disminuir de nivel el servidor, consulte [quitar Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Después de agregar el servidor de origen como miembro de un grupo de trabajo y desconectarlo de la red, debe quitarlo de AD DS en el servidor de destino.  
  
   Si va a migrar desde Windows Server Essentials, use Administrador del servidor para quitar el rol de Active Directory Domain Services y, por lo tanto, degradar el controlador de dominio en el servidor de origen mediante el siguiente procedimiento:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Para quitar el servidor de origen de Active Directory  
  
1.  En el servidor de destino, abra **Equipos y usuarios de Active Directory**.  
  
2.  En el panel de navegación **Usuarios y equipos de Active Directory** , expanda el nombre de dominio y **Equipos**.  
  
3.  Si el servidor de origen todavía existe en la lista de servidores, haga clic con el botón secundario en su nombre y, con el botón primario, en **Eliminar** y **Sí**.  
  
4.  Compruebe que el servidor de origen no aparece en la lista y cierre **Usuarios y equipos de Active Directory**.  
  
##  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a>Quitar y reasignar el servidor de origen  
 Desactive el servidor de origen y desconéctelo de la red. Se recomienda no volver a formatear el servidor de origen durante al menos una semana para asegurar que todos los datos necesarios se migren al servidor de destino. Después de comprobar que ha migrado todos los datos, puede reinstalar este servidor en la red como servidor secundario para otras tareas si es necesario.  
  
> [!NOTE]
>  Después de disminuir de nivel y quitar el servidor de origen, reinicie el servidor de destino.  
  
 Cuando se disminuye de nivel, el servidor de origen no se queda en el estado correcto. Si desea reasignar el servidor de origen, la forma más sencilla es volver a formatearlo, instalar un sistema operativo del servidor y configurarlo como servidor adicional.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ha degradado y quitado el servidor de origen de la nueva red de Windows Server Essentials. Ahora, vaya al [paso 7: realizar tareas posteriores a la migración para la migración a Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Para ver todos los pasos, vea [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

