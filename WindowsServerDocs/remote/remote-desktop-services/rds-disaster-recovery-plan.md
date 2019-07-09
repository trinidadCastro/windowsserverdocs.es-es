---
title: Crear un plan de recuperación ante desastres
description: Obtén información sobre cómo crear un plan de recuperación ante desastres para su implementación de RDS.
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
ms.openlocfilehash: e7bfe19258662a8e334ea0476689d8e860bfc8e5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743881"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Crear un plan de recuperación ante desastres para RDS

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Puedes crear un plan de recuperación ante desastres en Azure Site Recovery para automatizar el proceso de conmutación por error. Agrega todas las VM con componentes de RDS al plan de recuperación.

Sigue los pasos a continuación en Azure para crear el plan de recuperación:

1. Abre Azure Site Recovery Vault en Azure Portal y, luego, haz clic en **Planes de recuperación**.
2. Haz clic en **Crear** y escribe un nombre para el plan.
3. Selecciona tu **Origen** y **Destino**. El destino es un sitio de RDS secundario o Azure.
4. Selecciona las VM que hospedan los componentes de RDS y, luego, haz clic en **Aceptar**.

En las secciones siguientes se proporciona información adicional sobre cómo crear planes de recuperación para los distintos tipos de implementación de RDS.

## <a name="sessions-based-rds-deployment"></a>Implementación de RDS basada en sesiones

Para una implementación de RDS basada en sesiones, agrupa las VM para que aparezcan en secuencia:

1. Grupo 1 de conmutación por error: VM del host de sesión
2. Grupo 2 de conmutación por error: VM del Agente de conexión
3. Grupo 3 de conmutación por error: VM de acceso web

Tu plan tendrá un aspecto similar a este: 

![Un plan de recuperación ante desastres para una implementación de RDS basada en sesiones](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Implementación de RDS de escritorios agrupados

Para una implementación de RDS con escritorios agrupados, agrupa las VM de modo que aparezcan en secuencia, agregando pasos manuales y scripts.

1. Grupo 1 de conmutación por error: VM del Agente de conexión de RDS
2. Acción manual del grupo 1: actualiza DNS

   Ejecuta PowerShell en modo elevado en la VM del Agente de conexión. Ejecuta el siguiente comando y espera unos minutos para asegurarte de que el DNS se actualice con el nuevo valor:

   ```
   ipconfig /registerdns
   ```
3. Script del grupo 1: agrega hosts de virtualización

   Modifica el script siguiente para ejecutarlo para cada host de virtualización en la nube. Normalmente, después de agregar un host de virtualización a un Agente de conexión, deberás reiniciar el host. Asegúrate de que el host no tenga un reinicio pendiente antes de ejecutar el script o, de lo contrario, se producirá un error.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo 2 de conmutación por error: VM de plantillas
5. Script 1 del grupo 2: desactiva la VM de plantillas
   
   La VM de plantillas, cuando se recupere en el sitio secundario, se iniciará, pero es una VM de preparada con Sysprep y no se puede iniciar por completo. Además, RDS requiere apagar la VM para crear una configuración de VM agrupadas a partir de ella. Por lo tanto, debemos desactivarla. Si tienes un único servidor VMM, el nombre de la VM de plantillas es el mismo en el principal y el secundario. Por este motivo, usamos el id. de VM según lo especificado por la variable *Context* en el siguiente script. Si tienes varias plantillas, desactívalas todas.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Script 2 del grupo 2: quita las VM agrupadas existentes

   Deberás quitar del Agente de conexión las VM agrupadas en el sitio principal, de modo que las nuevas VM se puedan crear en el sitio secundario. En este caso, deberás especificar el host exacto en el que se van a crear las VM agrupadas. Ten en cuenta que esta acción eliminará las VM solo de la colección.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Acción manual del grupo 2: asigna una nueva plantilla

   Debes asignar la nueva plantilla al Agente de conexión para la colección, de modo que puedas crear nuevas VM agrupadas en el sitio de recuperación. Ve al Agente de conexión de RDS e identifica la colección. Edita las propiedades y especifica una nueva imagen de VM como plantilla.
8. Script 3 del grupo 2: vuelve a crear todas las VM agrupadas

   Vuelve a crear las VM agrupadas en el sitio de recuperación mediante el Agente de conexión. En este caso, deberás especificar el host exacto en el que se van a crear las VM agrupadas.

   El nombre de la VM agrupada debe ser único, con el prefijo y sufijo. Si ya existe el nombre de la VM, se producirá un error en el script. Además, si las VM del lado principal se numeran del 1 al 5, la numeración del sitio de recuperación continuará desde el 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Grupo 3 de conmutación por error: VM de acceso web y servidor de puerta de enlace

El plan de recuperación tendrá un aspecto similar al siguiente:

![Un plan de recuperación ante desastres para una implementación de RDS con escritorios agrupados](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Implementación de RDS de escritorios personales

Para una implementación de RDS con escritorios personales, agrupa las VM de modo que aparezcan en secuencia, agregando pasos manuales y scripts.

1. Grupo 1 de conmutación por error: VM del Agente de conexión de RDS
2. Acción manual del grupo 1: actualiza DNS

   Ejecuta PowerShell en modo elevado en la VM del Agente de conexión. Ejecuta el siguiente comando y espera unos minutos para asegurarte de que el DNS se actualice con el nuevo valor:

   ```
   ipconfig /registerdns
   ```
3. Script del grupo 1: agregue hosts de virtualización
      
   Modifica el script siguiente para ejecutarlo para cada host de virtualización en la nube. Normalmente, después de agregar un host de virtualización a un Agente de conexión, deberás reiniciar el host. Asegúrate de que el host no tenga un reinicio pendiente antes de ejecutar el script o, de lo contrario, se producirá un error.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo 2 de conmutación por error: VM de plantillas
5. Script 1 del grupo 2: desactiva la VM de plantillas
   
   La VM de plantillas, cuando se recupere en el sitio secundario, se iniciará, pero es una VM de preparada con Sysprep y no se puede iniciar por completo. Además, RDS requiere apagar la VM para crear una configuración de VM agrupadas a partir de ella. Por lo tanto, debemos desactivarla. Si tienes un único servidor VMM, el nombre de la VM de plantillas es el mismo en el principal y el secundario. Por este motivo, usamos el id. de VM según lo especificado por la variable *Context* en el siguiente script. Si tienes varias plantillas, desactívalas todas.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Grupo 3 de conmutación por error: VM personales
7. Script 1 del grupo 3: quita las VM personales existentes y agréguelas

   Quita del Agente de conexión las VM personales en el sitio principal, de modo que las nuevas VM se puedan crear en el sitio secundario. Debes extraer las asignaciones de las VM y volver a agregar las máquinas virtuales al Agente de conexión con el hash de las asignaciones. Esto solo quitará las VM personales de la colección y las volverá a agregar. La asignación de escritorios personales se exportará y se volverá a importar en la colección.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Grupo 3 de conmutación por error: VM de acceso web y servidor de puerta de enlace

Tu plan tendrá un aspecto similar a este: 

![Un plan de recuperación ante desastres para una implementación de RDS con escritorios personales](media/rds-asr-personal-desktops-drplan.png)
