---
title: Paso 3 instalación y configuración de cliente2
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 28c612e3e4b2df5095dcf4abe6f081e11d8722dd
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950021"
---
# <a name="step-3-install-and-configure-client2"></a>Paso 3 instalación y configuración de cliente2

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cliente2 es un equipo de Windows 7 &reg;  que se usa para mostrar la compatibilidad con versiones anteriores de acceso remoto que se ejecuta en servidores de Windows Server 2016.

1. Para instalar el sistema operativo en cliente2. Instale Windows &reg; 7 Enterprise o Windows &reg; 7 Ultimate en cliente2.

2. Para unir el cliente2 al dominio CORP. Únase a cliente2 en el dominio corp.contoso.com.

## <a name="to-install-the-operating-system-on-client2"></a>Para instalar el sistema operativo en cliente2

1.  Inicie la instalación de Windows 7.

2.  Cuando se le pida un nombre de usuario, escriba **user1**. Cuando se le pida un nombre de equipo, escriba **cliente2**.

3.  Cuando se le solicite una contraseña, escriba una contraseña segura dos veces.

4.  Cuando se le pida la configuración de protección, haga clic en **Usar configuración recomendada**.

5.  Cuando se le solicite la ubicación actual del equipo, haga clic en **red de trabajo**.

6.  Conecte el cliente2 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes de Windows 7 y, a continuación, desconéctese de Internet.

7.  Conecte el cliente2 a la subred de la red corporativa.

## <a name="user-account-control"></a>Control de cuentas de usuario
Al configurar el sistema operativo Windows 7, es necesario hacer clic en **continuar** en el cuadro de diálogo **control de cuentas de usuario** (UAC) para algunas tareas. Algunas de las tareas de configuración requieren la aprobación de UAC. Cuando se le pida, haga clic en **continuar** para autorizar estos cambios.

## <a name="to-join-client2-to-the-corp-domain"></a>Para unir el cliente2 al dominio CORP

1.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo** y, después, haga clic en **Propiedades**.

2.  En la página **sistema** , en el área **configuración de nombre, dominio y grupo de trabajo del equipo** , haga clic en **Cambiar configuración**.

3.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.

4.  En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , haga clic en **dominio**, escriba **Corp.contoso.com** y, a continuación, haga clic en **Aceptar**.

5.  Cuando se le pida un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de la cuenta de dominio user1 y, a continuación, haga clic en **Aceptar**.

6.  Cuando vea un cuadro de diálogo de bienvenida al dominio corp.contoso.com, haga clic en **Aceptar**.

7.  Cuando vea un cuadro de diálogo que le pida que reinicie el equipo, haga clic en **Aceptar**.

8.  En el cuadro de diálogo **propiedades del sistema** , haga clic en **cerrar** y, cuando vea un cuadro de diálogo que le pide que reinicie el equipo, haga clic en **reiniciar ahora**.

9. Una vez reiniciado el equipo, inicie sesión como CORP\User1.
