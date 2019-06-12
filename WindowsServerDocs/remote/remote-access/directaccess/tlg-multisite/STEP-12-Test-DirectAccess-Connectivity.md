---
title: PASO 12 probar la conectividad de DirectAccess
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4e45f0c3c988c86a2428c3beb8bafc29b7b16bc0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446934"
---
# <a name="step-12-test-directaccess-connectivity"></a>PASO 12 probar la conectividad de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de que puede probar la conectividad de los equipos cliente cuando se encuentran en las redes de Internet o Homenet, debe asegurarse de que tienen la configuración de directiva de grupo adecuada.  
  
- Para comprobar que los clientes tienen la directiva de grupo correcta  
  
- Probar la conectividad de DirectAccess desde Internet a través de EDGE1  
  
- Mover al grupo de seguridad de Win7_Clients_Site2 CLIENT2  
  
- Probar la conectividad de DirectAccess desde Internet a través de 2-EDGE1  
  
## <a name="prerequisites"></a>Requisitos previos  
Conecte los dos equipos cliente a la red de la red corporativa y, a continuación, reinicie ambos equipos cliente.  
  
## <a name="policy"></a>Compruebe que los clientes tienen la directiva de grupo correcto  
  
1.  En CLIENTE1, haga clic en **iniciar**, tipo **powershell.exe**, haga clic en **powershell**, haga clic en **avanzadas**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la ventana de Windows PowerShell, escriba **ipconfig** y presione ENTRAR.  
  
    Asegúrese de que la dirección IPv4 de adaptador de red corporativa se inicia con 10.0.0.  
  
3.  En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione ENTRAR. Se muestran las entradas de la tabla de directivas de resolución de nombres (NRPT) para DirectAccess.  
  
    -   . corp.contoso.com-estas configuraciones indican que todas las conexiones a corp.contoso.com deben resolverse en uno de los servidores de DirectAccess DNS, con el 2001:db8:1::2 de dirección IPv6 o 2001:db8:2::20.  
  
    -   NLS.corp.contoso.com estos valores indican que hay una exención para el nombre nls.corp.contoso.com.  
  
4.  Deje la ventana de Windows PowerShell abierta para el siguiente procedimiento.  
  
5.  En CLIENT2, haga clic en **iniciar**, haga clic en **todos los programas**, haga clic en **Accesorios**, haga clic en **Windows PowerShell**, haga clic en **Windows PowerShell**y, a continuación, haga clic en **ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
6.  En la ventana de Windows PowerShell, escriba **ipconfig** y presione ENTRAR.  
  
    Asegúrese de que la dirección IPv4 de adaptador de red corporativa se inicia con 10.0.0.  
  
7.  En la ventana de Windows PowerShell, escriba **directiva se muestra el espacio de nombres de netsh** y presione ENTRAR.  
  
    En la salida, debería haber dos secciones:  
  
    -   . corp.contoso.com-estas configuraciones indican que todas las conexiones a corp.contoso.com deben resolverse por el servidor de DirectAccess DNS, con el 2001:db8:1::2 de dirección IPv6.  
  
    -   NLS.corp.contoso.com estos valores indican que hay una exención para el nombre nls.corp.contoso.com.  
  
8.  Deje la ventana de Windows PowerShell abierta para el siguiente procedimiento.  
  
## <a name="EDGE1"></a>Probar la conectividad de DirectAccess desde Internet a través de EDGE1  
  
1. Desconecte 2-EDGE1 desde la red de Internet.  
  
2. Desconecte CLIENT1 y CLIENT2 del conmutador de red corporativa y conectarlas al conmutador de Internet. Espere 30 segundos.  
  
3. En CLIENTE1, en la ventana de Windows PowerShell, escriba **ipconfig/all** y presione ENTRAR.  
  
4. Examine la salida del comando ipconfig.  
  
   El equipo cliente está conectado a Internet y tiene una dirección IPv4 pública. Cuando el cliente de DirectAccess tiene una dirección IPv4 pública, utiliza las tecnologías de transición Teredo o IP-HTTPS IPv6 para tunelizar los mensajes de IPv6 a través de una Internet IPv4 entre el cliente de DirectAccess y el servidor de acceso remoto. Tenga en cuenta que Teredo es la tecnología de transición preferido.  
  
5. En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR. Esto vacía entradas de resolución de nombres que todavía existan en la memoria caché del cliente DNS de cuando el equipo cliente se conectó a la red corporativa.  
  
6. Deshabilitar la interfaz Teredo para asegurarse de que el equipo cliente usa IP-HTTPS para conectarse a la red corporativa con el siguiente comando:  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Asegúrese de que está conectado a través de EDGE1. Tipo **httpstunnel de interfaz de netsh mostrar interfaces** y presione ENTRAR.  
  
   El resultado debe contener la dirección URL: https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > En CLIENTE1, también puede ejecutar el siguiente comando de Windows PowerShell: **Get-NetIPHTTPSConfiguration**. La salida muestra las conexiones de dirección URL del servidor disponibles y el perfil activo actualmente.  
  
8. En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 asignada a APP1, que en este caso es 2001:db8:1::3.  
  
9. En la ventana de Windows PowerShell, escriba **ping app1 de 2** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 asignada a 2-APP1, que en este caso es 2001:db8:2::3.  
  
10. En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Debería ver las respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fd**c9:9f4e:eb1b**: 7777::a00:4. Tenga en cuenta que los valores en negrita variará en cómo se genera la dirección.  
  
    La capacidad de hacer ping APP2 es importante, porque el éxito indica que haya podido establecer una conexión con NAT64/DNS64, APP2 sea un recurso solo IPv4.  
  
11. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione ENTRAR. Verás el sitio web IIS predeterminado en APP1.  
  
12. En la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione ENTRAR. Verá el sitio Web predeterminado en APP1 de 2.  
  
13. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
14. En el **iniciar** , escriba<strong>\\\2-App1\Files</strong>, y, a continuación, presione ENTRAR. Haga doble clic en el archivo de texto de ejemplo.  
  
    Esto demuestra que has sido capaz de conectarse al servidor de archivos en el dominio corp2.corp.contoso.com cuando se conecta a través de EDGE1.  
  
15. En el **iniciar** , escriba<strong>\\\App2\Files</strong>, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto.  
  
    Esto demuestra que has sido capaz de conectarse a un servidor solo IPv4 utilizando SMB para obtener un recurso en el dominio de recursos.  
  
16. En el **iniciar** , escriba**wf.msc**, y, a continuación, presione ENTRAR.  
  
17. En el **Firewall de Windows con seguridad avanzada** de consola, tenga en cuenta que solo el **perfil público** está activo. El Firewall de Windows debe habilitarse para que DirectAccess funcione correctamente. Si el Firewall de Windows está deshabilitado, no funciona la conectividad de DirectAccess.  
  
18. En el panel izquierdo de la consola, expanda el **supervisión** nodo y haga clic en el **reglas de seguridad de conexión** nodo. Debería ver las reglas de seguridad de la conexión activa: **DirectAccess Policy-ClientToCorp**, **ClientToDNS64NAT64PrefixExemption de directiva de DirectAccess**, **ClientToInfra de directiva de DirectAccess**, y **DirectAccess Policy-ClientToNlaExempt**. Desplácese hacia el panel central a la derecha para mostrar el **1 de los métodos de autenticación** y **2nd métodos de autenticación** columnas. Tenga en cuenta que la primera regla (ClientToCorp) utiliza Kerberos V5 para establecer el túnel de intranet y la tercera regla (ClientToInfra) utiliza NTLMv2 para establecer el túnel de infraestructura.  
  
19. En el panel izquierdo de la consola, expanda el **las asociaciones de seguridad** nodo y haga clic en el **modo principal** nodo. Tenga en cuenta las asociaciones de seguridad del túnel de infraestructura mediante NTLMv2 y la asociación de seguridad del túnel de intranet con Kerberos V5. Haga clic en la entrada que muestra **usuario (Kerberos V5)** como el **2 método de autenticación** y haga clic en **propiedades**. En el **General** pestaña, observe el **Id. Local de segunda autenticación** es **CORP\User1**, que indica que se puede autenticar correctamente al dominio CORP con User1 Kerberos.  
  
20. Repita este procedimiento en el paso 3 en CLIENT2.  
  
## <a name="secgroup"></a>Mover al grupo de seguridad de Win7_Clients_Site2 CLIENT2  
  
1.  En DC1, haga clic en **iniciar**, tipo **dsa.msc**, y, a continuación, presione ENTRAR.  
  
2.  En la consola usuarios y equipos de usuarios de Active Directory, abra **corp.contoso.com/Users** y haga doble clic en **Win7_Clients_Site1**.  
  
3.  En el **Win7_Clients_Site1 propiedades** cuadro de diálogo, haga clic en el **miembros** , haga clic **CLIENT2**, haga clic en **quitar**, haga clic en **Sí**y, a continuación, haga clic en **Aceptar**.  
  
4.  Haga doble clic en **Win7_Clients_Site2**y, a continuación, en el **Win7_Clients_Site2 propiedades** cuadro de diálogo, haga clic en el **miembros** ficha.  
  
5.  Haga clic en **agregar**y en el **Seleccionar usuarios, contactos, equipos o cuentas de servicio** cuadro de diálogo, haga clic en **tipos de objeto**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.  
  
6.  En **escriba los nombres de objeto para seleccionar**, tipo **CLIENT2**y, a continuación, haga clic en **Aceptar**.  
  
7.  Reinicie CLIENT2 e inicie sesión con la cuenta corp/User1.  
  
8.  En CLIENT2, abra una ventana de Windows PowerShell con privilegios elevados, escriba **directiva se muestra el espacio de nombres de netsh** y presione ENTRAR.  
  
    En la salida, debería haber dos secciones:  
  
    -   . corp.contoso.com-estas configuraciones indican que todas las conexiones a corp.contoso.com deben resolverse por el servidor de DirectAccess DNS, con el 2001:db8:2::20 de dirección IPv6.  
  
    -   NLS.corp.contoso.com estos valores indican que hay una exención para el nombre nls.corp.contoso.com.  
  
## <a name="DAConnect"></a>Probar la conectividad de DirectAccess desde Internet a través de 2-EDGE1  
  
1. Conectar EDGE1 de 2 a la red de Internet.  
  
2. Desconecte EDGE1 desde la red de Internet.  
  
3. En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados.  
  
4. En la ventana de Windows PowerShell, escriba **ipconfig /flushdns** y presione ENTRAR. Esto vacía entradas de resolución de nombres que todavía existan en la memoria caché del cliente DNS de cuando el equipo cliente se conectó a la red corporativa.  
  
5. Asegúrese de que está conectado a través de 2-EDGE1. Tipo **httpstunnel de interfaz de netsh mostrar interfaces** y presione ENTRAR.  
  
   El resultado debe contener la dirección URL: https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > En CLIENTE1, también puede ejecutar el comando siguiente: **Get-NetIPHTTPSConfiguration**. La salida muestra las conexiones de dirección URL del servidor disponibles y el perfil activo actualmente.  
  
   > [!NOTE]  
   > Client1 cambia automáticamente el servidor a través del cual se conecta a los recursos corporativos. Si la salida del comando muestra una conexión a EDGE1, espere aproximadamente cinco minutos y, a continuación, vuelva a intentarlo.  
  
6. En la ventana de Windows PowerShell, escriba **ping app1** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 asignada a APP1, que en este caso es 2001:db8:1::3.  
  
7. En la ventana de Windows PowerShell, escriba **ping app1 de 2** y presione ENTRAR. Deberías ver respuestas de la dirección IPv6 asignada a 2-APP1, que en este caso es 2001:db8:2::3.  
  
8. En la ventana de Windows PowerShell, escriba **ping app2** y presione ENTRAR. Debería ver las respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fd**c9:9f4e:eb1b**: 7777::a00:4. Tenga en cuenta que los valores en negrita variará en cómo se genera la dirección.  
  
   La capacidad de hacer ping APP2 es importante, porque el éxito indica que haya podido establecer una conexión con NAT64/DNS64, APP2 sea un recurso solo IPv4.  
  
9. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione ENTRAR. Verás el sitio web IIS predeterminado en APP1.  
  
10. En la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione ENTRAR. Verás el sitio web predeterminado en APP2.  
  
11. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione ENTRAR. Verá el sitio Web predeterminado en APP3.  
  
12. En el **iniciar** , escriba<strong>\\\App1\Files</strong>, y, a continuación, presione ENTRAR. Haga doble clic en el archivo de texto de ejemplo.  
  
    Esto demuestra que has sido capaz de conectarse al servidor de archivos en el dominio corp.contoso.com, cuando se conecta a través de 2-EDGE1.  
  
13. En el **iniciar** , escriba<strong>\\\App2\Files</strong>, y, a continuación, presione ENTRAR. Haz doble clic en el archivo Nuevo documento de texto.  
  
    Esto demuestra que has sido capaz de conectarse a un servidor solo IPv4 utilizando SMB para obtener un recurso en el dominio de recursos.  
  
14. Repita este procedimiento en CLIENT2 en el paso 3.  
  


