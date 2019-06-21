---
title: PASO 11 configurar la implementación multisitio
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c95fb641c0d0fa3161caadfa2eb769e12b47672d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283233"
---
# <a name="step-11-configure-the-multisite-deployment"></a>PASO 11 configurar la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para configurar una implementación multisitio, realizar cambios en el Asistente de configuración acceso remoto actual en EDGE1, habilitar la funcionalidad de multisitio y, a continuación, agregar 2 EDGE1 como un segundo punto de entrada.  
  
- Configurar el acceso remoto en EDGE1  
  
- Habilitar la configuración multisitio en EDGE1  
  
- Agregar 2 EDGE1 como un segundo punto de entrada  
  
## <a name="configDA"></a>Configurar el acceso remoto en EDGE1  
  
1.  En el **iniciar** , escriba**RAMgmtUI.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**.  
  
3.  En el panel central de la consola, en el **paso 2: servidor de acceso remoto** área, haga clic en **editar**.  
  
4.  Haga clic en **configuración de prefijo**. En el **configuración de prefijo** página **prefijos IPv6 de la red interna**, escriba **2001:db8:1:: / 64; 2001:db8:2:: / 64**. En **prefijo IPv6 asignado a los equipos cliente de DirectAccess**, escriba **2001:db8:1:1000:: / 64**, haga clic en **siguiente**y, a continuación, haga clic en **finalizar** .  
  
5.  En el panel central de la consola, en el **paso 3: servidores de infraestructura** área, haga clic en **editar**.  
  
6.  Haga clic en **lista de búsqueda de sufijos DNS**. En el **lista de búsqueda de sufijos DNS** página, asegúrese de que el **los clientes de configurar DirectAccess con la lista de búsqueda de sufijos de cliente DNS** casilla está activada y que el **corp.contoso.com** y **corp2.corp.contoso.com** sufijos de dominio aparecen en la **sufijos de dominio para usar** lista, haga clic en **siguiente**y, a continuación, haga clic en Finalizar.  
  
7.  En el panel central de la consola, haga clic en **finalizar**.  
  
8.  En el **revisión de acceso remoto** cuadro de diálogo, revisar la configuración predeterminada y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
9. En el **tareas** panel, haga clic en **actualizar servidores de administración**y haga clic en **cerrar** cuando termine.  
  
## <a name="EnabledMultisite"></a>Habilitar la configuración multisitio en EDGE1  
  
1.  En la consola de administración de acceso remoto, en el **tareas** panel, haga clic en **habilitar multisitio**.  
  
2.  En el Asistente para habilitar implementación multisitio, en el **antes de comenzar** página, haga clic en **siguiente**.  
  
3.  En el **nombre de la implementación** página **nombre de la implementación multisitio**, tipo **Contoso**, en **primer punto de entrada nombre**, tipo **Edge1 sitio**y, a continuación, haga clic en **siguiente**.  
  
4.  En el **selección del punto de entrada** página, haga clic en **asignar automáticamente los puntos de entrada y permitir a los clientes seleccionar manualmente**y, a continuación, haga clic en **siguiente**.  
  
5.  En el **equilibrio de carga Global** página, haga clic en **No, no use equilibrio de carga global**y, a continuación, haga clic en **siguiente**.  
  
6.  En el **compatibilidad con clientes** página, haga clic en **permitir cliente en equipos que ejecutan Windows 7 para tener acceso a este punto de entrada**y haga clic en **agregar**.  
  
7.  En el **seleccionar grupos** cuadro de diálogo **escriba los nombres de objeto para seleccionar**, tipo **Win7_Clients_Site1**, haga clic en **Aceptar**y, a continuación, haga clic en **Siguiente**.  
  
8.  En el **configuración de GPO de cliente** página, haga clic en **siguiente**.  
  
9. En el **resumen** página, haga clic en **confirmar**.  
  
10. En el **Habilitar implementación multisitio** cuadro de diálogo, haga clic en **cerrar** y, a continuación, haga clic en el Asistente para habilitar implementación multisitio, **cerrar**.  
  
## <a name="AddEP"></a>Agregar 2 EDGE1 como un segundo punto de entrada  
  
1.  En la consola de administración de acceso remoto, en el **tareas** panel, haga clic en **agregar un punto de entrada**.  
  
2.  En el agregar un punto de entrada asistente, en el **detalles del punto de entrada** página **servidor de acceso remoto**, tipo **2 edge1.corp2.corp.contoso.com**, en **entrada nombre del punto de**, tipo **Edge1-2-Site**y, a continuación, haga clic en **siguiente**.  
  
3.  En el **topología de red** página, haga clic en **Edge**y, a continuación, haga clic en **siguiente**.  
  
4.  En el **nombre de red o la dirección IP** página **tipo en el nombre público o dirección IP usada por los clientes para conectarse al servidor de acceso remoto**, tipo **2 edge1.contoso.com**, y a continuación, haga clic en **siguiente**.  
  
5.  En el **adaptadores de red** página, asegúrese de que el **adaptador externo** es **Internet**, **adaptador interno** es **2 Corpnet -** , el certificado es **CN = 2 edge1.contoso.com**y, a continuación, haga clic en **siguiente**.  
  
6.  En el **configuración de prefijo** página **prefijo IPv6 asignado a los equipos cliente de DirectAccess**, tipo **2001:db8:2:2000:: / 64**y, a continuación, haga clic en **siguiente** .  
  
7.  En el **compatibilidad con clientes** página, haga clic en **permitir cliente en equipos que ejecutan Windows 7 para tener acceso a este punto de entrada**y haga clic en **agregar**.  
  
8.  En el **seleccionar grupos** cuadro de diálogo **escriba los nombres de objeto para seleccionar**, tipo **Win7_Clients_Site2**, haga clic en **Aceptar**y, a continuación, haga clic en **Siguiente**.  
  
9. En el **configuración de GPO de cliente** página, haga clic en **siguiente**.  
  
10. En el **configuración de GPO de servidor** página, haga clic en **siguiente**.  
  
11. En el **resumen** página haga clic en **confirmar**.  
  
12. En el **agregar punto de entrada** cuadro de diálogo, haga clic en **cerrar** y, a continuación, haga clic en el punto de entrada Asistente para agregar un, **cerrar**.  
  


