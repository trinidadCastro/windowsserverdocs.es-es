---
title: Solución de problemas en el servidor DHCP
description: Este artilce presenta cómo solucionar problemas en el servidor DHCP y recopilar datos.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 5ec2ef358cfaf7841b093843848f2ea5ee42433e
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181901"
---
# <a name="troubleshoot-problems-on-the-dhcp-server"></a>Solución de problemas en el servidor DHCP

En este artículo se describe cómo solucionar los problemas que se producen en el servidor DHCP.

## <a name="troubleshooting-checklist"></a>Lista de comprobación de solución de problemas

Utilice la siguiente configuración:

  - El servicio del servidor DHCP se ha iniciado y está en ejecución. Para comprobar esta configuración, ejecute el comando **net start** y busque **servidor DHCP**.

  - El servidor DHCP está autorizado. Consulte [autorización de servidor DHCP de Windows en escenario Unido](https://docs.microsoft.com/openspecs/windows_protocols/ms-dhcpe/56f8870b-a7c1-4db1-8a86-f69079fe5077)a un dominio.

  - Compruebe que las concesiones de direcciones IP están disponibles en el ámbito del servidor DHCP para la subred en la que se encuentra el cliente DHCP. Para ello, consulte la estadística del ámbito adecuado en la consola de administración del servidor DHCP.

  - Compruebe si \_ se puede encontrar una lista de direcciones erróneas en concesiones de direcciones.

  - Compruebe si los dispositivos de la red tienen direcciones IP estáticas que no se han excluido del ámbito DHCP.

  - Compruebe que la dirección IP a la que está enlazado el servidor DHCP está dentro de la subred de los ámbitos desde los que se deben conceder las direcciones IP. En caso de que no haya ningún agente de retransmisión disponible. Para ello, ejecute el cmdlet **Get-DhcpServerv4Binding** o **Get-DhcpServerv6Binding** .

  - Compruebe que solo el servidor DHCP está escuchando en el puerto UDP 67 y 68. Ningún otro proceso u otro servicio (como WDS o PXE) debe ocupar estos puertos. Para ello, ejecute el `netstat -anb` comando.

  - Compruebe que se ha agregado la exención IPsec del servidor DHCP Si está tratando con un entorno implementado por IPsec.

  - Compruebe que se puede hacer ping en la dirección IP del agente de retransmisión desde el servidor DHCP.

  - Enumerar y comprobar las directivas DHCP y los filtros configurados.

## <a name="event-logs"></a>Registros de eventos

Compruebe los registros de eventos del sistema y del servicio del servidor DHCP (**registros de aplicaciones y servicios** , \> **Microsoft** \> **Windows** \> **DHCP-Server**) en busca de problemas detectados relacionados con el problema observado.
En función del tipo de problema, se registra un evento en uno de los siguientes canales de eventos: [eventos de funcionamiento del servidor](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\))DHCP eventos administrativos del servidor DHCP eventos del sistema de servidor DHCP eventos de 
 [DHCP Server Administrative Events](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [DHCP Server System Events](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [notificación](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) 
 [DHCP Server Audit Events](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn800668\(v=ws.11\)) del servidor DHCP eventos de auditoría

## <a name="data-collection"></a>datos, recopilación

### <a name="dhcp-server-log"></a>Registro del servidor DHCP

Los registros de depuración del servicio del servidor DHCP proporcionan más información acerca de la asignación de concesión de direcciones IP y las actualizaciones dinámicas de DNS que realiza el servidor DHCP. Estos registros se encuentran de forma predeterminada en% WINDIR% \\ system32 \\ DHCP.
Para obtener más información, consulte [analizar los archivos de registro del servidor DHCP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd183591\(v=ws.10\)).

### <a name="network-trace"></a>Seguimiento de la red

Un seguimiento de red correlacionado puede indicar lo que el servidor DHCP estaba haciendo en el momento en que se registró el evento. Para crear este tipo de seguimiento, siga estos pasos:

1.  Vaya a [GitHub](https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS)y descargue el archivo [de \_tools.zipde TSS](https://github.com/CSS-Windows/WindowsDiag/blob/master/ALL/TSS/tss_tools.zip) .

2.  Copie el archivo detools.zip de TSS \_ y expándalo en una ubicación del disco local, por ejemplo, en la carpeta C: \\ Tools.

3.  Ejecute el siguiente comando desde C: \\ Tools en una ventana de símbolo del sistema con privilegios elevados:
    ```console
    TSS Ron Trace <Stop:Evt:>20321:<Other:>DhcpAdminEvents NoSDP NoPSR NoProcmon NoGPresult
    ```

    >[!Note]
    >En este comando, reemplace \<*Stop:Evt:*\> y \<*Other:*\> por el identificador del evento y el canal del evento en el que se va a centrar en la sesión de seguimiento.
    >El \_ archivo Léame de TSS. cmd \_Help.docx archivos contenidos en el archivo detools.zip de TSS \_ proporciona más información acerca de toda la configuración disponible.

4.  Una vez que se desencadena el evento, la herramienta crea una carpeta denominada C: \\ MS \_ Data. Esta carpeta contendrá algunos archivos de salida útiles que proporcionan información general acerca de la configuración de red y dominio del equipo.
    El archivo más interesante en esta carpeta es% COMPUTERNAME% \_ Date \_ Time \_ packetcapture \_ InternetClient \_ dbg. ETL.
    Mediante el uso de la aplicación [monitor de red](https://www.microsoft.com/download/4865) , puede cargar el archivo y establecer el filtro de presentación en el protocolo "DHCP o DNS" para examinar lo que sucede en segundo plano.
