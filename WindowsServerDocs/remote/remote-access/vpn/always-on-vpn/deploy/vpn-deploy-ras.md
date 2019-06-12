---
title: Configurar el servidor de acceso remoto para VPN de Always On
description: RRAS está diseñado para realizar bien como un enrutador y un servidor de acceso remoto. por lo tanto, admite una amplia gama de características.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 3920f7f075f4742a62577ade809cc0494b05bf1f
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749439"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Paso 3. Configurar el servidor de acceso remoto para VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)
- [**Anterior:** Paso 4. Instalar y configurar el servidor de directivas de redes (NPS)](vpn-deploy-nps.md)

RRAS está diseñado para realizar bien como un enrutador y un servidor de acceso remoto, ya que admite una amplia gama de características. Para los fines de esta implementación, necesita solo un pequeño subconjunto de estas características: compatibilidad con conexiones VPN de IKEv2 y enrutamiento LAN.

IKEv2 es una protocolo que se describe en la solicitud de Internet Engineering Task Force para comentarios 7296 de túnel de VPN. La ventaja principal de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si se pierde temporalmente la conexión o si un usuario mueve un equipo cliente de una red a otra, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin intervención del usuario.

Configurar el servidor RRAS para admitir las conexiones IKEv2 al deshabilitar protocolos no utilizados, lo que reduce la superficie de seguridad del servidor. Además, configurar el servidor para asignar direcciones a los clientes VPN desde un grupo de direcciones estáticas. Factible, puede asignar direcciones desde un grupo o un servidor DHCP. Sin embargo, con un servidor DHCP agrega complejidad al diseño y ofrece beneficios mínima.

>[!IMPORTANT]
>Es importante:
>- Instalar a dos adaptadores de red Ethernet en el servidor físico. Si va a instalar al servidor VPN en una máquina virtual, debe crear dos conmutadores virtuales externos, uno para cada adaptador de red físico; y, a continuación, cree dos adaptadores de red virtual para la máquina virtual, con cada adaptador de red conectado a un conmutador virtual.
>
>- Instalar al servidor de la red perimetral entre el borde y firewalls internos, con un adaptador de red conectado a la red de perímetro externo y un adaptador de red conectado a la red de perímetro interno.

>[!WARNING]
>Antes de comenzar, asegúrese de habilitar IPv6 en el servidor VPN. En caso contrario, no se puede establecer una conexión y muestra un mensaje de error.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Instalar el acceso remoto como un servidor VPN de puerta de enlace RAS

En este procedimiento, se instala el rol de acceso remoto como un servidor VPN de puerta de enlace de RAS de inquilino único. Para obtener más información, consulta [Acceso remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Instalar el rol de acceso remoto mediante Windows PowerShell

1. Abra Windows PowerShell como **administrador**.

2. Escriba y ejecute el siguiente cmdlet:

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Una vez finalizada la instalación, aparece el mensaje siguiente en Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Instalar el rol de acceso remoto mediante el administrador del servidor

Puede usar el procedimiento siguiente para instalar el rol de acceso remoto mediante el administrador del servidor.

1. En el servidor VPN, en el administrador del servidor, seleccione **administrar** y seleccione **agregar Roles y características**.
   
   Se abre el Asistente para agregar roles y características.

2. En el antes de comenzar la página, seleccione **siguiente**.

3. En la página Seleccionar tipo de instalación, seleccione el **instalación basada en roles o basada en características** opción y seleccione **siguiente**.

4. En la página Seleccionar destino servidor, seleccione el **seleccionar un servidor del grupo de servidores** opción.

5. En el grupo de servidores, seleccione el equipo local y seleccione **siguiente**.

6. En el, seleccione la página roles de servidor, en **Roles**, seleccione **acceso remoto**, a continuación, **siguiente**.

7. En la página Selección de características, seleccione **siguiente**.

8. En la página de acceso remoto, seleccione **siguiente**.

9.  En la página de servicio de rol seleccione, en **servicios de rol**, seleccione **DirectAccess y VPN (RAS)** .

   El **agregar Roles y características Asistente** abre el cuadro de diálogo.

11. En el cuadro de diálogo Agregar Roles y características, seleccione **agregar características** , a continuación, seleccione **siguiente**.

12. En la página de rol de servidor Web (IIS), seleccione **siguiente**.

13. En la página Seleccionar rol Servicios, seleccione **siguiente**.

14. En la página Confirmar selecciones de instalación, revise las selecciones realizadas y luego seleccione **instalar**.

15. Una vez completada la instalación, seleccione **cerrar**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurar el acceso remoto como un servidor VPN

En esta sección, puede configurar una VPN de acceso remoto para permitir conexiones VPN de IKEv2, denegar las conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a la conexión de los clientes VPN autorizados.

1. En el servidor VPN, en el administrador del servidor, seleccione el **notificaciones** marca.

2. En el **tareas** menú, seleccione **abrir el Asistente para introducción**

   Abre el Asistente de configuración del acceso remoto.

   >[!NOTE]
   >El Asistente de configuración del acceso remoto podría abrir detrás de administrador del servidor. Si piensa que el Asistente está tardando demasiado tiempo en Abrir, mover o minimizar el administrador del servidor para averiguar si el Asistente está detrás de él. Si no es así, espere a que el Asistente inicializar.

3. Seleccione **solo implementar VPN**.

    Se abrirá Microsoft Management Console (MMC) de acceso remoto y de enrutamiento.

4. Haga clic en el servidor VPN y luego seleccione **configurar y habilitar enrutamiento y acceso remoto**.

   Abre el Asistente para la instalación del servidor de acceso remoto y de enrutamiento.

5. En la página para el enrutamiento y el Asistente para la instalación del servidor de acceso remoto, seleccione **siguiente**.

6. En **configuración**, seleccione **configuración personalizada**y, a continuación, seleccione **siguiente**.

7. En **configuración personalizada**, seleccione **acceso VPN**y, a continuación, seleccione **siguiente**.

   La finalización se abre el Asistente para la instalación del servidor de acceso remoto y de enrutamiento.

8. Seleccione **finalizar** para cerrar el asistente, a continuación, seleccione **Aceptar** para cerrar el cuadro de diálogo enrutamiento y acceso remoto.

9. Seleccione **iniciar servicio** para iniciar el acceso remoto.

10. En el acceso remoto de MMC, haga clic en el servidor VPN y luego seleccione **propiedades**.

11. En Propiedades, seleccione la **seguridad** y a hacer:

    a. Seleccione **proveedor de autenticación** y seleccione **autenticación RADIUS**.

    b. Seleccione **configurar**.

       Se abre el cuadro de diálogo de autenticación RADIUS.

    c. Seleccione **agregar**.

       Se abre el cuadro de diálogo Agregar servidor RADIUS.

    d. En **nombre del servidor**, escriba el nombre de dominio completo (FQDN) del servidor NPS de la red corporativa o de organización.
    
       Por ejemplo, si el nombre de NetBIOS del servidor NPS es NPS1 y el nombre de dominio es corp.contoso.com, escriba **NPS1.corp.contoso.com**.

    e. En **secreto compartido**, seleccione **cambio**.

       Se abre el cuadro de diálogo Cambiar secreto.

    f. En **secreto nuevo**, escriba una cadena de texto.

    g. En **confirmar el nuevo secreto**, escriba la misma cadena de texto y luego seleccione **Aceptar**.

    >[!IMPORTANT]
    >Guarde esta cadena de texto. Al configurar el servidor NPS de la red de la organización corporativo, agregará este servidor VPN como cliente RADIUS. Durante esa configuración, usará este mismo secreto compartido para que puedan comunicarse el NPS y los servidores VPN.

12. En **Agregar servidor RADIUS**, revise la configuración predeterminada para:

    - **Time-out**

    - **Puntuación inicial**

    - **Puerto**

13. Si es necesario, cambie los valores para que coincida con los requisitos de su entorno y seleccione **Aceptar**.

    Un servidor NAS es un dispositivo que proporciona cierto nivel de acceso a una red más grande. Un NAS que usa una infraestructura RADIUS también es un cliente RADIUS, enviar las solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para autenticación, autorización y contabilidad.

14. Revise la configuración de **proveedor de cuentas**:

    |                    Si desea que el...                     |                                                     En ese caso…                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Actividad de acceso remota registrada en el servidor de acceso remoto |                               Asegúrese de que **Windows Accounting** está seleccionada.                               |
    |        NPS para realizar servicios de administración de cuentas para VPN         | Cambio **proveedor de cuentas** a **cuentas RADIUS** y, a continuación, configure el NPS como el proveedor de cuentas. |

15. Seleccione el **IPv4** y a hacer:

    a. Seleccione **grupo de direcciones estáticas**.

    b. Seleccione **agregar** para configurar un grupo de direcciones IP.

       El grupo de direcciones estáticas debe contener las direcciones de la red de perímetro interno. Estas direcciones se encuentran en la conexión de red interna en el servidor VPN, no en la red corporativa.

    c. En **dirección IP inicial**, escriba la dirección IP inicial del intervalo que desea asignar a los clientes VPN.

    d. En **dirección IP final**, escriba la dirección IP final del intervalo que desea asignar a los clientes VPN o en **número de direcciones**, escriba el número de la dirección que desea que estén disponibles. Si usa DHCP para esta subred, asegúrese de configurar una exclusión de dirección correspondiente en los servidores DHCP.

    e. (Opcional) Si usa DHCP, seleccione **adaptador**y en la lista de resultados, seleccione el adaptador de Ethernet conectado a la red de perímetro interno.

16. (Opcional) *Si va a configurar el acceso condicional para la conectividad VPN*, desde el **certificado** desplegable lista, en **enlace de certificado SSL**, seleccione el servidor VPN autenticación.

17. (Opcional) *Si va a configurar el acceso condicional para la conectividad VPN*, en la MMC de NPS, expanda **directivas\\las directivas de red** y hacer: 

    a. El derecho **las conexiones al servidor de acceso remoto y de Microsoft Routing** directiva de red y seleccione **propiedades**.

    b. Seleccione el **conceder acceso. Conceder acceso si la solicitud de conexión coincide con esta directiva** opción.

    c. En el tipo de servidor de acceso de red, seleccione **el servidor de acceso remoto (VPN de acceso telefónico)** en la lista desplegable.

18. En el enrutamiento y acceso remoto de MMC, haga clic en **puertos,** y, a continuación, seleccione **propiedades**. 
    
    Se abre el cuadro de diálogo Propiedades de puertos.

19. Seleccione **minipuerto WAN (SSTP)** y seleccione **configurar**. Configurar dispositivo - abre el cuadro de diálogo de minipuerto WAN (SSTP).

    a. Desactive el **conexiones de acceso remoto (sólo de entrada)** y **conexiones de enrutamiento de marcado a petición (de entrada y salidas)** casillas de verificación.

    b. Seleccione **Aceptar**.

20. Seleccione **minipuerto WAN (L2TP)** y seleccione **configurar**. Configurar dispositivo - abre el cuadro de diálogo de minipuerto WAN (L2TP).

    a. En **número máximo de puertos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desee admitir.

    b. Seleccione **Aceptar**.

21. Seleccione **minipuerto WAN (PPTP)** y seleccione **configurar**. Configurar dispositivo - abre el cuadro de diálogo de minipuerto WAN (PPTP).

    a. En **número máximo de puertos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desee admitir.

    b. Seleccione **Aceptar**.

22. Seleccione **minipuerto WAN (IKEv2)** y seleccione **configurar**. Configurar dispositivo - abre el cuadro de diálogo de minipuerto WAN (IKEv2).

     a. En **número máximo de puertos**, escriba el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que desee admitir.

     b. Seleccione **Aceptar**.

23. Si se le solicite, seleccione **Sí** para confirmar reiniciar el servidor y seleccione **cerrar** para reiniciar el servidor.

## <a name="next-step"></a>Paso siguiente

[Paso 4. Instalar y configurar el servidor de directivas de redes (NPS)](vpn-deploy-nps.md): En este paso, se instala el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Server Manager agregar Roles y características Asistente. También configura NPS para controlar la autenticación, autorización y los derechos de administración de cuentas de conexión todos los solicitudes que recibe desde el servidor VPN.
