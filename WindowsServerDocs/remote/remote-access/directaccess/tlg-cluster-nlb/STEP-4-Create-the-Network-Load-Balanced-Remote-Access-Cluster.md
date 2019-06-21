---
title: PASO 4 crear el clúster de acceso remoto con equilibrio de carga de red
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8e6defc6cc8579f18df2f9636383c65c9a18eb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281611"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>PASO 4 crear el clúster de acceso remoto con equilibrio de carga de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 le permiten crear clústeres de servidores de acceso remoto. Un clúster actúa como un único servidor lógico y proporciona la configuración centralizada y administración de los servidores del clúster. Cuando se usa el equilibrio de carga de red (NLB) no hay compatibilidad con hasta 8 miembros de acceso remoto en un único clúster. Los clústeres de acceso remotos proporcionan una alta disponibilidad y equilibrio de carga de las conexiones de los clientes de DirectAccess a la red interna.  
  
Los procedimientos siguientes le permiten crear y probar un clúster de acceso remoto:  
  
1. Instale la característica de equilibrio de carga de red en EDGE1 y EDGE2. Antes de habilitar el equilibrio de carga, debe instalar la característica de equilibrio de carga de red en EDGE1 y EDGE2.
  
2. Habilitar el equilibrio de carga en EDGE1. EDGE1 se instaló originalmente en modo de servidor único. Para habilitar el equilibrio de carga, configura nuevas externas e internas direcciones IP dedicadas (DIP) para EDGE1. La DIP en EDGE1 anterior se configura automáticamente como direcciones IP virtuales (VIP) para el clúster. La nueva DIP externo es 131.107.0.10, la nueva DIP interno de IPv4 es 10.0.0.10, la nueva DIP de IPv6 interna es 2001:db8:1::10. Las VIP de clúster son 131.107.0.2 y 131.107.0.3 (externo) y 10.0.0.2 y 2001:db8:1::2 (interno).
  
3. Agregar EDGE2 para el clúster con equilibrio de carga. Después de habilitar el equilibrio de carga, ahora puede agregar EDGE2 al clúster para proporcionar disponibilidad alta y equilibrio de carga para las conexiones de cliente de DirectAccess.

## <a name="prerequisites"></a>Requisitos previos

Si va a crear este laboratorio de pruebas en máquinas virtuales, debe habilitar la suplantación en EDGE1 y EDGE2 de direcciones de MAC.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Habilitar la suplantación de identidad en EDGE1 y EDGE2 de direcciones MAC  
  
1.  Realizar un apagado ordenado en EDGE1 y EDGE2.  
  
2.  En el equipo que hospeda las máquinas virtuales, en **Administrador de Hyper-V**, haga clic en EDGE1 y, a continuación, haga clic en **configuración**.  
  
3.  En el **configuración** cuadro de diálogo el **Hardware** lista, haga clic en el adaptador de red conectado a la red de la red corporativa y, a continuación, en el panel de detalles, seleccione el **habilitar suplantación de direcciones MAC**  casilla de verificación.  
  
4.  En el **Hardware** lista, haga clic en el adaptador de red conectado a la red de Internet y, a continuación, en el panel de detalles, seleccione el **habilitar suplantación de direcciones MAC** casilla de verificación.  
  
5.  En el **configuración** cuadro de diálogo, haga clic en **Aceptar**.  
  
6.  Repita este procedimiento en el paso 2 en EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Instale la característica de equilibrio de carga de red en EDGE1 y EDGE2  
Para configurar EDGE1 y EDGE2 en un clúster, debe instalar la característica de equilibrio de carga de red en EDGE1 y EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Para instalar el equilibrio de carga de red  
  
1.  En EDGE1, en la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **siguiente** cuatro veces para ir a la pantalla de selección de características de servidor.  
  
3.  En el **seleccionar características** cuadro de diálogo, seleccione **Network Load Balancing**, haga clic en **agregar características**, haga clic en **siguiente**y, a continuación, haga clic en **Instalar**.  
  
4.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
5.  Repita este procedimiento en EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Habilitar el equilibrio de carga en EDGE1  
Utilice este procedimiento para habilitar el equilibrio de carga y configurar la nueva DIP en EDGE1.  
  
### <a name="enable-load-balancing"></a>Habilitar el equilibrio de carga  
  
1.  En EDGE1, haga clic en **iniciar**, tipo **RAMgmtUI.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, en el panel izquierdo, haga clic en **configuración**y, a continuación, en el **tareas** panel, haga clic en **habilitar Equilibrio de carga**.  
  
3.  Habilitar Asistente de equilibrio de carga, haga clic en **siguiente**.  
  
4.  En el **el método de equilibrio de carga** página, haga clic en **equilibrio de carga de red (NLB) Use Windows**y, a continuación, haga clic en **siguiente**.  
  
5.  En el **de direcciones IP externas dedicado** página, en el **dirección IPv4** , escriba **131.107.0.10**, en el **máscara de subred** , compruebe la prefijo de subred es **255.255.255.0**y, a continuación, haga clic en **siguiente**.  
  
6.  En el **las direcciones IP internas dedicado** página, haga lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el **dirección IPv4** , escriba **10.0.0.10** y en el **máscara de subred** , compruebe que el prefijo de subred es **255.255.255.0**.  
  
    2.  En el **dirección IPv6** , escriba **2001:db8:1::10** y en la longitud del prefijo de subred, compruebe el valor es **64**.  
  
7.  En el **resumen** página, haga clic en **confirmar**.  
  
8.  En el **habilitar Equilibrio de carga** cuadro de diálogo, haga clic en **cerrar**.  
  
9. Habilitar Asistente de equilibrio de carga, haga clic en **cerrar**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Agregar EDGE2 para el clúster con equilibrio de carga  
Utilice este procedimiento para agregar EDGE2 al clúster de NLB.  
  
> [!NOTE]  
> Debe esperar dos minutos después de completar los pasos anteriores antes de continuar. Después de habilitar NLB, la RAConfigTask se ejecuta y configura la máquina con la configuración de NLB. Esta operación puede tardar unos minutos en completarse y, si el administrador ejecuta otra configuración relacionada NLB antes de que finalice la tarea, se producirá un error en esa configuración.  
  
### <a name="add-edge2-to-the-cluster"></a>Agregar EDGE2 al clúster  
  
1.  En el equipo de EDGE1 o máquina virtual, en la consola de administración de acceso remoto, en el **tareas** panel, en **clúster con equilibrio de carga**, haga clic en **agregar o quitar servidores**.  
  
2.  En el **agregar o quitar servidores** cuadro de diálogo, haga clic en **Agregar servidor**.  
  
3.  En el **agregar un servidor** asistente la **Seleccionar servidor** , escriba **EDGE2**y, a continuación, haga clic en **siguiente**.  
  
4.  En el **adaptadores de red** página **adaptador externo**, asegúrese de que **Internet** está seleccionada y en **adaptador interno**, asegúrese de que que **Corpnet** está seleccionada. Haga clic en **examinar**, en el **Windows Security** diálogo cuadro, asegúrese de que **certificado IP-HTTPS** está seleccionado, haga clic en **Aceptar**y, a continuación, haga clic en **Siguiente**.  
  
5.  En el **resumen** página, haga clic en **agregar**.  
  
6.  En la página **Finalización**, haga clic en **Cerrar**.  
  
7.  En el **agregar o quitar servidores** cuadro de diálogo, haga clic en **confirmar**.  
  
8.  En el **agregar y quitar servidores** cuadro de diálogo, haga clic en **cerrar**.  
  
9. En el **iniciar** , escriba**nlbmgr.exe**y presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
10. En el **Administrador de equilibrio de carga de red**, haga clic en **clúster interno DA**. En el panel de detalles, asegúrese de que ambos **EDGE1(Corpnet)** y **EDGE2(Corpnet)** tienen el estado **convergente**.  
  
11. Si no es un servidor **convergente**, en el árbol de consola, haga clic en el servidor, seleccione **Control Host**y, a continuación, haga clic en **iniciar**.  
  
12. En el **Administrador de equilibrio de carga de red**, haga clic en **clúster DA Internet**. Asegúrese de que en el panel de detalles, ambos **EDGE1(Internet)** y **EDGE2(Internet)** tienen el estado **convergente**.  
  
13. Si no es un servidor **convergente**, en el árbol de consola, haga clic en el servidor, seleccione **Control Host**y, a continuación, haga clic en **iniciar**.
