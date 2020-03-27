---
title: Paso 4 crear el clúster de acceso remoto con equilibrio de carga de red
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c2f855512b978462f89b8f32b1f7edf59180f563
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310776"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>Paso 4 crear el clúster de acceso remoto con equilibrio de carga de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 permiten crear clústeres de servidores de acceso remoto. Un clúster actúa como un único servidor lógico y proporciona una configuración y administración centralizada para los servidores del clúster. Al usar equilibrio de carga de red (NLB), se admiten hasta 8 miembros de acceso remoto en un único clúster. Los clústeres de acceso remoto proporcionan alta disponibilidad y equilibrio de carga de las conexiones desde los clientes de DirectAccess a la red interna.  
  
Los procedimientos siguientes le permiten crear y probar un clúster de acceso remoto:  
  
1. Instale la característica de equilibrio de carga de red en EDGE1 y EDGE2. Antes de habilitar el equilibrio de carga, debe instalar la característica de equilibrio de carga de red en EDGE1 y EDGE2.
  
2. Habilite el equilibrio de carga en EDGE1. EDGE1 se instaló originalmente en modo de servidor único. Para habilitar el equilibrio de carga, configure nuevas direcciones IP dedicadas internas y externas (DIP) para EDGE1. Las DIP anteriores en EDGE1 se configuran automáticamente como direcciones IP virtuales (VIP) para el clúster. La nueva DIP externa es, 131.107.0.10, la nueva DIP IPv4 interna es 10.0.0.10, la nueva DIP IPv6 interna es 2001: db8:1:: 10. Las VIP de clúster son 131.107.0.2 y 131.107.0.3 (external), y 10.0.0.2 y 2001: db8:1:: 2 (interno).
  
3. Agregue EDGE2 al clúster de carga equilibrada. Después de habilitar el equilibrio de carga, ahora puede Agregar EDGE2 al clúster para proporcionar equilibrio de carga y alta disponibilidad para las conexiones de cliente de DirectAccess.

## <a name="prerequisites"></a>Requisitos previos

Si va a crear este laboratorio de pruebas en máquinas virtuales, debe habilitar la suplantación de direcciones MAC en EDGE1 y EDGE2.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Habilitar la suplantación de direcciones MAC en EDGE1 y EDGE2  
  
1.  Realice un apagado estable en EDGE1 y EDGE2.  
  
2.  En el equipo que hospeda las máquinas virtuales, en el **Administrador de Hyper-V**, haga clic con el botón secundario en EDGE1 y, a continuación, haga clic en **configuración**.  
  
3.  En el cuadro de diálogo **configuración** , en la lista **hardware** , haga clic en el adaptador de red conectado a la red CorpNet y, a continuación, en el panel de detalles, active la casilla **Habilitar suplantación de direcciones MAC** .  
  
4.  En la lista **hardware** , haga clic en el adaptador de red conectado a la red de Internet y, a continuación, en el panel de detalles, active la casilla **Habilitar suplantación de direcciones MAC** .  
  
5.  En el cuadro de diálogo **configuración** , haga clic en **Aceptar**.  
  
6.  Repita este procedimiento en el paso 2 de EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Instalar la característica de equilibrio de carga de red en EDGE1 y EDGE2  
Para configurar EDGE1 y EDGE2 en un clúster, debe instalar la característica de equilibrio de carga de red en EDGE1 y EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Para instalar el equilibrio de carga de red  
  
1.  En EDGE1, en la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haga clic en **siguiente** cuatro veces para ir a la pantalla de selección de características del servidor.  
  
3.  En el cuadro de diálogo **seleccionar características** , seleccione **equilibrio de carga de red**, haga clic en **Agregar características**, haga clic en **siguiente**y, a continuación, haga clic en **instalar**.  
  
4.  En el cuadro de diálogo **Progreso de la instalación**, comprueba que la instalación se realiza correctamente y, a continuación, haz clic en **Cerrar**.  
  
5.  Repita este procedimiento en EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Habilitación del equilibrio de carga en EDGE1  
Use este procedimiento para habilitar el equilibrio de carga y configurar las nuevas DIP en EDGE1.  
  
### <a name="enable-load-balancing"></a>Habilitar el equilibrio de carga  
  
1.  En EDGE1, haga clic en **Inicio**, escriba **RAMgmtUI. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En el panel izquierdo de la consola de administración de acceso remoto, haga clic en **configuración**y, a continuación, en el panel **tareas** , haga clic en **Habilitar equilibrio de carga**.  
  
3.  En el Asistente para habilitar equilibrio de carga, haga clic en **siguiente**.  
  
4.  En la página **método de equilibrio de carga** , haga clic en **usar equilibrio de carga de red (NLB) de Windows**y, a continuación, en **siguiente**.  
  
5.  En la página **direcciones IP dedicadas externas** , en el cuadro **dirección IPv4** , **Escriba, 131.107.0.10**, en el cuadro **máscara de subred** , compruebe que el prefijo de subred es **255.255.255.0**y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **direcciones IP dedicadas internas** , realice lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el cuadro **dirección IPv4** , escriba **10.0.0.10** y, en el cuadro **máscara de subred** , compruebe que el prefijo de subred es **255.255.255.0**.  
  
    2.  En el cuadro **dirección IPv6** , escriba **2001: db8:1:: 10** y, en la longitud del prefijo de subred, compruebe que el valor es **64**.  
  
7.  En la página **Resumen** , haga clic en **confirmar**.  
  
8.  En el cuadro de diálogo **Habilitar equilibrio de carga** , haga clic en **cerrar**.  
  
9. En el Asistente para habilitar equilibrio de carga, haga clic en **cerrar**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Agregar EDGE2 al clúster de carga equilibrada  
Use este procedimiento para agregar EDGE2 al clúster de NLB.  
  
> [!NOTE]  
> Debe esperar dos minutos después de completar los pasos anteriores antes de continuar. Después de habilitar NLB, el RAConfigTask se ejecuta y configura la máquina con la configuración de NLB. Esto puede tardar unos minutos en completarse y, si el administrador ejecuta otra configuración relacionada con NLB antes de que finalice la tarea, se producirá un error en la configuración.  
  
### <a name="add-edge2-to-the-cluster"></a>Agregar EDGE2 al clúster  
  
1.  En el equipo de EDGE1 o máquina virtual, en la consola de administración de acceso remoto, en el panel **tareas** , en **clúster de carga equilibrada**, haga clic en **Agregar o quitar servidores**.  
  
2.  En el cuadro de diálogo **Agregar o quitar servidores** , haga clic en **Agregar servidor**.  
  
3.  En el Asistente para **Agregar un servidor** , en la página **Seleccionar servidor** , escriba **EDGE2**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **adaptadores de red** , en **adaptador externo**, asegúrese de que está seleccionado Internet y, en **adaptador interno**, asegúrese de que está seleccionada la opción **red** **corporativa** . Haga clic en **examinar**, en el cuadro de diálogo **seguridad de Windows** , asegúrese de que está seleccionado **certificado IP-https** , haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **Resumen** , haga clic en **Agregar**.  
  
6.  En la página **Finalización**, haga clic en **Cerrar**.  
  
7.  En el cuadro de diálogo **Agregar o quitar servidores** , haga clic en **confirmar**.  
  
8.  En el cuadro de diálogo **Agregar y quitar servidores** , haga clic en **cerrar**.  
  
9. En la pantalla **Inicio** , escriba**nlbmgr. exe**y presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
10. En el **Administrador de equilibrio de carga de red**, haga clic en **clúster de das interno**. En el panel de detalles, asegúrese de que tanto **EDGE1 (CorpNet)** como **EDGE2 (CorpNet)** tienen el estado **convergido**.  
  
11. Si un servidor no **converge**, en el árbol de consola, haga clic con el botón secundario en el servidor, seleccione **controlar host**y, a continuación, haga clic en **iniciar**.  
  
12. En el **Administrador de equilibrio de carga de red**, haga clic en **clúster de das de Internet**. Asegúrese de que en el panel de detalles, tanto **EDGE1 (Internet)** como **EDGE2 (Internet)** tienen el estado **convergido**.  
  
13. Si un servidor no **converge**, en el árbol de consola, haga clic con el botón secundario en el servidor, seleccione **controlar host**y, a continuación, haga clic en **iniciar**.
