---
title: PASO 6 prueba la conectividad de DirectAccess desde la subred Homenet
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c19376b7301068639ba1315af5e9f5a9a4514b3b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283056"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>PASO 6 prueba la conectividad de DirectAccess desde la subred Homenet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La implementación de la contraseña de un solo uso (OTP) de DirectAccess ahora está completa y puede empezar a probar la conectividad desde la subred Homenet.  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Para probar la funcionalidad OTP desde la subred Homenet en CLIENT1  
  
1. En CLIENTE1, asegúrese de que se registran como **User1**.  
  
2. En el **iniciar** , escriba**powershell.exe**, haga clic en **powershell**, haga clic en **avanzadas**y, a continuación, haga clic en **ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
3. En la ventana de Windows PowerShell, escriba **gpupdate /force**, y presione ENTRAR.  
  
4. Desconecte CLIENT1 de la subred Corpnet y conéctelo a la subred Homenet.  
  
5. En CLIENT1, abra Internet Explorer y, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione ENTRAR. Presiona F5.  
  
   No debe abrir el sitio.  
  
6. En el **iniciar** , escriba**RSA**y haga clic en **RSA SecurID Token**.  
  
7. Espere a que el token de RSA SecurID cambia la contraseña de un solo uso y, a continuación, haga clic en **copia**.  
  
8. Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.  
  
9. Haga clic en **conexión de DirectAccess de Contoso**y haga clic en **continuar**.  
  
10. Presione Ctrl + Alt + Supr y haga clic en el **contraseña de un solo uso (OTP)** icono.  
  
11. Pegue el código de token de ocho dígitos copiados anteriormente y haga clic en **Aceptar**. Espere completar la autenticación. El estado de conexión de DirectAccess al área de trabajo ahora estará **conectado**.  
  
12. En Internet Explorer, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione ENTRAR. Presiona F5. Verás el sitio web IIS predeterminado en APP1.  
  
13. En la barra de direcciones de Internet Explorer, escriba **https://app2.corp.contoso.com/** y presione ENTRAR. Presiona F5. Verá el sitio Web de IIS predeterminado en APP2.  
  
14. En el **iniciar** , escriba<strong>\\\app1\files</strong>, y presione ENTRAR.  
  
15. En el **archivos** ventana de carpeta compartida, haga doble clic en el **Example.txt** archivo. Verá el contenido del archivo Example.txt.  
  
16. En el **iniciar** , escriba<strong>\\\app2\files</strong>, y presione ENTRAR.  
  
17. En el **archivos** ventana de carpeta compartida, haga doble clic en el **nuevo texto.txt** archivo. Verá el contenido del archivo texto.txt nuevo.  
  


