---
title: Paso 11 configuración de la implementación multisitio
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0061144387adf041216b7d0d264dced916bc7941
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860358"
---
# <a name="step-11-configure-the-multisite-deployment"></a>Paso 11 configuración de la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para configurar una implementación multisitio, realice cambios en el Asistente para configuración de acceso remoto actual en EDGE1, habilite la característica multisitio y, a continuación, agregue 2-EDGE1 como segundo punto de entrada.  
  
- Configurar el acceso remoto en EDGE1  
  
- Habilitar la configuración multisitio en EDGE1  
  
- Agregar 2-EDGE1 como segundo punto de entrada  
  
## <a name="configure-remote-access-on-edge1"></a><a name="configDA"></a>Configurar el acceso remoto en EDGE1  
  
1.  En la pantalla **Inicio** , escriba**RAMgmtUI. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**.  
  
3.  En el panel central de la consola, en el área **paso 2 servidor de acceso remoto** , haga clic en **Editar**.  
  
4.  Haga clic en **configuración de prefijo**. En la página **configuración de prefijo** , en **prefijos IPv6 de la red interna**, escriba **2001: db8:1::/64; 2001: db8:2::/64**. En el **prefijo IPv6 asignado a los equipos cliente de DirectAccess**, escriba **2001: db8:1: 1000::/64**, haga clic en **siguiente**y, a continuación, haga clic en **Finalizar**.  
  
5.  En el panel central de la consola, en el área de **servidores de infraestructura paso 3** , haga clic en **Editar**.  
  
6.  Haga clic en **lista de búsqueda de sufijos DNS**. En la página **lista de búsqueda de sufijos DNS** , asegúrese de que la casilla **configurar clientes de DirectAccess con la lista de búsqueda de sufijos de cliente DNS** está activada y que los sufijos de dominio **Corp.contoso.com** y **Corp2.Corp.contoso.com** aparecen en la lista **sufijos de dominio que se van a usar** , haga clic en **siguiente**y, a continuación, haga clic en finalizar.  
  
7.  En el panel central de la consola, haga clic en **Finalizar**.  
  
8.  En el cuadro de diálogo **revisión de acceso remoto** , revise las opciones de configuración y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
9. En el panel **tareas** , haga clic en **actualizar servidores de administración**y haga clic en **cerrar** cuando termine.  
  
## <a name="enable-multisite-configuration-on-edge1"></a><a name="EnabledMultisite"></a>Habilitar la configuración multisitio en EDGE1  
  
1.  En el panel **tareas** de la consola de administración de acceso remoto, haga clic en **Habilitar multisitio**.  
  
2.  En el Asistente para habilitar la implementación multisitio, en la página **antes de comenzar** , haga clic en **siguiente**.  
  
3.  En la página **nombre de implementación** , en nombre de la **implementación multisitio**, escriba **contoso**, en **primer nombre de punto de entrada**, escriba **Edge1-site**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **selección de punto de entrada** , haga clic en **asignar puntos de entrada automáticamente y permitir que los clientes seleccionen manualmente**y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **equilibrio de carga global** , haga clic en no **, no usar equilibrio de carga global**y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **compatibilidad con clientes** , haga clic en **permitir que los equipos cliente que ejecutan Windows 7 accedan a este punto de entrada**y haga clic en **Agregar**.  
  
7.  En el cuadro de diálogo **seleccionar grupos** , en **Escriba los nombres de objeto que desea seleccionar**, escriba **Win7_Clients_Site1**, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
8.  En la página **configuración de GPO de cliente** , haga clic en **siguiente**.  
  
9. En la página **Resumen** , haga clic en **confirmar**.  
  
10. En el cuadro de diálogo **Habilitar la implementación multisitio** , haga clic en **cerrar** y, a continuación, en el Asistente para habilitar la implementación multisitio, haga clic en **cerrar**.  
  
## <a name="add-2-edge1-as-a-second-entry-point"></a><a name="AddEP"></a>Agregar 2-EDGE1 como segundo punto de entrada  
  
1.  En el panel **tareas** de la consola de administración de acceso remoto, haga clic en **Agregar un punto de entrada**.  
  
2.  En el Asistente para agregar un punto de entrada, en la página **detalles del punto de entrada** , en servidor de **acceso remoto**, escriba **2-edge1.Corp2.Corp.contoso.com**, en nombre de **punto de entrada**, escriba **2-edge1-site**y, a continuación, haga clic en **siguiente**.  
  
3.  En la página **topología de red** , haga clic en **borde**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **nombre de red o dirección IP** , en **Escriba el nombre público o la dirección IP que usan los clientes para conectarse al servidor de acceso remoto**, escriba **2-edge1.contoso.com**y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **adaptadores de red** , asegúrese de que el **adaptador externo** es **Internet**, el **adaptador interno** es **2-Corpnet**, el certificado es **CN = 2-edge1.contoso.com**y, a continuación, haga clic en **siguiente**.  
  
6.  En la **página Configuración de prefijo** , en **prefijo IPv6 asignado a equipos cliente de DirectAccess**, escriba **2001: db8:2: 2000::/64**y, a continuación, haga clic en **siguiente**.  
  
7.  En la página **compatibilidad con clientes** , haga clic en **permitir que los equipos cliente que ejecutan Windows 7 accedan a este punto de entrada**y haga clic en **Agregar**.  
  
8.  En el cuadro de diálogo **seleccionar grupos** , en **Escriba los nombres de objeto que desea seleccionar**, escriba **Win7_Clients_Site2**, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
9. En la página **configuración de GPO de cliente** , haga clic en **siguiente**.  
  
10. En la página **configuración de GPO de servidor** , haga clic en **siguiente**.  
  
11. En la página **Resumen** , haga clic en **confirmar**.  
  
12. En el cuadro de diálogo **Agregar punto de entrada** , haga clic en **cerrar** y, a continuación, en el Asistente para agregar un punto de entrada, haga clic en **cerrar**.  
  


