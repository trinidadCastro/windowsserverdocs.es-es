---
title: Paso 6 probar la conectividad de DirectAccess desde la subred HomeNet
description: Obtenga información sobre cómo empezar a probar la conectividad desde la subred HomeNet.
manager: brianlic
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 1d0cd5c85ca97ffd7d5275d2402db17a560255fa
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040345"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>Paso 6 probar la conectividad de DirectAccess desde la subred HomeNet

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La implementación de contraseña de un solo tiempo (OTP) de DirectAccess ya está completa y puede empezar a probar la conectividad desde la subred HomeNet.

### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Para probar la funcionalidad de OTP desde la subred HomeNet en CLIENT1

1. En CLIENT1, asegúrese de que ha iniciado sesión como **user1**.

2. En la pantalla **Inicio** , escriba **powershell.exe**, haga clic con el botón secundario en **PowerShell**, haga clic en **Opciones avanzadas** y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

3. En la ventana de Windows PowerShell, escriba **gpupdate/force** y presione Entrar.

4. Desconecte CLIENT1 de la subred de la red corporativa y conéctelo a la subred HomeNet.

5. En CLIENT1, abra Internet Explorer y, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione Entrar. Presione F5.

   El sitio no debe abrirse.

6. En la pantalla **Inicio** , escriba **RSA** y haga clic en **token RSA SecurID**.

7. Espere hasta que el token de SecurID de RSA cambie la contraseña de un solo tiempo y, a continuación, haga clic en **copiar**.

8. Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.

9. Haga clic en **conexión de DirectAccess de Contoso** y haga clic en **continuar**.

10. Presione Ctrl + Alt + Supr y haga clic en el icono de **contraseña de un solo tiempo (OTP)** .

11. Pegue el token del token de ocho dígitos copiado anteriormente y haga clic en **Aceptar**. Espere a que se complete la autenticación. El estado de la conexión del área de trabajo de DirectAccess ahora estará **conectado**.

12. En Internet Explorer, en la barra de direcciones, escriba **https://app1.corp.contoso.com/** y presione Entrar. Presione F5. Verás el sitio web IIS predeterminado en APP1.

13. En la barra de direcciones de Internet Explorer, escriba **https://app2.corp.contoso.com/** y presione Entrar. Presione F5. Verá el sitio web de IIS predeterminado en APP2.

14. En la pantalla **Inicio** , escriba <strong> \\ \app1\files</strong>y presione Entrar.

15. En la ventana carpetas compartidas de **archivos** , haga doble clic en el archivo de **Example.txt** . Verá el contenido del archivo de Example.txt.

16. En la pantalla **Inicio** , escriba <strong> \\ \app2\files</strong>y presione Entrar.

17. En la ventana carpetas compartidas de **archivos** , haga doble clic en el archivo de **Document.txtde texto nuevo** . Verá el contenido del nuevo texto Document.txt archivo.



