---
title: PASO 3 instalar y configurar CLIENT2
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2c7ff4953fd4369340f55f40f93cfc01d4240b26
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281467"
---
# <a name="step-3-install-and-configure-client2"></a>PASO 3 instalar y configurar CLIENT2

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Client2 es Windows 7&reg; equipo que se usa para demostrar la hacia atrás compatibilidad de acceso remoto que se ejecutan en servidores de Windows Server 2016.  
  
1. Para instalar el sistema operativo en CLIENT2. Instalar Windows&reg; 7 Enterprise o Windows&reg; 7 Ultimate en CLIENT2.  
  
2. Para unirse a CLIENT2 en el dominio CORP. Únase a CLIENT2 al dominio corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Para instalar el sistema operativo en CLIENT2  
  
1.  Inicie la instalación de Windows 7.  
  
2.  Cuando se le pida un nombre de usuario, escriba **User1**. Cuando se le pida un nombre de equipo, escriba **CLIENT2**.  
  
3.  Cuando se le pida una contraseña, escriba una contraseña segura dos veces.  
  
4.  Cuando se le pida para la configuración de protección, haga clic en **usar la configuración recomendada**.  
  
5.  Cuando se le solicitará la ubicación del equipo actual, haga clic en **red trabajo**.  
  
6.  Conecte CLIENT2 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows 7 y, a continuación, desconectarse de Internet.  
  
7.  CLIENT2 se conecte a la subred Corpnet.  
  
## <a name="user-account-control"></a>Control de cuentas de usuario  
Al configurar el sistema operativo Windows 7, debe hacer clic en **continuar** en el **User Account Control** cuadro de diálogo (UAC) para realizar algunas tareas. Algunas de las tareas de configuración requieren la aprobación de UAC. Cuando se le pida, haga clic en siempre **continuar** para autorizar a estos cambios.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Para unirse a CLIENT2 en el dominio CORP  
  
1.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo**y después haga clic en **Propiedades**.  
  
2.  En el **sistema** página, en el **configuración de nombre, dominio y grupo de trabajo de equipo** área, haga clic en **cambiar la configuración de**.  
  
3.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
4.  En el **cambios de dominio o el nombre de equipo** cuadro de diálogo, haga clic en **dominio**, tipo **corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando se le pida un nombre de usuario y contraseña, escriba el nombre de usuario y la contraseña de la cuenta de dominio de User1 y, a continuación, haga clic en **Aceptar**.  
  
6.  Cuando vea un cuadro de diálogo de bienvenida al dominio corp.contoso.com, haga clic en **Aceptar**.  
  
7.  Cuando vea un cuadro de diálogo cuadro que le pide que reinicie el equipo, haga clic en **Aceptar**.  
  
8.  En el **las propiedades del sistema** cuadro de diálogo, haga clic en **cerrar**, y cuando vea un cuadro de diálogo que le pide que reinicie el equipo, haga clic en **reiniciar ahora**.  
  
9. Una vez reiniciado el equipo, inicie sesión como corp\usuario1.
