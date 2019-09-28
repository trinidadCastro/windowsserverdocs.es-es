---
title: Configurar el servidor de acceso remoto para VPN de Always On
description: RRAS está diseñado para funcionar bien como un enrutador y un servidor de acceso remoto; por lo tanto, admite una amplia gama de características.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: c04074338cf4ba0189eb1e9bc45a80b948fdbfbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388045"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Paso 3. Configurar el servidor de acceso remoto para VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Previo** Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)
- [**Previo** Paso 4. Instalación y configuración del servidor de directivas de redes (NPS)](vpn-deploy-nps.md)

RRAS está diseñado para funcionar bien como un enrutador y un servidor de acceso remoto, ya que admite una amplia gama de características. Para los fines de esta implementación, solo necesita un pequeño subconjunto de estas características: compatibilidad con conexiones VPN de IKEv2 y enrutamiento de LAN.

IKEv2 es un protocolo de túnel VPN que se describe en solicitud de fuerza de la tarea de ingeniería de Internet para los comentarios 7296. La principal ventaja de IKEv2 es que tolera las interrupciones en la conexión de red subyacente. Por ejemplo, si la conexión se pierde temporalmente o si un usuario mueve un equipo cliente de una red a otra, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin la intervención del usuario.

Configure el servidor RRAS para que admita conexiones IKEv2 al deshabilitar los protocolos no utilizados, lo que reduce la superficie de seguridad del servidor. Además, configure el servidor para asignar direcciones a los clientes VPN desde un grupo de direcciones estáticas. Puede asignar direcciones de forma factible desde un grupo o un servidor DHCP. sin embargo, el uso de un servidor DHCP agrega complejidad al diseño y ofrece ventajas mínimas.

>[!IMPORTANT]
>Es importante:
>- Instale dos adaptadores de red Ethernet en el servidor físico. Si va a instalar el servidor VPN en una máquina virtual, debe crear dos conmutadores virtuales externos, uno para cada adaptador de red físico. y, a continuación, cree dos adaptadores de red virtual para la máquina virtual, con cada adaptador de red conectado a un conmutador virtual.
>
>- Instale el servidor en la red perimetral entre el perímetro y los firewalls internos, con un adaptador de red conectado a la red perimetral externa y un adaptador de red conectado a la red perimetral interna.

>[!WARNING]
>Antes de empezar, asegúrese de habilitar IPv6 en el servidor VPN. De lo contrario, no se puede establecer una conexión y se muestra un mensaje de error.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Instalar el acceso remoto como un servidor VPN de puerta de enlace RAS

En este procedimiento, instalará el rol de acceso remoto como un servidor VPN de puerta de enlace RAS de un solo inquilino. Para obtener más información, consulta [Acceso remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Instalar el rol de acceso remoto mediante Windows PowerShell

1. Abra Windows PowerShell como **Administrador**.

2. Escriba y ejecute el siguiente cmdlet:

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Una vez finalizada la instalación, aparece el siguiente mensaje en Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Instale el rol de acceso remoto mediante Administrador del servidor

Puede usar el siguiente procedimiento para instalar el rol de acceso remoto mediante Administrador del servidor.

1. En el servidor VPN, en Administrador del servidor, seleccione **administrar** y, a continuación, **Agregar roles y características**.
   
   Se abre el Asistente para agregar roles y características.

2. En la página antes de comenzar, seleccione **siguiente**.

3. En la página Seleccionar tipo de instalación, seleccione la opción de instalación basada en **características o en roles** y seleccione **siguiente**.

4. En la página Seleccionar servidor de destino, seleccione la opción **seleccionar un servidor del grupo de servidores** .

5. En grupo de servidores, seleccione el equipo local y seleccione **siguiente**.

6. En la página Seleccionar roles de servidor, en **roles**, seleccione **acceso remoto**y, a continuación, **siguiente**.

7. En la página seleccionar características, seleccione **siguiente**.

8. En la página acceso remoto, seleccione **siguiente**.

9.  En la página seleccionar servicio de rol, en **servicios de rol**, seleccione **DirectAccess y VPN (RAS)** .

   Se abre el cuadro de diálogo **Asistente para agregar roles y características** .

11. En el cuadro de diálogo Agregar roles y características, seleccione **Agregar características** y, a continuación, seleccione **siguiente**.

12. En la página rol de servidor Web (IIS), seleccione **siguiente**.

13. En la página seleccionar servicios de rol, seleccione **siguiente**.

14. En la página confirmar selecciones de instalación, revise las opciones seleccionadas y seleccione **instalar**.

15. Una vez completada la instalación, seleccione **cerrar**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurar el acceso remoto como un servidor VPN

En esta sección, puede configurar VPN de acceso remoto para permitir conexiones VPN IKEv2, denegar conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a los clientes VPN autorizados.

1. En el servidor VPN, en Administrador del servidor, seleccione la marca **notificaciones** .

2. En el menú **tareas** , seleccione **abrir el Asistente para introducción**

   Se abre el Asistente para configurar el acceso remoto.

   >[!NOTE]
   >El Asistente para configurar el acceso remoto podría abrirse detrás de Administrador del servidor. Si cree que el asistente está tardando demasiado tiempo en abrirse, mueva o minimice Administrador del servidor para averiguar si el asistente está detrás. Si no es así, espere a que se inicialice el asistente.

3. Seleccione **implementar solo VPN**.

    Se abre Microsoft Management Console (MMC) de enrutamiento y acceso remoto.

4. Haga clic con el botón secundario en el servidor VPN y seleccione **configurar y Habilitar enrutamiento y acceso remoto**.

   Se abre el Asistente para la instalación del servidor de enrutamiento y acceso remoto.

5. En el Asistente para la instalación del servidor de enrutamiento y acceso remoto, seleccione **siguiente**.

6. En **configuración**, seleccione **Configuración personalizada**y, a continuación, seleccione **siguiente**.

7. En **Configuración personalizada**, seleccione **acceso VPN**y, a continuación, seleccione **siguiente**.

   Se abre el Asistente para la instalación del servidor de enrutamiento y acceso remoto.

8. Seleccione **Finalizar** para cerrar el asistente y, después, haga clic en **Aceptar** para cerrar el cuadro de diálogo enrutamiento y acceso remoto.

9. Seleccione **Iniciar servicio** para iniciar el acceso remoto.

10. En el MMC de acceso remoto, haga clic con el botón secundario en el servidor VPN y seleccione **propiedades**.

11. En propiedades, seleccione la pestaña **seguridad** y haga lo siguiente:

    a. Seleccione **proveedor de autenticación** y seleccione **autenticación RADIUS**.

    b. Seleccione **configurar**.

       Se abre el cuadro de diálogo autenticación RADIUS.

    c. Seleccione **Agregar**.

       Se abre el cuadro de diálogo Agregar servidor RADIUS.

    d. En **nombre del servidor**, escriba el nombre de dominio completo (FQDN) del servidor NPS de la organización o red corporativa.
    
       Por ejemplo, si el nombre NetBIOS del servidor NPS es NPS1 y el nombre de dominio es corp.contoso.com, escriba **NPS1.Corp.contoso.com**.

    e. En **secreto compartido**, seleccione **cambiar**.

       Se abre el cuadro de diálogo cambiar secreto.

    f. En **nuevo secreto**, escriba una cadena de texto.

    g. En **confirmar nuevo secreto**, escriba la misma cadena de texto y luego seleccione **Aceptar**.

    >[!IMPORTANT]
    >Guarde esta cadena de texto. Al configurar el servidor NPS en la organización o la red corporativa, agregará este servidor VPN como cliente RADIUS. Durante esa configuración, usará este mismo secreto compartido para que los servidores NPS y VPN puedan comunicarse.

12. En **Agregar servidor RADIUS**, revise la configuración predeterminada de:

    - **Tiempo de espera**

    - **Puntuación inicial**

    - **Casilla**

13. Si es necesario, cambie los valores para que coincidan con los requisitos de su entorno y seleccione **Aceptar**.

    Un NAS es un dispositivo que proporciona cierto nivel de acceso a una red de mayor tamaño. Un NAS que usa una infraestructura de RADIUS también es un cliente RADIUS, que envía solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para la autenticación, la autorización y las cuentas.

14. Revise la configuración del **proveedor de cuentas**:

    |                    Si desea...                     |                                                     En ese caso…                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Actividad de acceso remoto registrada en el servidor de acceso remoto |                               Asegúrese de que la opción **contabilidad de Windows** está seleccionada.                               |
    |        NPS para realizar servicios de contabilidad para VPN         | Cambie el **proveedor de cuentas** a **cuentas RADIUS** y, a continuación, configure el NPS como proveedor de cuentas. |

15. Seleccione la pestaña **IPv4** y haga lo siguiente:

    a. Seleccione **grupo de direcciones estáticas**.

    b. Seleccione **Agregar** para configurar un grupo de direcciones IP.

       El grupo de direcciones estáticas debe contener direcciones de la red perimetral interna. Estas direcciones se encuentran en la conexión de red interna en el servidor VPN, no en la red corporativa.

    c. En **dirección IP inicial**, escriba la dirección IP inicial del intervalo que desea asignar a los clientes VPN.

    d. En **dirección IP final**, escriba la dirección IP final en el intervalo que desea asignar a los clientes VPN o, en **número de direcciones**, escriba el número de la dirección que desea que esté disponible. Si utiliza DHCP para esta subred, asegúrese de configurar una exclusión de direcciones correspondiente en los servidores DHCP.

    e. Opta Si usa DHCP, seleccione **adaptador**y, en la lista de resultados, seleccione el adaptador Ethernet conectado a la red perimetral interna.

16. Opta *Si está configurando el acceso condicional para la conectividad VPN*, en la lista desplegable **certificado** , en **enlace de certificado SSL**, seleccione la autenticación del servidor VPN.

17. Opta *Si está configurando el acceso condicional para la conectividad VPN*, en el MMC de NPS, expanda **directivas\\directivas de red** y haga lo siguiente: 

    a. Derecho: conexiones a la Directiva de red del **servidor de enrutamiento y acceso remoto de Microsoft** y seleccione **propiedades**.

    b. **Seleccione conceder acceso. Conceda acceso si la solicitud de conexión coincide** con esta opción de directiva.

    c. En tipo de servidor de acceso a la red, seleccione **servidor de acceso remoto (VPN, acceso telefónico)** en la lista desplegable.

18. En MMC de enrutamiento y acceso remoto, haga clic con el botón secundario en **puertos** y seleccione **propiedades**. 
    
    Se abre el cuadro de diálogo Propiedades de puertos.

19. Seleccione **minipuerto WAN (SSTP)** y haga clic en **configurar**. Se abre el cuadro de diálogo Configurar dispositivo-WAN Miniport (SSTP).

    a. Desactive las casillas **conexiones de acceso remoto (sólo de entrada)** y **enrutamiento de marcado a petición (entrante y saliente)** .

    b. Seleccione **Aceptar**.

20. Seleccione **minipuerto WAN (L2TP)** y haga clic en **configurar**. Se abre el cuadro de diálogo Configurar minipuerto de dispositivo-WAN (L2TP).

    a. En **puertos máximos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desea admitir.

    b. Seleccione **Aceptar**.

21. Seleccione **minipuerto WAN (PPTP)** y haga clic en **configurar**. Se abre el cuadro de diálogo Configurar minipuerto de dispositivo-WAN (PPTP).

    a. En **puertos máximos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desea admitir.

    b. Seleccione **Aceptar**.

22. Seleccione **minipuerto WAN (IKEv2)** y haga clic en **configurar**. Se abre el cuadro de diálogo Configurar minipuerto de dispositivo-WAN (IKEv2).

     a. En **puertos máximos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desea admitir.

     b. Seleccione **Aceptar**.

23. Si se le solicita, seleccione **sí** para confirmar el reinicio del servidor y seleccione **cerrar** para reiniciar el servidor.

## <a name="next-step"></a>Paso siguiente

[Paso 4. Instale y configure el servidor de directivas de redes](vpn-deploy-nps.md)(NPS): En este paso, instalará Administrador del servidor el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Asistente para agregar roles y características. También puede configurar NPS para que controle todas las tareas de autenticación, autorización y contabilidad para las solicitudes de conexión que recibe desde el servidor VPN.
