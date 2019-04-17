---
title: Degradar y quitar el servidor de origen de la nueva red1 de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Degradar y quitar el servidor de origen de la nueva red1 de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Una vez finalizada la instalación de Windows Server Essentials y completar las tareas en el Asistente para la migración, debes realizar las siguientes tareas:  
  

1.  [Desinstalar Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconecte impresoras que están conectadas directamente al servidor de origen](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Disminuir el servidor de origen](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover el rol de servidor DHCP desde el servidor de origen al enrutador](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Quitar y reutilizar el servidor de origen](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Desinstalar Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconecte impresoras que están conectadas directamente al servidor de origen](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Disminuir el servidor de origen](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover el rol de servidor DHCP desde el servidor de origen al enrutador](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Quitar y reutilizar el servidor de origen](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>Desinstalar Exchange Server 2003  
  
> [!IMPORTANT]
>  Si agrega cuentas de usuario después de mover buzones al servidor de destino y antes de desinstalar Exchange Server 2003 desde el servidor de origen, se agregan los buzones en el servidor de origen. Esto es por diseño. Debes mover los buzones al servidor de destino para todas las cuentas de usuario que se agregan durante este tiempo. Repita las instrucciones de buzones de Exchange Server de mover y opciones de configuración para la migración de Windows Server Essentials antes de desinstalar Exchange Server 2003.  
  
 Antes de degradar debe desinstalar Exchange Server 2003 desde el servidor de origen. Esto elimina todas las referencias en servicios de dominio de Active Directory (AD DS) para Exchange Server en el servidor de origen. Debes tener los medios de Windows Small Business Server 2003 quitar Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Para desinstalar Exchange Server 2003 desde el servidor de origen  
  
1.  Iniciar sesión en el servidor de origen como administrador  
  
2.  Haz clic en **inicio**, haz clic en **Panel de Control**y, a continuación, haz clic en **agregar o quitar programas**.  
  
3.  En la lista de programas, selecciona **Windows Small Business Server 2003**y, a continuación, haz clic en **cambiar o quitar**.  
  
4.  En el Asistente para la instalación, haz clic en **siguiente** hasta que el **selección de componentes** aparecerá la página.  
  
5.  En la página Selección de componentes, expanda **Exchange Server**y, a continuación, elige **quitar**.  
  
    > [!NOTE]

    >  Exchange Server comprueba que no existen buzones o las carpetas públicas en el servidor. Si los datos permanecen, un mensaje de error aparece al hacer clic **quitar**. Para evitar este problema, asegúrate de que has completado todos los procedimientos en el tema [mover SBS 2003 configuración y los datos al servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

    >  Exchange Server comprueba que no existen buzones o las carpetas públicas en el servidor. Si los datos permanecen, un mensaje de error aparece al hacer clic **quitar**. Para evitar este problema, asegúrate de que has completado todos los procedimientos en el tema [mover SBS 2003 configuración y los datos al servidor de destino](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6.  Haz clic en **siguiente**.  
  
7.  Cuando se te solicite, inserta Windows Small Business Server 2003 CD #3 y sigue las instrucciones en pantalla.  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>Desconecte impresoras que están conectadas directamente al servidor de origen  
 Antes de degradar al servidor de origen, desconecte físicamente las impresoras que están conectadas directamente al servidor de origen y que se comparten a través del servidor de origen. No garantizar que ningún objeto de Active Directory para las impresoras que se han conectado directamente al servidor de origen. Las impresoras después puede directamente conectadas al servidor de destino y compartidas desde Windows Server Essentials.  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>Disminuir al servidor de origen  
 Antes de degradar al servidor de origen de la función de controlador de dominio de AD DS para el rol de servidor miembro del dominio, asegúrate de que la configuración de directiva de grupo se aplica a todos los equipos cliente, como se describe en el siguiente procedimiento.  
  
> [!IMPORTANT]
>  El servidor de origen y el servidor de destino deben estar conectados a la red mientras se actualizan los cambios de directiva de grupo en los equipos cliente.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forzar una actualización de directiva de grupo en un equipo cliente  
  
1.  Iniciar sesión como administrador el equipo cliente.  
  
2.  Abre una ventana de símbolo del sistema como administrador.  
  
3.  En el símbolo del sistema, escribe **comando gpupdate/force**, y, a continuación, presione ENTRAR.  
  
4.  El proceso puede requerir que se cierra la sesión y volver a iniciarla para finalizar. Haz clic en **Sí** para confirmar.  
  
##### <a name="to-demote-the-source-server"></a>Para disminuir al servidor de origen  
  
1.  En el servidor de origen, haz clic en **inicio**, haz clic en **ejecutar**, tipo **dcpromo**y, a continuación, haz clic en **Aceptar**.  
  
2.  Haz clic en **siguiente** dos veces.  
  
    > [!NOTE]
    >  No selecciones **este servidor es el último controlador de dominio en el dominio**.  
  
3.  Escribe una contraseña para la nueva cuenta de administrador en el servidor y, a continuación, haz clic en **siguiente**.  
  
4.  En la **resumen** cuadro de diálogo, se le informa de que se quitarán del equipo de AD DS y que el servidor se convertirá en un miembro del dominio. Haz clic en **siguiente**.  
  
5.  Haz clic en **finalizar**. Reinicia el servidor de origen.  
  
6.  Después de reiniciar el servidor de origen, agrega el servidor de origen como un miembro de un grupo de trabajo antes de desconectar desde la red.  
  
 Después de agregar el servidor de origen como un miembro de un grupo de trabajo y se desconecta de la red, debes quitar de AD DS en el servidor de destino.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Para quitar el servidor de origen de Active Directory  
  
1.  En el servidor de destino, abre **equipos y usuarios de Active Directory**.  
  
2.  En la **equipos y usuarios de Active Directory** panel de navegación, expande el nombre de dominio y, a continuación, expanda **equipos**.  
  
3.  Haga clic con el nombre del servidor de origen si aún existe en la lista de servidores, **eliminar**y, a continuación, haz clic en **Sí**.  
  
4.  Comprueba que el servidor de origen no esté lista y, a continuación, cerrar **equipos y usuarios de Active Directory**.  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>Mover el rol de servidor DHCP desde el servidor de origen al enrutador  
  
> [!NOTE]

>  Si ya ha realizado esta tarea antes de iniciar el proceso de migración, continuar con la sección [quitar y reutilizar el servidor de origen](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

>  Si ya ha realizado esta tarea antes de iniciar el proceso de migración, continuar con la sección [quitar y reutilizar el servidor de origen](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Si tu servidor de origen está ejecutando la función de DHCP, realiza los siguientes pasos para mover el rol DHCP al enrutador.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover el rol DHCP desde el servidor de origen al enrutador  
  
1.  Desactivar el servicio DHCP en el servidor de origen, como sigue:  
  
    1.  En el servidor de origen, haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **servicios**.  
  
    2.  En la lista de servicios en ejecución actualmente, haz clic en el **Windows Server**y, a continuación, haz clic en **propiedades**.  
  
    3.  Para **tipo de inicio**, selecciona **deshabilitado**.  
  
    4.  Detener el servicio.  
  
2.  Activar la función de DHCP en el enrutador  
  
    1.  Sigue las instrucciones de la documentación del enrutador para activar la función de DHCP en el enrutador.  
  
    2.  Para garantizar que las direcciones IP emitidas por el servidor de origen sea el mismo, sigue las instrucciones de la documentación del enrutador para configurar el intervalo de DHCP en el enrutador para ser el mismo que el rango DHCP en el servidor de origen.  
  
    > [!IMPORTANT]
    >  Si no has configurado un reservas de IP o DHCP estática en el enrutador para el servidor de destino y el intervalo DHCP no es el mismo que el servidor de origen, es posible que el enrutador emitirá una nueva dirección IP de servidor de destino. Si esto ocurre, restablecer las reglas del enrutador para reenviar a la nueva dirección IP del servidor de destino de enrutamiento de puertos.  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>Quitar y reutilizar el servidor de origen  
 Desactivar el servidor de origen y se desconecta de la red. Te recomendamos que no formatea el servidor de origen para al menos una semana para asegurarte de que se migren todos los datos necesarios para el servidor de destino. Después de comprobar que todos los datos se ha migrado, puede reinstalar este servidor en la red como un servidor secundario para otras tareas, si es necesario.  
  
> [!NOTE]
>  Después de degradar y quitar el servidor de origen, reinicia el servidor de destino.  
  
 Después de disminuir al servidor de origen, no está en buen estado. Si quieres reutilizar el servidor de origen, la forma más sencilla es cambiar su formato, instalar un sistema operativo de servidor y, a continuación, configurado para usarlo como un servidor adicional.
