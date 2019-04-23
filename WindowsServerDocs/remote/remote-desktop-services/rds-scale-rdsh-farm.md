---
title: Escalar horizontalmente la implementación de RDS mediante la adición de una granja de servidores Host de sesión de escritorio remoto
description: Agregar a un segundo Host de sesión de escritorio remoto a su entorno de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: d243994a68c0bf4f0584f68475a185acb9cb73d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865496"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Escalar horizontalmente la implementación de servicios de escritorio remoto mediante la adición de una granja de servidores Host de sesión de escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede mejorar la disponibilidad y escalabilidad de la implementación de RDS mediante la adición de una granja de servidores Host de sesión de escritorio remoto (RDSH).   
  
 
Use los pasos siguientes para agregar otro Host de sesión de escritorio remoto a la implementación:  
  
1. Crear un servidor para hospedar al segundo Host de sesión de escritorio remoto. Si usa máquinas virtuales de Azure, asegúrese de incluir la nueva máquina virtual en el mismo conjunto de disponibilidad que contiene al primer Host de sesión de escritorio remoto.
2. Habilitar la administración remota en el nuevo servidor o máquina virtual:
   1. En el administrador del servidor, haga clic en **servidor Local > configuración actual de administración remota (deshabilitado)**. 
   2. Seleccione **habilitar la administración remota para este servidor**y, a continuación, haga clic en **Aceptar**. 
   3. Opcional: Windows Update para descargar e instalar actualizaciones automáticamente no se puede establecer temporalmente. Esto ayuda a evitar que los cambios y los reinicios del sistema mientras se implementa el servidor RDSH. En el administrador del servidor, haga clic en **servidor Local > configuración actual de Windows Update**. Haga clic en **opciones avanzadas > aplazar actualizaciones**. 
3. Agregue el servidor o máquina virtual al dominio:
   1. En el administrador del servidor, haga clic en **servidor Local > configuración de grupo de trabajo actual**. 
   2. Haga clic en **cambio > dominio**y, a continuación, escriba el nombre de dominio (por ejemplo, Contoso.com). 
   3. Escriba las credenciales de administrador de dominio. 
   4. Reinicie el servidor o máquina virtual.
4. Agregue al Host de sesión de escritorio remoto nuevo a la granja de servidores:
>[!NOTE] 
> Paso 1: creación de una dirección IP pública para la máquina virtual RDMS, solo es necesario si está usando una máquina virtual para el RDMS y si aún no tiene una dirección IP asignada.
   
   1. Cree una dirección IP pública para la máquina virtual ejecuta Servicios de administración de escritorio remoto (RDMS). La máquina virtual RDMS normalmente será la máquina virtual ejecuta la primera instancia del rol de agente de conexión a Escritorio remoto.  
       1. En el portal de Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en la máquina virtual RDMS (por ejemplo, Contoso-Cb1).  
       2. Haga clic en **configuración > interfaces de red**y, a continuación, haga clic en la interfaz de red correspondiente.   
       3. Haga clic en **configuración > dirección IP**.
       4. Para **dirección IP pública**, seleccione **habilitado**y, a continuación, haga clic en **dirección IP**.   
       5. Si tiene una dirección IP pública existente que desea usar, selecciónelo en la lista. En caso contrario, haga clic en **crear nuevo**, escriba un nombre y, a continuación, haga clic en **Aceptar** y, a continuación, **guardar**.   
   2. Inicio de sesión en el RDMS.
   3. Agregue un nuevo servidor RDSH al administrador del servidor:   
       1. Inicie el administrador del servidor, haga clic en **administrar > Agregar servidores**.   
       2. En el cuadro de diálogo Agregar servidores, haga clic en **Buscar ahora**.   
       3. Seleccione el servidor que desea usar para el Host de sesión de escritorio remoto o la máquina virtual recién creada (por ejemplo, Contoso-Sh2) y haga clic en **Aceptar**.
   4. Adición del servidor RDSH a la implementación
       1. Launch Server Manager .  
       2. Haga clic en **servicios de escritorio remoto > información general > servidores de implementación > tareas > Agregar servidores de Host de sesión de escritorio remoto**.   
       3. Seleccione el nuevo servidor (por ejemplo, Contoso-Sh2) y, a continuación, haga clic en **siguiente**.  
       4. En la página de confirmación, seleccione **reiniciar los equipos remotos según sea necesario**y, a continuación, haga clic en **agregar**.   
   5. Agregar servidor RDSH a la granja de servidores de recopilación:
       1. Inicia el Administrador del servidor.   
       2. Haga clic en **servicios de escritorio remoto** y, a continuación, haga clic en la colección a la que desea agregar el servidor RDSH recién creado (por ejemplo, ContosoDesktop).   
       3. En **servidores Host**, haga clic en **tareas > Agregar servidores de Host de sesión de escritorio remoto**.   
       4. Seleccione el servidor recién creado (por ejemplo, Contoso-Sh2) y, a continuación, haga clic en **siguiente**.   
       5. En la página de confirmación, haga clic en **agregar**.   

