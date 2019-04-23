---
title: Instalar y configurar el servidor NPS
description: Procesamiento de solicitudes de conexión que se envían por el servidor VPN del servidor NPS comprueba que el usuario tiene permiso para conectarse, la identidad del usuario y registra los aspectos de la solicitud de conexión que eligió al configurar cuentas RADIUS en NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890316"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Paso 4. Instalar y configurar el servidor de directivas de redes (NPS)

>   Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10


&#171;  [**próximo:** Paso 3. Configurar el servidor de acceso remoto para siempre en VPN](vpn-deploy-ras.md)<br>
&#187;  [**próximo:** Paso 5. Configurar DNS y configuración del Firewall](vpn-deploy-dns-firewall.md)


En este paso, se instale el servidor de directivas de redes (NPS) para el procesamiento de solicitudes de conexión que se envían por el servidor VPN:

- Realizar la autorización para comprobar que el usuario tiene permiso para conectarse.
- Realizar la autenticación para comprobar la identidad del usuario.
- Realización de contabilidad para registrar los aspectos de la solicitud de conexión que eligió al configurar cuentas RADIUS en NPS.

Los pasos descritos en esta sección le permiten completar los siguientes elementos:

1.  En el equipo o máquina virtual que planea para el servidor NPS e instalado en su organización o la red corporativa, puede instalar NPS.

   >[!TIP] 
   >Si ya tiene uno o varios servidores NPS de la red, no es necesario realizar la instalación del servidor NPS: en su lugar, puede utilizar este tema para actualizar la configuración de un servidor existente de NPS.

>[!NOTE]  
No puede instalar el servicio servidor de directivas de red en Windows Server Core.

2.  En el servidor NPS o corporativas de la organización, puede configurar NPS para funcionar como servidor RADIUS que procesa las solicitudes de conexión recibidas desde el servidor VPN.

## <a name="install-network-policy-server"></a>Instalar el Servidor de directivas de redes

En este procedimiento, instale NPS mediante el uso de Windows PowerShell o el Server Manager agregar Roles y características Asistente. NPS es un servicio de rol del rol de servidor Servicios de acceso y directivas de redes.

>[!TIP] 
>De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Al instalar NPS, y habilita el Firewall de Windows con seguridad avanzada, excepciones de firewall para estos puertos se crean automáticamente para el tráfico IPv4 e IPv6. Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que usa para Tráfico RADIUS.

**Procedimiento para Windows PowerShell:**

Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione ENTRAR.

`Install-WindowsFeature NPAS -IncludeManagementTools`

**Procedimiento para el administrador del servidor:**

1.  En el administrador del servidor, haga clic en **administrar**y haga clic en **agregar Roles y características**. <p>Se abre el Asistente para agregar roles y características.

2.  En antes de comenzar, haga clic en **siguiente**.

    >[!NOTE] 
    >El **antes de comenzar** no se muestra la página del Asistente de las características y agregar Roles si había seleccionado anteriormente **omitir esta página predeterminada** cuando se ejecutaban el agregar Roles y características de asistente.

1.  En Seleccionar tipo de instalación, asegúrese de que **instalación basada en roles o basada en características** está seleccionada y haga clic en **siguiente**.

2.  En Seleccionar servidor de destino, asegúrese de que **seleccionar un servidor del grupo de servidores** está seleccionada.

3.  En el grupo de servidores, asegúrese de que el equipo local está seleccionado y haga clic en **siguiente**.

4.  En Seleccionar Roles de servidor, en **Roles**, seleccione **servicios de acceso y directivas de redes**. Abre un cuadro de diálogo que pregunta si se deben agregar características necesarias para servicios de acceso y directivas de redes.

5.  Haga clic en **agregar características**y haga clic en **siguiente**

6.  En Seleccionar características, haga clic en **siguiente**y en servicios de acceso y directivas de redes, revise la información proporcionada y haga clic en **siguiente**.

7.  En servicios de rol seleccione, haga clic en **servidor de directivas de red**.

8.  Para las características necesarias para el servidor de directivas de red, haga clic en **agregar características** y haga clic en **siguiente**.

9.  En Confirmar selecciones de instalación, haga clic en **reiniciar automáticamente el servidor de destino si es necesario**.

10. Haga clic en **Sí** para confirmar el texto seleccionado y, a continuación, haga clic en **instalar**. <p>La página de progreso de instalación muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "instalación correcta en *ComputerName*" se muestra, donde *ComputerName* es el nombre del equipo en el que instaló el servidor de directivas de red.

11. Haga clic en **Cerrar**.

## <a name="configure-nps"></a>Configuración de NPS

Después de instalar NPS, configurar NPS para administrar toda la autenticación, autorización, y lo solicitan los derechos de administración de cuentas de conexión se recibe desde el servidor VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrar el servidor NPS en Active Directory

En este procedimiento, registrar el servidor en Active Directory para que tenga permiso para acceder a información de cuenta de usuario durante el procesamiento de solicitudes de conexión.

**Procedimiento:**

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2.  En la consola NPS, haga clic en **NPS (Local)** y haga clic en **registrar el servidor en Active Directory** para seleccionarlo.<p>Se abre el cuadro de diálogo servidor de directivas de red.

3.  En el cuadro de diálogo servidor de directivas de red, haga clic en **Aceptar** dos veces.

Para conocer otros métodos de registro NPS, consulte [registrar un servidor NPS en un dominio de Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurar las cuentas de servidor de directivas de redes

En este procedimiento, configure la directiva de servidor de cuentas de red mediante uno de los siguientes tipos de registro:

-   **Registro de eventos**. Se utiliza principalmente para supervisar y solucionar los intentos de conexión. Puede configurar el registro mediante la obtención de las propiedades del servidor NPS en la consola de NPS de eventos NPS.

-   **Registro de autenticación de usuario y las solicitudes de cuentas en un archivo local**. Se utiliza principalmente para fines de facturación y análisis de la conexión. También puede usar como una herramienta de investigación de seguridad porque proporciona un método de seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configuración de cuentas.

-   **Registro de autenticación de usuario y las solicitudes de cuentas en una base de datos de Microsoft SQL Server compatible con XML**. Se usa para permitir que a varios servidores que ejecuten NPS tengan un origen de datos. También proporciona las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server utilizando el Asistente para configuración de cuentas.

Para configurar cuentas de servidor de directivas de red, consulte [configurar red directiva de servidor de contabilidad](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Agregue el servidor VPN como cliente RADIUS

En el [configurar el servidor de acceso remoto de VPN de Always On](vpn-deploy-ras.md) sección, ha instalado y configurado el servidor VPN. Durante la configuración del servidor VPN, se agrega un secreto compartido en el servidor VPN. 

En este procedimiento, usará la misma cadena de texto secreto compartido para configurar el servidor VPN como cliente RADIUS en NPS. Utilice la misma cadena de texto que se utilizó en el servidor VPN, o se produce un error de comunicación entre el servidor NPS y el servidor VPN.

>[!IMPORTANT] 
>Cuando se agrega un nuevo servidor de acceso de red (servidor VPN, punto de acceso inalámbrico, conmutador de autenticación o el servidor de acceso telefónico) a la red, debe agregar el servidor como un cliente RADIUS en NPS para que NPS reconozca y pueda comunicarse con el servidor de acceso de red.

**Procedimiento:**

1.  En el servidor NPS, en la consola NPS, haga doble clic en **clientes y servidores RADIUS**.

2.  Haga clic en **clientes RADIUS** y haga clic en **New**. Se abre el cuadro de diálogo nuevo cliente RADIUS.

3.  Compruebe que la **habilitar este cliente RADIUS** casilla está activada.

4.  En **Nombre_descriptivo**, escriba un nombre para mostrar para el servidor VPN.

5.  En **dirección (IP o DNS)**, escriba el FQDN o la dirección IP de NAS.<p>Si especifica el FQDN, haga clic en **compruebe** si desea comprobar que el nombre es correcto y se asigna a una dirección IP válida.

6.  En **secreto compartido**, hacer:

    1.  Asegúrese de que **Manual** está seleccionada.

    2.  Escriba la cadena de texto seguro que también especificó en el servidor VPN.

    3.  Vuelva a escribir el secreto compartido en Confirmar secreto compartido.

7.  Haga clic en **Aceptar**. El servidor VPN aparece en la lista de clientes RADIUS configurado en el servidor NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configure NPS como un radio para las conexiones VPN

En este procedimiento, configure NPS como servidor RADIUS en la red de su organización. En el NPS, debe definir una directiva que permita solo los usuarios en un grupo específico para tener acceso a la red de la organización corporativo a través del servidor VPN - y, a continuación, solo cuando se usa un certificado de usuario válido en una solicitud de autenticación PEAP.

**Procedimiento:**

1.  En la consola NPS, en la configuración estándar, asegúrese de que **servidor RADIUS para conexiones VPN o telefónico** está seleccionada.

2.  Haga clic en **configurar VPN o telefónico**.<p>Se abre el Asistente para configurar VPN o telefónico.

3.  Haga clic en **las conexiones de red privada Virtual (VPN)** y haga clic en **siguiente**.

4.  En especificar telefónico o de servidor VPN, en los clientes RADIUS, seleccione el nombre del servidor VPN al que agregó en el paso anterior.<p>Por ejemplo, si el nombre de NetBIOS del servidor VPN es RAS1, haga clic en **RAS1**.

5.  Haz clic en **Siguiente**.

6.  En configurar métodos de autenticación, complete los pasos siguientes:

    1.  Desactive el **Microsoft Encrypted Authentication versión 2 (MS-CHAPv2)** casilla de verificación.

    2.  Haga clic en el **protocolo de autenticación Extensible** casilla de verificación para seleccionarlo.

    3.  En el tipo (según el método de acceso y configuración de red), haga clic en **Microsoft: EAP protegido (PEAP)** y haga clic en **configurar**.<p>Se abre el cuadro de diálogo Editar propiedades de EAP protegido.

    4.  Haga clic en **quitar** para quitar el tipo EAP de contraseña segura (EAP-MSCHAP v2).

    5.  Haz clic en **Agregar**. Se abre el cuadro de diálogo Agregar EAP.

    6.  Haga clic en **tarjeta inteligente u otro certificado**y haga clic en **Aceptar**.

    7.  Haga clic en **Aceptar** para cerrar Editar propiedades de EAP protegido.

7.  Haz clic en **Siguiente**.

8.  En especificar grupos de usuarios, complete los pasos siguientes:

    1.  Haz clic en **Agregar**. Se abre el cuadro de diálogo Seleccionar usuarios, equipos, cuentas de servicio o grupos.

    2.  Tipo **usuarios de VPN** y haga clic en **Aceptar**.

    3.  Haz clic en **Siguiente**.

9.  En especificar filtros IP, haga clic en **siguiente**.

10. En especificar la configuración de cifrado, haga clic en **siguiente**. No realice los cambios.<p>Esta configuración se aplica sólo a conexiones de punto a punto MPPE (cifrado), lo que no es compatible con este escenario.

11. En especificar un nombre de dominio Kerberos, haga clic en **siguiente**.

12. Haga clic en **finalizar** para cerrar el asistente.

## <a name="autoenroll-the-nps-server-certificate"></a>Inscribir un certificado del servidor NPS

En este procedimiento, actualiza Directiva de grupo en el servidor NPS local manualmente. Cuando las actualizaciones de directiva de grupo, si está configurada la inscripción automática de certificados y funciona correctamente, el equipo local es inscriben automáticamente un certificado de entidad de certificación (CA).

>[!NOTE]  
>Directiva de grupo se actualiza automáticamente al reiniciar el equipo miembro del dominio, o cuando un usuario inicia sesión en un equipo miembro del dominio. Además, directiva de grupo se actualiza periódicamente. De forma predeterminada, esta actualización periódica ocurre cada 90 minutos con un desplazamiento aleatorio de hasta 30 minutos.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

**Procedimiento:**

1.  En NPS, abra Windows PowerShell.

2.  En el símbolo del sistema de Windows PowerShell, escriba **gpupdate**, y, a continuación, presione ENTRAR.

## <a name="next-step"></a>Paso siguiente
[Paso 5. Configuración DNS y del firewall para VPN de Always On](vpn-deploy-dns-firewall.md): En este paso, se instala el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Server Manager agregar Roles y características Asistente. También configura NPS para controlar la autenticación, autorización y los derechos de administración de cuentas de conexión todos los solicitudes que recibe desde el servidor VPN.



---

