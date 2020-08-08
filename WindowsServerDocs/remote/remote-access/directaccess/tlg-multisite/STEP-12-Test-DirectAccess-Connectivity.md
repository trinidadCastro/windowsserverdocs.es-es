---
title: Paso 12 probar la conectividad de DirectAccess
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c8963265b6695cf50837216a8ffe0ae7d120e22c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969152"
---
# <a name="step-12-test-directaccess-connectivity"></a>Paso 12 probar la conectividad de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de que pueda probar la conectividad de los equipos cliente cuando se encuentran en redes de Internet o HomeNet, debe asegurarse de que tienen la configuración de directiva de grupo correcta.

- Para comprobar que los clientes tienen la Directiva de grupo correcta

- Probar la conectividad de DirectAccess desde Internet a través de EDGE1

- Traslado de cliente2 al grupo de seguridad de Win7_Clients_Site2

- Probar la conectividad de DirectAccess desde Internet a través de 2-EDGE1

## <a name="prerequisites"></a>Requisitos previos
Conecte ambos equipos cliente a la red CorpNet y, a continuación, reinicie los dos equipos cliente.

## <a name="verify-clients-have-the-correct-group-policy"></a><a name="policy"></a>Comprobar que los clientes tienen la Directiva de grupo correcta

1.  En CLIENT1, haga clic en **Inicio**, escriba **powershell.exe**, haga clic con el botón secundario en **PowerShell**, haga clic en **Opciones avanzadas**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

2.  En la ventana de Windows PowerShell, escriba **ipconfig** y presione Entrar.

    Asegúrese de que la dirección IPv4 del adaptador de CorpNet empiece por 10.0.0.

3.  En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione Entrar. Se muestran las entradas de la tabla de directivas de resolución de nombres (NRPT) para DirectAccess.

    -   . corp.contoso.com: esta configuración indica que todas las conexiones a corp.contoso.com deben resolverse mediante uno de los servidores DNS de DirectAccess, con la dirección IPv6 2001: db8:1:: 2 o 2001: db8:2:: 20.

    -   nls.corp.contoso.com: esta configuración indica que existe una exención para el nombre nls.corp.contoso.com.

4.  Deje abierta la ventana de Windows PowerShell para el procedimiento siguiente.

5.  En cliente2, haga clic en **Inicio**, en **todos los programas**, en **accesorios**, en **Windows PowerShell**, haga clic con el botón secundario en **Windows PowerShell**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

6.  En la ventana de Windows PowerShell, escriba **ipconfig** y presione Entrar.

    Asegúrese de que la dirección IPv4 del adaptador de CorpNet empiece por 10.0.0.

7.  En la ventana de Windows PowerShell, escriba **netsh namespace Show Policy** y presione Entrar.

    En la salida, debe haber dos secciones:

    -   . corp.contoso.com: esta configuración indica que todas las conexiones a corp.contoso.com deben resolverse mediante el servidor DNS de DirectAccess, con la dirección IPv6 2001: db8:1:: 2.

    -   nls.corp.contoso.com: esta configuración indica que existe una exención para el nombre nls.corp.contoso.com.

8.  Deje abierta la ventana de Windows PowerShell para el procedimiento siguiente.

## <a name="test-directaccess-connectivity-from-the-internet-through-edge1"></a><a name="EDGE1"></a>Probar la conectividad de DirectAccess desde Internet a través de EDGE1

1. Desconecte 2-EDGE1 de la red de Internet.

2. Desconecte CLIENT1 y cliente2 del conmutador CorpNet y conéctese al conmutador de Internet. Espere 30 segundos.

3. En CLIENT1, en la ventana de Windows PowerShell, escriba **ipconfig/all** y presione Entrar.

4. Examine la salida del comando ipconfig.

   El equipo cliente ya está conectado a Internet y tiene una dirección IPv4 pública. Cuando el cliente de DirectAccess tiene una dirección IPv4 pública, usa las tecnologías de transición Teredo o IPv6 de IP-HTTPS para Tunelizar los mensajes IPv6 a través de Internet IPv4 entre el cliente de DirectAccess y el servidor de acceso remoto. Tenga en cuenta que Teredo es la tecnología de transición preferida.

5. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar. Esto vacía las entradas de resolución de nombres que pueden existir en la memoria caché de DNS del cliente desde el momento en que el equipo cliente se conectó a la red corporativa.

6. Deshabilite la interfaz Teredo para asegurarse de que el equipo cliente usa IP-HTTPS para conectarse a CorpNet con el siguiente comando:

   ```
   netsh interface teredo set state disable
   ```

7. Asegúrese de que está conectado a través de EDGE1. Escriba **netsh interface httpstunnel show interfaces** y presione Entrar.

   La salida debe contener la dirección URL: https://edge1.contoso.com:443/IPHTTPS .

   > [!TIP]
   > En CLIENT1, también puede ejecutar el siguiente comando de Windows PowerShell: **Get-NetIPHTTPSConfiguration**. La salida muestra las conexiones de URL de servidor disponibles y el perfil activo actualmente.

8. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 asignada a APP1, que en este caso es 2001: db8:1:: 3.

9. En la ventana de Windows PowerShell, escriba **ping 2-app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 asignada a 2-APP1, que en este caso es 2001: db8:2:: 3.

10. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Debería ver las respuestas de la dirección NAT64 asignada por EDGE1 a APP2, que en este caso es FD**C9:9f4e: eb1b**: South:: A00:4. Tenga en cuenta que los valores en negrita variarán debido a cómo se genera la dirección.

    La capacidad de hacer ping APP2 es importante, porque la operación correcta indica que se ha podido establecer una conexión mediante NAT64/DNS64, ya que APP2 es un recurso solo IPv4.

11. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione Entrar. Verás el sitio web IIS predeterminado en APP1.

12. En la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione Entrar. Verá el sitio web predeterminado en 2-APP1.

13. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

14. En la pantalla **Inicio** , escriba<strong> \\ \2-App1\Files</strong>y, a continuación, presione Entrar. Haga doble clic en el archivo de texto de ejemplo.

    Esto demuestra que se pudo conectar al servidor de archivos en el dominio corp2.corp.contoso.com cuando se conecta a través de EDGE1.

15. En la pantalla **Inicio** , escriba<strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto.

    Esto demuestra que se pudo conectar a un servidor solo IPv4 mediante SMB para obtener un recurso en el dominio de recursos.

16. En la pantalla **Inicio** , escriba**WF. msc**y, a continuación, presione Entrar.

17. En la consola **firewall de Windows con seguridad avanzada** , observe que solo está activo el **perfil público** . El Firewall de Windows debe estar habilitado para que DirectAccess funcione correctamente. Si el Firewall de Windows está deshabilitado, la conectividad de DirectAccess no funcionará.

18. En el panel izquierdo de la consola, expanda el nodo **supervisión** y haga clic en el nodo **reglas de seguridad de conexión** . Debería ver las reglas de seguridad de conexión activas: **DirectAccess Policy-ClientToCorp**, **DirectAccess Policy-ClientToDNS64NAT64PrefixExemption**, **DirectAccess Policy-ClientToInfra**y **DirectAccess Policy-ClientToNlaExempt**. Desplácese por el panel central hacia la derecha para mostrar las columnas de los **métodos de primera autenticación** y **segunda autenticación** . Tenga en cuenta que la primera regla (ClientToCorp) usa Kerberos V5 para establecer el túnel de intranet y la tercera regla (ClientToInfra) utiliza NTLMv2 para establecer el túnel de infraestructura.

19. En el panel izquierdo de la consola, expanda el nodo **asociaciones de seguridad** y haga clic en el nodo **modo principal** . Observe las asociaciones de seguridad del túnel de infraestructura mediante NTLMv2 y la Asociación de seguridad del túnel de intranet con Kerberos V5. Haga clic con el botón secundario en la entrada que muestra **usuario (Kerberos V5)** como **método de segunda autenticación** y haga clic en **propiedades**. En la pestaña **General** , observe que el **segundo identificador local de autenticación** es **CORP\User1**, lo que indica que user1 pudo autenticarse correctamente en el dominio Corp con Kerberos.

20. Repita este procedimiento desde el paso 3 en cliente2.

## <a name="move-client2-to-the-win7_clients_site2-security-group"></a><a name="secgroup"></a>Traslado de cliente2 al grupo de seguridad de Win7_Clients_Site2

1.  En DC1, haga clic en **Inicio**, escriba **DSA. msc**y, a continuación, presione Entrar.

2.  En la consola de Active Directory usuarios y equipos, Abra **Corp.contoso.com/users** y haga doble clic en **Win7_Clients_Site1**.

3.  En el cuadro de diálogo **propiedades de Win7_Clients_Site1** , haga clic en la pestaña **miembros** , haga clic en **cliente2**, haga clic en **quitar**, haga clic en **sí**y, a continuación, haga clic en **Aceptar**.

4.  Haga doble clic en **Win7_Clients_Site2**y, a continuación, en el cuadro de diálogo **propiedades de Win7_Clients_Site2** , haga clic en la pestaña **miembros** .

5.  Haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, contactos, equipos o cuentas de servicio** , haga clic en **tipos de objetos**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.

6.  En **Escriba los nombres de objeto que desea seleccionar**, escriba **cliente2**y, a continuación, haga clic en **Aceptar**.

7.  Reinicie cliente2 e inicie sesión con la cuenta Corp/user1.

8.  En cliente2, abra una ventana de Windows PowerShell con privilegios elevados, escriba **netsh namespace Show Policy** y presione Entrar.

    En la salida, debe haber dos secciones:

    -   . corp.contoso.com: esta configuración indica que todas las conexiones a corp.contoso.com deben resolverse mediante el servidor DNS de DirectAccess, con la dirección IPv6 2001: db8:2:: 20.

    -   nls.corp.contoso.com: esta configuración indica que existe una exención para el nombre nls.corp.contoso.com.

## <a name="test-directaccess-connectivity-from-the-internet-through-2-edge1"></a><a name="DAConnect"></a>Probar la conectividad de DirectAccess desde Internet a través de 2-EDGE1

1. Conecte 2-EDGE1 a la red de Internet.

2. Desconecte EDGE1 de la red de Internet.

3. En CLIENT1, abra una ventana de Windows PowerShell con privilegios elevados.

4. En la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar. Esto vacía las entradas de resolución de nombres que pueden existir en la memoria caché de DNS del cliente desde el momento en que el equipo cliente se conectó a la red corporativa.

5. Asegúrese de que está conectado a través de 2-EDGE1. Escriba **netsh interface httpstunnel show interfaces** y presione Entrar.

   La salida debe contener la dirección URL: https://2-edge1.contoso.com:443/IPHTTPS .

   > [!TIP]
   > En CLIENT1, también puede ejecutar el siguiente comando: **Get-NetIPHTTPSConfiguration**. La salida muestra las conexiones de URL de servidor disponibles y el perfil activo actualmente.

   > [!NOTE]
   > CLIENT1 cambia automáticamente el servidor a través del cual se conecta a los recursos corporativos. Si la salida del comando muestra una conexión a EDGE1, espere aproximadamente cinco minutos y vuelva a intentarlo.

6. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 asignada a APP1, que en este caso es 2001: db8:1:: 3.

7. En la ventana de Windows PowerShell, escriba **ping 2-app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 asignada a 2-APP1, que en este caso es 2001: db8:2:: 3.

8. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Debería ver las respuestas de la dirección NAT64 asignada por EDGE1 a APP2, que en este caso es FD**C9:9f4e: eb1b**: South:: A00:4. Tenga en cuenta que los valores en negrita variarán debido a cómo se genera la dirección.

   La capacidad de hacer ping APP2 es importante, porque la operación correcta indica que se ha podido establecer una conexión mediante NAT64/DNS64, ya que APP2 es un recurso solo IPv4.

9. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione Entrar. Verás el sitio web IIS predeterminado en APP1.

10. En la barra de direcciones de Internet Explorer, escriba **https://2-app1/** y presione Entrar. Verás el sitio web predeterminado en APP2.

11. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verá el sitio web predeterminado en APP3.

12. En la pantalla **Inicio** , escriba<strong> \\ \App1\Files</strong>y, a continuación, presione Entrar. Haga doble clic en el archivo de texto de ejemplo.

    Esto demuestra que se pudo conectar al servidor de archivos en el dominio corp.contoso.com cuando se conecta a través de 2-EDGE1.

13. En la pantalla **Inicio** , escriba<strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto.

    Esto demuestra que se pudo conectar a un servidor solo IPv4 mediante SMB para obtener un recurso en el dominio de recursos.

14. Repita este procedimiento en el cliente2 del paso 3.



