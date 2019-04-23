---
title: Crear tu plan de recuperación ante desastres
description: Obtenga información sobre cómo crear un plan de recuperación ante desastres para la implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879506"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Crear el plan de recuperación ante desastres para RDS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede crear un plan de recuperación ante desastres en Azure Site Recovery para automatizar el proceso de conmutación por error. Agregue todas las máquinas virtuales del componente RDS al plan de recuperación.

Use los pasos siguientes en Azure para crear el plan de recuperación:

1. Abrir almacén de Azure Site Recovery en Azure portal y, a continuación, haga clic en **planes de recuperación**.
2. Haga clic en **crear** y escriba un nombre para el plan.
3. Seleccione su **origen** y **destino**. El destino es un sitio RDS secundario o Azure.
4. Seleccione las máquinas virtuales que hospedan los componentes RDS y, a continuación, haga clic en **Aceptar**.

Las secciones siguientes proporcionan información adicional acerca de cómo crear planes de recuperación para los diferentes tipos de implementación de RDS.

## <a name="sessions-based-rds-deployment"></a>Implementación basada en sesiones de RDS

Para una implementación basada en sesiones RDS, agrupar las máquinas virtuales para que aparezcan en la secuencia:

1. Grupo de conmutación por error 1: máquina virtual del Host de sesión
2. Grupo de conmutación por error 2: conexión de agente de VM
3. Grupo de conmutación por error 3: máquina virtual de Web Access

El plan será algo parecido a esto: 

![Un plan de recuperación ante desastres para una implementación de RDS basados en sesión](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Implementación de RDS de escritorios

Para una implementación de RDS con escritorios, agrupe las máquinas virtuales de modo que aparezcan en la secuencia, agregar scripts y los pasos manuales.

1. Grupo de conmutación por error 1: máquina virtual de agente de conexión de RDS
2. Grupo 1 acción manual - actualización de DNS

   Ejecute PowerShell en modo elevado en la máquina virtual de agente de conexión. Ejecute el siguiente comando y espere unos minutos para asegurarse de que el DNS se actualiza con el nuevo valor:

   ```
   ipconfig /registerdns
   ```
3. Grupo 1 script - agregar hosts de virtualización

   Modifique el script siguiente para ejecutar para cada host de virtualización en la nube. Normalmente, después de agregar un host de virtualización para un agente de conexión, deberá reiniciar el host. Asegúrese de que el host no tiene un reinicio pendiente antes de ejecutar la secuencia de comandos, o bien se producirá un error.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo de conmutación por error 2: plantilla de VM
5. Grupo 2 script 1: activar desactivar la máquina virtual de plantilla
   
   Se iniciará la plantilla de máquina virtual cuando se recuperan en el sitio secundario, pero es una máquina virtual de preparada con Sysprep y no se puede iniciar por completo. También RDS requiere que la máquina virtual esté apagado para crear una configuración agrupada de la máquina virtual a partir de él. Por lo tanto, debemos desactivarlo. Si tiene un único servidor VMM, el nombre de la máquina virtual de plantilla es el mismo en el servidor principal y la secundaria. Por este motivo, usamos el identificador de la máquina virtual según lo especificado por el *contexto* variable en el siguiente script. Si tiene varias plantillas, desactivarlas todas.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Grupo 2 script 2: quitar las máquinas virtuales agrupadas existentes

   Deberá quitar las máquinas virtuales agrupadas en el sitio primario desde el agente de conexión, por lo que se pueden crear nuevas máquinas virtuales en el sitio secundario. En este caso, deberá especificar el host exacto en que se va a crear la máquina virtual agrupada. Tenga en cuenta que esta acción eliminará las máquinas virtuales de sólo la colección.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Acción manual del grupo 2 - nueva plantilla de asignación

   Debe asignar la nueva plantilla para el agente de conexión para la colección para que pueda crear nuevas máquinas virtuales agrupadas en el sitio de recuperación. Vaya al agente de conexión de RDS e identificar la colección. Editar las propiedades y especifique una nueva imagen de máquina virtual como su plantilla.
8. Grupo 2 script 3: volver a crear máquinas virtuales agrupadas todas

   Vuelva a crear las máquinas virtuales agrupadas en el sitio de recuperación mediante el agente de conexión. En este caso, deberá especificar el host exacto en que se va a crear la máquina virtual agrupada.

   El nombre de máquina virtual agrupado debe ser único, con el prefijo y sufijo. Si ya existe el nombre de la máquina virtual, se producirá un error en la secuencia de comandos. Además, si el lado principal máquinas virtuales se numeran del 1 al 5, la numeración de sitio de recuperación continuará desde 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Grupo de conmutación por error 3: máquina virtual del servidor de puerta de enlace y de Web Access

El plan de recuperación tendrá un aspecto similar al siguiente:

![Un plan de recuperación ante desastres para una implementación de RDS con escritorios](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Implementación de RDS escritorios personales

Para una implementación de RDS con escritorios personales, agrupe las máquinas virtuales de modo que aparezcan en la secuencia, agregar scripts y los pasos manuales.

1. Grupo de conmutación por error 1: máquina virtual de agente de conexión de RDS
2. Grupo 1 acción manual - actualización de DNS

   Ejecute PowerShell en modo elevado en la máquina virtual de agente de conexión. Ejecute el siguiente comando y espere unos minutos para asegurarse de que el DNS se actualiza con el nuevo valor:

   ```
   ipconfig /registerdns
   ```
3. Grupo 1 script - agregar hosts de virtualización
      
   Modifique el script siguiente para ejecutar para cada host de virtualización en la nube. Normalmente, después de agregar un host de virtualización para un agente de conexión, deberá reiniciar el host. Asegúrese de que el host no tiene un reinicio pendiente antes de ejecutar la secuencia de comandos, o bien se producirá un error.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo de conmutación por error 2: plantilla de VM
5. Grupo 2 script 1: activar desactivar la plantilla de máquina virtual
   
   Se iniciará la plantilla de máquina virtual cuando se recuperan en el sitio secundario, pero es una máquina virtual de preparada con Sysprep y no se puede iniciar por completo. También RDS requiere que la máquina virtual esté apagado para crear una configuración agrupada de la máquina virtual a partir de él. Por lo tanto, debemos desactivarlo. Si tiene un único servidor VMM, el nombre de la máquina virtual de plantilla es el mismo en el servidor principal y la secundaria. Por este motivo, usamos el identificador de la máquina virtual según lo especificado por el *contexto* variable en el siguiente script. Si tiene varias plantillas, desactivarlas todas.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Grupo de conmutación por error 3: máquinas virtuales personales
7. Grupo 3 script 1: quitar las máquinas virtuales personales existentes y agregarlos

   Quite las máquinas virtuales personales en el sitio primario desde el agente de conexión, por lo que se pueden crear nuevas máquinas virtuales en el sitio secundario. Debe extraer las asignaciones de las máquinas virtuales y volver a agregar las máquinas virtuales para el agente de conexión con el hash de las asignaciones. Esto solo quitará las máquinas virtuales personales de la colección y volver a agregarlos. La asignación de escritorio personal se exportan y volver a importar en la colección.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Grupo de conmutación por error 3: máquina virtual del servidor de puerta de enlace y de Web Access

El plan será algo parecido a esto: 

![Un plan de recuperación ante desastres para una implementación de RDS escritorios personales](media/rds-asr-personal-desktops-drplan.png)
