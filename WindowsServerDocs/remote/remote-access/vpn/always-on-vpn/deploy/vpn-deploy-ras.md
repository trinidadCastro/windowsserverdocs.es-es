---
title: Configurar el servidor de acceso remoto para VPN de Always On
description: RRAS está diseñado para funcionar bien como un enrutador y un servidor de acceso remoto. por lo tanto, admite una amplia variedad de características.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: a338ddfec1ed5cd0e9198f64dc4952eb591cdc1b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067519"
---
# Paso 3. Configurar el servidor de acceso remoto para VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 2. Configurar la infraestructura de servidor](vpn-deploy-server-infrastructure.md)<br>
& #187;  [ **Anterior:** paso 4. Instalar y configurar el servidor de directivas de redes (NPS)](vpn-deploy-nps.md)


RRAS está diseñado para realizar así como un enrutador y un servidor de acceso remoto porque admite una amplia variedad de características. Para los fines de esta implementación, se requiere solo un pequeño subconjunto de estas características: compatibilidad con conexiones VPN IKEv2 y el enrutamiento de LAN.

IKEv2 es una VPN se describe en la solicitud de grupo de trabajo de ingeniería de Internet para 7296 de comentarios de protocolo de túnel. La principal ventaja de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si se pierde temporalmente la conexión o si un usuario desplaza un equipo cliente desde una red a otro, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin intervención del usuario.

Configurar el servidor RRAS para admitir conexiones IKEv2 mientras deshabilitar protocolos no utilizadas, lo que reduce la superficie de seguridad del servidor. Además, configurar el servidor para asignar direcciones a los clientes VPN de un grupo de dirección estática. Factible puede asignar direcciones de un grupo o un servidor DHCP. Sin embargo, con un servidor DHCP agrega complejidad en el diseño y ofrece beneficios mínimas.


>[!Important]
>Es importante:
>- Instalar a dos adaptadores de red Ethernet en el servidor físico. Si vas a instalar al servidor VPN en una máquina virtual, debes crear dos modificadores virtuales externos, uno para cada adaptador de red física; y, a continuación, crear dos adaptadores de red virtual para la máquina virtual, a cada adaptador de red conectada a un conmutador virtual.
>
>- Instalar al servidor de la red perimetral entre el borde y firewalls internos, con un adaptador de red conectado a la red perimetral externa y un adaptador de red conectado a la red perimetral interna.


>[!Warning]
>Antes de comenzar, asegúrate de habilitar IPv6 en el servidor VPN. De lo contrario, no se puede establecer una conexión y muestra un mensaje de error.

## Instalar el acceso remoto como un servidor VPN de puerta de enlace RAS

En este procedimiento, puedes instalar el rol de acceso remoto como un servidor de VPN de puerta de enlace de RAS solo inquilino. Para obtener más información, consulta [Acceso remoto](../../../Remote-Access.md).


### Instalar el rol de acceso remoto con Windows PowerShell

1. Abre Windows PowerShell como **Administrador**.

2. Escribe el siguiente comando y presiona **ENTRAR**:

   `Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools`

   Una vez completada la instalación, aparece el mensaje siguiente en Windows PowerShell.
    
   | Correcto | Reinicio necesario | Código de salida | Resultado de la característica                             |
   |---------|----------------|-----------|--------------------------------------------|
   | Verdadero    | No             | Correcto   | {Kit de administración de Connection Manager de RAS |
   ---

### Instalar el rol de acceso remoto mediante el administrador del servidor

Puedes usar el siguiente procedimiento para instalar el rol de acceso remoto con el administrador del servidor.

1.  En el servidor VPN, en el Administrador de servidores, haga clic en **Administrar** y haz clic en **Agregar Roles y características**. <p>Abre el agregar Roles and Features Wizard.

2.  En el antes de empezar a la página, haz clic en**siguiente**.

3.  En la página Seleccionar el tipo de instalación, seleccione la opción de **instalación basado en roles o basada en características** y haz clic en **siguiente**.

4.  En la página de servidor Select destination, selecciona la opción de **Seleccionar un servidor de grupo de servidores** .

5.  En el grupo de servidores, selecciona el equipo local y haga clic en **siguiente**.

6.  En la página de roles de servidor selecciona en **Roles**, haz clic en el **Acceso remoto**y, a continuación, **siguiente**.

7.  En la página Selección de características, haz clic en **siguiente**.

8.  En la página de acceso remoto, haz clic en **siguiente**.

9.  En la página de servicio de rol selecciona en los **Servicios de rol**, haga clic en**DirectAccess y VPN (RAS)**.<p>Se abrirá el cuadro de diálogo **Agregar Roles and Features Wizard** .

10. En la agregar Roles y el cuadro de diálogo de características, haz clic en **Agregar características** y haz clic en **siguiente**.

11. En la página de rol de servidor Web (IIS), haz clic en **siguiente**.

12. En la página de servicios de rol de selección, haz clic en **siguiente**.

13. En la página de las selecciones de instalación de confirmar, revisar las opciones y haga clic en **instalar**.

14. Una vez completada la instalación, haz clic en **Cerrar**.

## Configurar el acceso remoto como un servidor VPN

En esta sección, puedes configurar VPN de acceso remoto para permitir conexiones VPN IKEv2, denegar conexiones desde otros protocolos VPN y asignar un conjunto de direcciones IP estáticas para la emisión de direcciones IP a los clientes VPN autorizados que se conectan.

1.  En el servidor VPN, en el Administrador de servidores, haz clic en la marca de **notificaciones** .

2.  En el menú de **tareas** , haz clic en **Abrir el Asistente de inicio**.<p>Abre el Asistente de configuración de acceso remoto. 

    >[!NOTE] 
    >El Asistente para la configuración de acceso remoto podría abrir detrás del Administrador de servidores. Si crees que tarda demasiado tiempo para abrir el asistente, mover o minimizar el Administrador de servidores para saber si el Asistente está detrás de ella. Si no es así, espera a que el Asistente inicializar.

3.  Haz clic en **implementar una VPN solo**.<p>Abre el enrutamiento y Microsoft Management Console (MMC) de acceso remoto.

4.  Haz clic en el servidor VPN y haga clic en **configurar y habilitar el enrutamiento y acceso remoto**.<p>Abre el Asistente para instalación de servidor de acceso remoto y de enrutamiento.

5.  En la bienvenida al enrutamiento y el Asistente para instalación de servidor de acceso remoto, haz clic en **siguiente**.

6.  En **configuración**, haz clic en la **Configuración personalizada**y, a continuación, haz clic en **siguiente**.

7.  En **Configuración personalizada**, haz clic en el **acceso a la VPN**y, a continuación, haz clic en **siguiente**.<p>La finalización se abrirá el Asistente para instalación de servidor de acceso remoto y de enrutamiento.

8.  Haga clic en **Finalizar** para cerrar al asistente y haz clic en **Aceptar** para cerrar el cuadro de diálogo de enrutamiento y acceso remoto.

9.  Haz clic en **iniciar el servicio** para iniciar el acceso remoto.

10. En la consola de MMC de acceso remoto, haz clic en el servidor VPN y haga clic en **Propiedades**.

11. En las propiedades, haz clic en la pestaña de **seguridad** y realice:

    a. Haga clic en el **proveedor de autenticación** y **Autenticación RADIUS**.
    
    b. Haz clic en **Configurar**.<p>Se abrirá el cuadro de diálogo de autenticación RADIUS.
    
    c. Haz clic en **Agregar**.<p>Abre el cuadro de diálogo Agregar servidor RADIUS.
    
    d. En el **nombre del servidor**, escribe el nombre de dominio completo (FQDN) del servidor NPS en la red corporativa o de organización.<p>Por ejemplo, si el nombre NetBIOS de tu servidor NPS es NPS1 y el nombre de dominio es corp.contoso.com, escriba **NPS1.corp.contoso.com**.
    
    e. En **secreto compartido**, haz clic en el **cambio**.<p>Abre el cuadro de diálogo Cambiar secreto.
    
    f. En el **nuevo secreto**, escribe una cadena de texto.
    
    g. **Confirmar el nuevo secreto**, escriba la misma cadena de texto y haz clic en **Aceptar**.

    >[!IMPORTANT] 
    >Guarda esta cadena de texto. Al configurar el servidor NPS en la red corporativa o de organización, agregarás este servidor VPN como un cliente de radio. Durante la configuración de control, usarás este mismo secreto compartido para que puedan comunicar la NPS y los servidores VPN.

12. **Agregar servidor RADIUS**, revisa la configuración predeterminada para:

    - **Tiempo de espera**
    
    - **Puntuación inicial**
    
    - **Port**

13. Si es necesario, cambiar los valores para que coincida con los requisitos para el entorno y haz clic en **Aceptar**.<p>Un NAS es un dispositivo que proporciona cierto nivel de acceso a una red más grande. Un NAS con una infraestructura de RADIUS también es un cliente RADIUS, enviar solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para la autenticación, autorización y cuentas.

14. Revisar la configuración de **proveedor de cuentas**:

    | Si quieres que el …  | En ese caso…             |
    |---------------------|-------------------|
    | Actividad de acceso remota ha iniciado sesión en el servidor de acceso remoto | Asegúrate de que está activada la **Explicación de Windows** .      |
    | NPS para prestar servicios de administración de cuentas de VPN   | Cambie el **proveedor de administración de cuentas** a **Cuentas RADIUS** y, a continuación, configura el NPS como el proveedor de cuentas. |
    ---

15. Haz clic en la pestaña **IPv4** y realice:

    a. Haz clic en **el grupo de dirección estática**.
    
    b. Haz clic en **Agregar** para configurar un grupo de direcciones IP.<p>El conjunto de direcciones estáticas debe contener las direcciones de la red perimetral interno. Estas direcciones están en la conexión de red interna en el servidor VPN, no a la red corporativa.
    
    c. En la **dirección IP de inicio**, escribe la dirección IP inicial en el intervalo que quieres asignar a los clientes VPN.
    
    d. En la **dirección IP final**, escribe la dirección IP final en el intervalo que quieres asignar a los clientes VPN o en el **número de direcciones**, escribe el número de la dirección que quieres que esté disponible. Si estás usando DHCP para la subred, asegúrate de que puedes configurar una exclusión de dirección correspondiente en tus servidores DHCP.
    
    e. (Opcional) Si estás usando DHCP, haz clic en el **adaptador**y en la lista de resultados, haz clic en el adaptador Ethernet conectado a la red perimetral interno.

16. (Opcional) *Si estás configurando el acceso condicional para la conectividad de VPN*, en la lista desplegable de **certificado** , en el **Enlace de certificado SSL**, seleccione la autenticación del servidor VPN.

17. (Opcional) *Si estás configurando el acceso condicional para la conectividad de VPN*, en el NPS en MMC, expanda **Policies\\Network directivas** y realice: 

    a. Derecha, la directiva de red de **conexiones de enrutamiento de Microsoft y el servidor de acceso remoto** y selecciona **Propiedades**.
    
    b. Selecciona el **conceder acceso. Conceder acceso si la solicitud de conexión coincide con esta directiva** opción.
    
    c. En tipo de servidor de acceso de red, selecciona **El servidor de acceso remoto (VPN de acceso telefónico)** de la lista desplegable.

3.  En el enrutamiento y acceso remoto de MMC, haz clic en **puertos** y, a continuación, haga clic en **Propiedades**. <p>Abre el cuadro de diálogo de propiedades de puertos.

4.  Haz clic en **Minipuerto WAN (SSTP)** y haga clic en **Configurar**. Configurar dispositivo - se abrirá el cuadro de diálogo de minipuerto WAN (SSTP).

    a. Desactiva las casillas de verificación **conexiones de acceso remoto (solo de entrada)** y **conexiones de enrutamiento de marcado a petición (entrantes y salientes)** .
    
    b. Haz clic en **Aceptar**.

5.  Haz clic en **Minipuerto WAN (L2TP)** y haga clic en **Configurar**. Configurar dispositivo - se abrirá el cuadro de diálogo de minipuerto WAN (L2TP).

    a. En el **número máximo de puertos**, escribe el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que quieres admitir.
    
    b. Haz clic en **Aceptar**.

6.  Haz clic en **Minipuerto WAN (PPTP)** y haga clic en **Configurar**. Configurar dispositivo - se abrirá el cuadro de diálogo de minipuerto WAN (PPTP).

    a. En el **número máximo de puertos**, escribe el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que quieres admitir.
    
    b. Haz clic en **Aceptar**.
    
7. Haz clic en **Minipuerto WAN (IKEv2)** y haga clic en **Configurar**. Configurar dispositivo - se abrirá el cuadro de diálogo de minipuerto WAN (IKEv2).

    a. En el **número máximo de puertos**, escribe el número de puertos para que coincida con el número máximo de conexiones VPN simultáneas que quieres admitir.
    
    b. Haz clic en **Aceptar**.

7.  Si se te solicite, haz clic en **Sí** para confirmar reiniciar el servidor y haz clic en **Cerrar** para reiniciar el servidor.

## Paso siguiente
[Paso 4. Instalar y configurar el servidor de directivas de redes (NPS)](vpn-deploy-nps.md): en este paso, instala servidor de directivas de redes (NPS) mediante Windows PowerShell o el administrador agregar Roles de servidor y Features Wizard. También configuración NPS para controlar todos los derechos de administración de cuentas de conexión, autorización y autenticación las solicitudes que recibe desde el servidor VPN.





---
