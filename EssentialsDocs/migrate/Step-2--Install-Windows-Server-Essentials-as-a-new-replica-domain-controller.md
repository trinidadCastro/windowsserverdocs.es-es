---
title: "Paso 2: Instalar Windows Server Essentials como un nuevo controlador de dominio de réplica"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 757012b7d1a57a001e3b55cdc0604b63852a3d3c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Paso 2: Instalar Windows Server Essentials como un nuevo controlador de dominio de réplica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta sección describe cómo instalar Windows Server Essentials y Windows Server 2012 R2 Standard (con el rol de Windows Server Essentials Experience habilitado) como un controlador de dominio.  
  
 Para entornos con usuarios de hasta 25 y 50 dispositivos, puedes seguir los pasos descritos en esta guía para migrar de versiones anteriores de Windows SBS a Windows Server Essentials. Para entornos con hasta 100 usuarios y dispositivos de 200, puedes seguir la misma guía para migrar a las ediciones Standard y Datacenter de Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado. Dos escenarios se tratan en esta documentación.  
  
> [!IMPORTANT]
>  Si se migran a Windows Server Essentials, se agrega el siguiente mensaje de error al registro de eventos cada día durante el período de gracia de 21 días hasta que se elimina el servidor de origen de la red. Después del período de gracia 21 días, se cerrará el servidor de origen. <br> **La comprobación de la función FSMO detecta una condición en tu entorno que está fuera del cumplimiento de la directiva de licencia. El servidor de administración debe mantener el controlador de dominio principal y los roles de maestro de Active Directory de nombres de dominio. Mueva los roles de Active Directory al servidor de administración ahora. Este servidor se puede cerrar automáticamente si no se ha corregido el problema en 21 días desde el momento en que se detectó por primera vez esta condición**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Instalar Windows Server Essentials o Windows Server 2012 R2 Standard en el servidor de destino  
  
1.  Instalar Windows Server Essentials o Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado siguiendo las instrucciones de [instalar y configurar Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Si se inicia el Asistente de configurar Windows Server Essentials, cancelarla.  
  
2.  Transferir los FSMO desde el servidor de origen.  
  
    > [!NOTE]
    >  Si Windows Server Essentials es el único controlador de dominio del dominio, la función FSMO se mueve automáticamente al servidor que ejecuta Windows Server Essentials cuando degradar el servidor de origen.  
  
3.  Abre el administrador del servidor y ejecutar al Asistente para agregar Roles y características.  
  
4.  Si no está instalado, agrega el rol de Windows Server Essentials Experience.  
  
5.  Después de instalar el rol de Windows Server Essentials Experience, la tarea de configurar Windows Server Essentials aparece en el área de notificación. Haz clic en la tarea para iniciar al Asistente para configurar Windows Server Essentials.  
  
6.  Sigue las instrucciones para completar la configuración de Windows Server Essentials. Antes de ejecutar al asistente, haz lo siguiente:  
  
    -   Cambiar el nombre del servidor si es necesario, porque no puedes cambiar el nombre después de completar al Asistente de configurar Windows Server Essentials.  
  
    -   Asegúrate de que el servidor tiempo y opciones de configuración sean correctas.  
  
7.  Comprobar la instalación de la siguiente manera:  
  
    1.  Abra el panel.  
  
    2.  Haz clic en **usuarios** pestaña y comprueba que se muestran las cuentas de usuario en Active Directory.  
  
### <a name="transfer-the-operations-master-roles"></a>Los roles de maestro de operaciones de transferencia  
 Los roles de maestro (también denominado operaciones de maestro único flexibles o FSMO) de las operaciones se deben transferir desde el servidor de origen al servidor de destino en días 21 de instalación de Windows Server Essentials en el servidor de destino.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Para transferir los roles de maestro de operaciones  
  
1.  En el servidor de destino, abre una ventana de símbolo del sistema como administrador. Consulta [para abrir una ventana de símbolo del sistema como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  En el símbolo del sistema, escribe **NETDOM QUERY FSMO**, y, a continuación, presione ENTRAR.  
  
3.  En el símbolo del sistema, escribe **ntdsutil**, y, a continuación, presione ENTRAR.  
  
4.  En la **ntdsutil** símbolo del sistema, escribe los siguientes comandos:  
  
    1.  Tipo **activar instancia NTDS**, y, a continuación, presione ENTRAR.  
  
    2.  Tipo **roles**, y, a continuación, presione ENTRAR.  
  
    3.  Tipo **conexiones**, y, a continuación, presione ENTRAR.  
  
    4.  Tipo **conectarse al servidor** *< ServerName\ >* (donde *< ServerName\ >* es el nombre del servidor de destino), y, a continuación, presione ENTRAR.  
  
    5.  En el símbolo del sistema, escribe **q**, y, a continuación, presione ENTRAR.  
  
        1.  Tipo **transferir PDC**, presione ENTRAR y, a continuación, haz clic en **Sí** en la **confirmación de transferencia de función** cuadro de diálogo.  
  
        2.  Tipo **maestro de infraestructura de transferencia**, presione ENTRAR y, a continuación, haz clic en **Sí** en la **confirmación de transferencia de función** cuadro de diálogo.  
  
        3.  Tipo **transferir el patrón de nomenclatura**, presione ENTRAR y, a continuación, haz clic en **Sí** en la **confirmación de transferencia de función** cuadro de diálogo.  
  
        4.  Tipo **maestro de transferencia de RID**, presione ENTRAR y, a continuación, haz clic en **Sí** en la **confirmación de transferencia de función** cuadro de diálogo.  
  
        5.  Tipo **maestro de esquema de transferencia**, presione ENTRAR y, a continuación, haz clic en **Sí** en la **confirmación de transferencia de función** cuadro de diálogo.  
  
    6.  Tipo **q**, y, a continuación, presione ENTRAR para regresar a la ventana de símbolo del sistema.  
  
> [!NOTE]
>  Desde cualquier servidor en la red, puedes comprobar que se ha transferido los roles de maestro de operaciones al servidor de destino. Abre una ventana de símbolo del sistema como administrador (para obtener más información, consulta [para abrir una ventana de símbolo del sistema como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Tipo **netdom consulta fsmo**, y, a continuación, presione ENTRAR.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Se han instalado Windows Server Essentials como un nuevo controlador de dominio de réplica. Ahora ve a [paso 3: unirte a equipos con el servidor de Windows Server Essentials nuevo](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

