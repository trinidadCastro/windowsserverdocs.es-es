---
title: Paso 7 probar la conectividad de DirectAccess desde Internet
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 738e0f10762c0d292e344ba25fa34cdb0d17b766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367539"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>Paso 7 probar la conectividad de DirectAccess desde Internet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La implementación de contraseña de un solo tiempo (OTP) de DirectAccess se ha probado desde la subred HomeNet y ahora se puede probar desde Internet.  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Para probar la funcionalidad de OTP desde Internet en CLIENT1  
  
1. En CLIENT1, asegúrese de que ha iniciado sesión como **user1**. Conecte CLIENT1 a la subred de la red corporativa.  
  
2. En la pantalla **Inicio** , escriba**PowerShell. exe**, haga clic con el botón derecho en **PowerShell**, haga clic en **Opciones avanzadas**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
3. En la ventana de Windows PowerShell, escriba **gpupdate/force** y presione Entrar.  
  
4. Desconecte CLIENT1 de la subred HomeNet, conéctela a Internet y reinicie el equipo.  
  
5. En CLIENT1, abra Internet Explorer y, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione Entrar. Presiona F5.  
  
   El sitio no debe abrirse.  
  
6. En la pantalla **Inicio** , escriba**RSA**y haga clic en **token RSA SecurID**.  
  
7. Espere hasta que el token de SecurID de RSA cambie la contraseña de un solo tiempo y, a continuación, haga clic en **copiar**.  
  
8. Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.  
  
9. Haga clic en **conexión del área de trabajo**y en **continuar**.  
  
10. Presione Ctrl + Alt + Supr y haga clic en el icono de **contraseña de un solo tiempo (OTP)** .  
  
11. Pegue el token del token de ocho dígitos copiado anteriormente y haga clic en **Aceptar**. Espere a que se complete la autenticación. El estado de la conexión del área de trabajo de DirectAccess ahora estará **conectado**.  
  
12. En Internet Explorer, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione Entrar. Presiona F5. Verás el sitio web IIS predeterminado en APP1.  
  
13. En la barra de direcciones de Internet Explorer, escriba **https://app2.corp.contoso.com/** y presione Entrar. Presiona F5. Verá el sitio web de IIS predeterminado en APP2.  
  
14. En la pantalla **Inicio** , escriba<strong>\\ \ APP1\FILES</strong>y presione Entrar.  
  
15. En la ventana carpetas compartidas de **archivos** , haga doble clic en el archivo **example. txt** . Verá el contenido del archivo example. txt.  
  
16. En la pantalla **Inicio** , escriba<strong>\\ \ APP2\FILES</strong>y presione Entrar.  
  
17. En la ventana carpetas compartidas de **archivos** , haga doble clic en el nuevo archivo de **texto documento. txt** . Verá el contenido del nuevo archivo de texto Document. txt.  
  


