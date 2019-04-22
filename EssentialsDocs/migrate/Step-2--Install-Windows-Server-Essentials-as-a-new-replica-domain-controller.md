---
title: 'Paso 2: Instalar Windows Server Essentials como nuevo controlador de dominio de réplica'
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816466"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Paso 2: Instalar Windows Server Essentials como nuevo controlador de dominio de réplica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En esta sección se describe cómo instalar Windows Server Essentials y Windows Server 2012 R2 Standard (con el rol de experiencia con Windows Server Essentials habilitado) como un controlador de dominio.  
  
 Para entornos con hasta 25 usuarios y 50 dispositivos, puede seguir los pasos descritos en esta guía para migrar desde versiones anteriores de Windows SBS a Windows Server Essentials. Para entornos con hasta 100 usuarios y 200 dispositivos, puede seguir las mismas instrucciones para migrar a las ediciones Standard y Datacenter de Windows Server 2012 R2 con el rol experiencia con Windows Server Essentials instalado. Ambos escenarios se tratan en esta documentación.  
  
> [!IMPORTANT]
>  Si migra a Windows Server Essentials, el siguiente mensaje de error se agrega al registro de eventos cada día durante el período de gracia de 21 días hasta que quite el servidor de origen de la red. Tras el período de gracia 21 días, el servidor de origen se apagará. <br> **La comprobación del rol FSMO detectó una condición en su entorno que no es compatible con la directiva de licencias. El Servidor de administración debe asumir los roles de Active Directory de controlador del dominio principal y de maestro de nomenclatura de dominios. Por lo tanto, debe mover las funciones de Active Directory al servidor de administración ahora. Este servidor se automáticamente cerrará si no se corrige el problema en 21 días desde el momento en que esta condición se detectó**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Instalar Windows Server Essentials o Windows Server 2012 R2 Standard en el servidor de destino  
  
1.  Instalar Windows Server Essentials o Windows Server 2012 R2 Standard con el rol de experiencia con Windows Server Essentials habilitado siguiendo las instrucciones de [instalar y configurar Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Si se inicia el asistente para configurar Windows Server Essentials, cancélelo.  
  
2.  Transfiera los roles FSMO del servidor de origen.  
  
    > [!NOTE]
    >  Si Windows Server Essentials es el único controlador de dominio en el dominio, el rol FSMO se mueve automáticamente al servidor que ejecuta Windows Server Essentials al degradar el servidor de origen.  
  
3.  Abra el Administrador del servidor y ejecute el Asistente para agregar roles y características.  
  
4.  Si no está instalado, agregue el rol Experiencia con Windows Server Essentials.  
  
5.  A continuación, aparecerá la tarea Configurar Windows Server Essentials en el área de notificación. Haga clic en ella y se iniciará el asistente para configurar Windows Server Essentials.  
  
6.  Siga las instrucciones para completar la configuración de Windows Server Essentials. Antes de ejecutar al asistente, haga lo siguiente:  
  
    -   Si es necesario, cambie el nombre de servidor. No podrá hacerlo después de completar el asistente para configurar Windows Server Essentials.  
  
    -   Asegúrese de que la hora y la configuración del servidor es correctos.  
  
7.  Haga lo siguiente para comprobar la instalación:  
  
    1.  Abra el Panel.  
  
    2.  Haga clic en la pestaña **Usuarios** y compruebe que las cuentas de usuario aparecen en Active Directory.  
  
### <a name="transfer-the-operations-master-roles"></a>Transferir los roles de maestro de operaciones  
 Las funciones (también denominado operaciones de maestro únicas flexible o FSMO) del maestro de operaciones se deben transferir desde el servidor de origen al servidor de destino en los 21 días de la instalación de Windows Server Essentials en el servidor de destino.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Para transferir los roles de maestro de operaciones:  
  
1.  En el servidor de destino, abra una ventana del símbolo del sistema como administrador. Vea [Para abrir una ventana del símbolo del sistema como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  En el símbolo del sistema, escriba **NETDOM QUERY FSMO**y, a continuación, presione ENTRAR.  
  
3.  En el símbolo del sistema, escriba **ntdsutil** y presione ENTRAR.  
  
4.  En el símbolo del sistema **ntdsutil**, escriba los siguientes comandos:  
  
    1.  Escriba **activate instance NTDS**y presione ENTRAR.  
  
    2.  Escriba **roles** y, a continuación, presione ENTRAR.  
  
    3.  Escriba **connections** y, a continuación, presione ENTRAR.  
  
    4.  Tipo **conectarse al servidor** *< ServerName\>*  (donde *< ServerName\>*  es el nombre del servidor de destino), y, a continuación, presione ENTRAR.  
  
    5.  En el símbolo del sistema, escriba **q** y, a continuación, presione ENTRAR.  
  
        1.  Escriba **transfer PDC**, presione ENTRAR y haga clic en **Sí** en el cuadro de diálogo de **confirmación de transferencia de rol**.  
  
        2.  Escriba **transfer infrastructure master**, presione ENTRAR y haga clic en **Sí** en el cuadro de diálogo de **confirmación de transferencia de rol**.  
  
        3.  Escriba **transfer naming master**, presione ENTRAR y haga clic en **Sí** en el cuadro de diálogo de **confirmación de transferencia de rol**.  
  
        4.  Escriba **transfer RID master**, presione ENTRAR y haga clic en **Sí** en el cuadro de diálogo de **confirmación de transferencia de rol** .  
  
        5.  Escriba **transfer schema master**, presione ENTRAR y haga clic en **Sí** en el cuadro de diálogo de **confirmación de transferencia de rol**.  
  
    6.  Escriba **q**y presione ENTRAR hasta que vuelva al símbolo del sistema.  
  
> [!NOTE]
>  Desde cualquier servidor de la red, puede comprobar que los roles de maestro de operaciones se han transferido al servidor de destino. Abra una ventana del símbolo del sistema como administrador (para más información, consulte [Para abrir una ventana del símbolo del sistema como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Escriba **netdom query fsmo** y, a continuación, presione ENTRAR.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ha instalado Windows Server Essentials como nuevo controlador de dominio de réplica. Ahora, vaya a [paso 3: Unir equipos al nuevo servidor de Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Para ver todos los pasos, consulte [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

