---
title: Paso 5 probar la conectividad de DirectAccess desde Internet y a través del clúster
description: Obtenga información sobre cómo probar la conectividad de DirectAccess desde Internet y a través del clúster.
manager: brianlic
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e301c9786b0d76e45c8417a7a3c9340937f5f529
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039215"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>Paso 5 probar la conectividad de DirectAccess desde Internet y a través del clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

CLIENT1 ya está listo para las pruebas de DirectAccess.

- Pruebe la conectividad de DirectAccess desde Internet. Conecte CLIENT1 a la red Internet simulada. Cuando se conecta a la red Internet simulada, se asignan direcciones IPv4 públicas al cliente. Cuando a un cliente de DirectAccess se le asigna una dirección IPv4 pública, intenta establecer una conexión con el servidor de acceso remoto mediante una tecnología de transición IPv6.

- Pruebe la Conectividad del cliente de DirectAccess a través del clúster. Probar la funcionalidad del clúster. Antes de comenzar con las pruebas, se recomienda que apague tanto EDGE1 como EDGE2 durante al menos cinco minutos. Hay varias razones para ello, que incluyen los tiempos de espera de la caché ARP y los cambios relacionados con NLB. Al validar la configuración de NLB en un laboratorio de pruebas, tendrá que ser paciente, ya que los cambios en la configuración no se reflejarán inmediatamente en la conectividad hasta que haya transcurrido un período de tiempo. Es importante tenerlo en cuenta al realizar las siguientes tareas.

    > [!TIP]
    > Se recomienda borrar la memoria caché de Internet Explorer antes de llevar a cabo este procedimiento y cada vez que pruebe la conexión a través de un servidor de acceso remoto diferente para asegurarse de que está probando la conexión y no recuperando las páginas web de la memoria caché.

## <a name="test-directaccess-connectivity-from-the-internet"></a>Probar la conectividad de DirectAccess desde Internet

1. Desconecte CLIENT1 del conmutador CorpNet y conéctelo al conmutador de Internet. Espere 30 segundos.

2. En una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig/flushdns** y presione Entrar. Esto vacía las entradas de resolución de nombres que pueden existir en la memoria caché de DNS del cliente desde el momento en que el equipo cliente se conectó a la red corporativa.

3. En la ventana de Windows PowerShell, escriba **Get-DnsClientNrptPolicy** y presione Entrar.

   El resultado muestra la configuración actual de la tabla de directivas de resolución de nombres (NRPT). Esta configuración indica que todas las conexiones a. corp.contoso.com deben resolverse mediante el servidor DNS de acceso remoto, con la dirección IPv6 2001: db8:1:: 2. Asimismo, fíjate en la entrada NRPT que indica que existe una exención para el nombre nls.corp.contoso.com; los nombres de la lista de exenciones no reciben respuesta del servidor DNS de acceso remoto. Puede hacer ping a la dirección IP del servidor DNS de acceso remoto para confirmar la conectividad con el servidor de acceso remoto. por ejemplo, puede hacer ping a 2001: db8:1:: 2.

4. En la ventana de Windows PowerShell, escriba **ping app1** y presione Entrar. Debería ver las respuestas de la dirección IPv6 de APP1, que en este caso es 2001: db8:1:: 3.

5. En la ventana de Windows PowerShell, escriba **ping App2** y presione Entrar. Deberías ver respuestas de la dirección NAT64 que EDGE1 asigna a APP2, que en este caso es fdc9:9f4e:eb1b:7777::a00:4.

   La capacidad de hacer ping APP2 es importante, porque la operación correcta indica que se ha podido establecer una conexión mediante NAT64/DNS64, ya que APP2 es un recurso solo IPv4.

6. Deje abierta la ventana de Windows PowerShell para el procedimiento siguiente.

7. Abra Internet Explorer, en la barra de direcciones de Internet Explorer, escriba **https://app1/** y presione Entrar. Verás el sitio web IIS predeterminado en APP1.

8. En la barra de direcciones de Internet Explorer, escriba **https://app2/** y presione Entrar. Verás el sitio web predeterminado en APP2.

9. En la pantalla **Inicio** , escriba <strong> \\ \App2\Files</strong>y, a continuación, presione Entrar. Haz doble clic en el archivo Nuevo documento de texto.

    Esto demuestra que se pudo conectar a un servidor solo IPv4 mediante SMB para obtener un recurso en el dominio de recursos.

10. En la pantalla **Inicio** , escriba **WF. msc** y, a continuación, presione Entrar.

11. En la consola **firewall de Windows con seguridad avanzada** , observe que solo el perfil **privado** o **público** está activo. El Firewall de Windows debe estar habilitado para que DirectAccess funcione correctamente. Si el Firewall de Windows está deshabilitado, la conectividad de DirectAccess no funcionará.

12. En el panel izquierdo de la consola, expanda el nodo **supervisión** y haga clic en el nodo **reglas de seguridad de conexión** . Debería ver las reglas de seguridad de conexión activas: **DirectAccess Policy-ClientToCorp**, **DirectAccess Policy-ClientToDNS64NAT64PrefixExemption**, **DirectAccess Policy-ClientToInfra** y **DirectAccess Policy-ClientToNlaExempt**. Desplácese por el panel central hacia la derecha para mostrar las columnas de los **métodos de primera autenticación** y **segunda autenticación** . Tenga en cuenta que la primera regla (ClientToCorp) usa Kerberos V5 para establecer el túnel de intranet y la tercera regla (ClientToInfra) utiliza NTLMv2 para establecer el túnel de infraestructura.

13. En el panel izquierdo de la consola, expanda el nodo **asociaciones de seguridad** y haga clic en el nodo **modo principal** . Observe las asociaciones de seguridad del túnel de infraestructura mediante NTLMv2 y la Asociación de seguridad del túnel de intranet con Kerberos V5. Haga clic con el botón secundario en la entrada que muestra **usuario (Kerberos V5)** como **método de segunda autenticación** y haga clic en **propiedades**. En la pestaña **General** , observe que el **segundo identificador local de autenticación** es **CORP\User1**, lo que indica que user1 pudo autenticarse correctamente en el dominio Corp con Kerberos.

## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Probar la Conectividad del cliente de DirectAccess a través del clúster

1. Realice un apagado estable en EDGE2.

   Puede usar el administrador de equilibrio de carga de red para ver el estado de los servidores al ejecutar estas pruebas.

2. En CLIENT1, en la ventana de Windows PowerShell, escriba **ipconfig/flushdns** y presione Entrar. Esto vacía las entradas de resolución de nombres que pueden existir en la memoria caché de DNS del cliente.

3. En la ventana de Windows PowerShell, haga ping a APP1 y APP2. Debe recibir las respuestas de ambos recursos.

4. En la pantalla **Inicio** , escriba <strong> \\ \app2\files</strong>. Debería ver la carpeta compartida en el equipo APP2. La capacidad de abrir el recurso compartido de archivos en APP2 indica que el segundo túnel, que requiere la autenticación Kerberos para el usuario, funciona correctamente.

5. Abra Internet Explorer y, a continuación, abra los sitios Web https://app1/ y https://app2/ . La capacidad de abrir ambos sitios web confirma que los túneles primero y segundo están en funcionamiento. Cierre Internet Explorer.

6. Inicie el equipo EDGE2.

7. En EDGE1, realice un cierre estable.

8. Espere 5 minutos y vuelva a CLIENT1. Realice los pasos 2-5. Esto confirma que CLIENT1 pudo conmutar por error de forma transparente a EDGE2 después de que EDGE1 deja de estar disponible.
