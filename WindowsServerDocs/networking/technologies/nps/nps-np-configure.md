---
title: Configurar las directivas de red
description: Este tema proporciona una visión general de la configuración de directiva de red para el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abcd9343f10100884f96d17d82704382e2af760
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policies"></a>Configurar las directivas de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para configurar las directivas de red en NPS.

## <a name="add-a-network-policy"></a>Agregar una directiva de red

Red el servidor de directivas \(NPS\) usa las directivas de red y las propiedades de marcado de cuentas de usuario para determinar si una solicitud de conexión está autorizado para conectarse a la red.

Puedes usar este procedimiento para configurar una nueva directiva de red en la consola NPS o la consola de acceso remoto.

### <a name="performing-authorization"></a>Realizar la autorización

Cuando NPS realiza la autorización de una solicitud de conexión, compara la solicitud con cada directiva de red en la lista ordenada de directivas, a partir de la primera directiva y, a continuación, desplazarse por la lista de directivas configuradas. Si NPS encuentra una directiva cuyas condiciones coinciden con la solicitud de conexión, NPS usa la directiva coincidente y las propiedades de marcado de la cuenta de usuario para realizar la autorización. Si se configuran las propiedades de marcado de la cuenta de usuario para conceder acceso o control de acceso mediante la directiva de red y se autoriza la solicitud de conexión, NPS aplica la configuración que está configurada en la directiva de red para la conexión.

Si NPS no encuentra una directiva de red que coincida con la solicitud de conexión, la solicitud de conexión se rechaza a menos que se establecen las propiedades de marcado de la cuenta de usuario que conceda acceso.

Si se establecen las propiedades de marcado de la cuenta de usuario para denegar el acceso, NPS rechazará la solicitud de conexión.

### <a name="key-settings"></a>Configuración de claves

Cuando usas el Asistente para la nueva directiva de red para crear una directiva de red, el valor que especifiques en **método de conexión de red** se usa para configurar automáticamente el **tipo de directiva** condición: 

- Si haces que el valor predeterminado sin especificar, se evalúa la directiva de red que crees NPS para todos los tipos de conexión de red que usan cualquier tipo de servidor de acceso de red (NAS).
- Si especificas un método de conexión de red, NPS evalúa la directiva de red solo si la solicitud de conexión se origina en el tipo de servidor de acceso de red que especifiques.

En la **permiso de acceso** página, debes seleccionar **acceso otorgado** si quieres la directiva para permitir que los usuarios se conecten a la red. Si quieres la directiva para impedir que los usuarios conectarse a la red, selecciona **acceso denegado**. 

Si quieres que el permiso de acceso a determinarse al usuario acceso telefónico propiedades de la cuenta en Active Directory&reg; \(AD DS\) los servicios de dominio, puedes seleccionar el **acceso está determinado por el usuario propiedades de acceso telefónico** casilla de verificación.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-network-policy"></a>Para agregar una directiva de red 

1. Abre la consola NPS y, a continuación, haz doble clic en **directivas**.

2. En el árbol de consola, haz clic en **las directivas de red**y haz clic en **nueva**. Abre el Asistente para la nueva directiva de red.

3. Usar al Asistente para la nueva directiva de red para crear una directiva.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Crear directivas de red para acceso telefónico o VPN con un asistente

Puedes usar este procedimiento para crear las directivas de solicitud de conexión y directivas de red necesarias para implementar los servidores de acceso telefónico o de red privada virtual \(VPN\) servidores como clientes \(RADIUS\) servicio autenticación remota telefónica de usuario en el servidor NPS RADIUS.

>[!NOTE]
>Los equipos cliente, como equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes de RADIUS. Clientes de RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico, conmutadores de autenticación 802.1X, servidores de red privada virtual \(VPN\) y servidores de acceso telefónico, ya que estos dispositivos usan el protocolo RADIUS para comunicarse con los servidores RADIUS, como servidores NPS.

Este procedimiento explica cómo abrir al Asistente para nueva telefónico o de conexiones de red privada Virtual en NPS.

Después de ejecutar al asistente, se crean las directivas siguientes:

- Directiva de solicitud de una conexión
- Una directiva de red

Puedes ejecutar al Asistente para nueva telefónico o de conexiones de red privada Virtual cada vez que necesitas para crear nuevas directivas para servidores de acceso telefónico y VPN.

Ejecuta al Asistente para nuevo telefónico o de conexiones de red privada Virtual no es el único paso necesario para implementar telefónico o servidores VPN como clientes RADIUS en el servidor NPS. Ambos métodos de acceso de red requieren que implementas los componentes de hardware y software adicionales.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para crear directivas para acceso telefónico o VPN con un asistente

1. Abre la consola NPS. Si no está ya seleccionado, haz clic en **NPS \(Local\)**. Si quieres crear directivas en un servidor NPS remoto, selecciona el servidor.

2. En **Introducción** y **configuración estándar**, selecciona **RADIUS server para conexiones VPN o telefónico**. El texto y los vínculos en el cambio de texto para reflejar la selección.

3. Haz clic en **configurar VPN o telefónico con un asistente**. Abre el Asistente para nueva telefónico o de conexiones de red privada Virtual.

4. Sigue las instrucciones del Asistente para completar la creación de las directivas de nuevo.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Crear directivas de red para 802.1X X con cable o inalámbrica con el Asistente

Puedes usar este procedimiento para crear la directiva de solicitud de conexión y la directiva de red que son necesarios para implementar los conmutadores de autenticación 802.1X o puntos de acceso inalámbrico 802.1X como clientes del servicio de autenticación remota telefónica de usuario (RADIUS) en el servidor NPS RADIUS.

Este procedimiento explica cómo iniciar al Asistente para nuevo IEEE 802.1X seguro cableadas y conexiones inalámbricas en NPS.

Después de ejecutar al asistente, se crean las directivas siguientes:

- Directiva de solicitud de una conexión
- Una directiva de red

Puedes ejecutar al Asistente para nuevo IEEE 802.1X seguro cableadas y conexiones inalámbricas cada vez que necesitas para crear nuevas directivas para acceso 802.1X.

Ejecuta al Asistente para nuevo IEEE 802.1X seguro cableadas y conexiones inalámbricas no es el único paso necesario para implementar la autenticación 802.1X conmutadores y puntos de acceso inalámbrico como clientes RADIUS en el servidor NPS. Ambos métodos de acceso de red requieren que implementas los componentes de hardware y software adicionales.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para crear directivas para 802.1X con cable o inalámbrica con el Asistente

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS. 

2. Si no está ya seleccionado, haz clic en **NPS \(Local\)**. Si quieres crear directivas en un servidor NPS remoto, selecciona el servidor.

3. En **Introducción** y **configuración estándar**, selecciona **RADIUS server para conexiones con cable o inalámbricas 802.1X**. El texto y los vínculos en el cambio de texto para reflejar la selección.

4. Haz clic en **configurar 802.1X usar un asistente**. Abre el Asistente para nuevo IEEE 802.1X seguro cableadas y conexiones inalámbricas.

5. Sigue las instrucciones del Asistente para completar la creación de las directivas de nuevo.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configuración de NPS para omitir las propiedades de marcado de la cuenta de usuario

Usa este procedimiento para configurar una directiva de red NPS para omitir las propiedades de marcado de cuentas de usuario en Active Directory durante el proceso de autorización. Cuentas de usuario en equipos y usuarios de Active Directory tienen propiedades de marcado que se evalúa como NPS durante el proceso de autorización a menos que la **permiso de acceso de red** se establece la propiedad de la cuenta de usuario en **controlar el acceso a través de la directiva de red NPS**. 

Hay dos casos donde desea configurar NPS para omitir las propiedades de marcado de cuentas de usuario en Active Directory:

- Cuando desee simplificar la autorización de NPS mediante la directiva de red, pero no todas las cuentas de usuario tienen la **permiso de acceso de red** propiedad establecida en **controlar el acceso a través de la directiva de red NPS**. Por ejemplo, algunas cuentas de usuario tenga la **permiso de acceso de red** establece la propiedad de la cuenta de usuario en **denegar el acceso** o **permitir el acceso**.

- Cuando otras propiedades de acceso telefónico de cuentas de usuario no son aplicables al tipo de conexión que está configurado en la directiva de red. Por ejemplo, propiedades que no sean el **permiso de acceso de red** configuración se aplica solo a acceso telefónico o conexiones VPN, pero vas a crear la directiva de red es para las conexiones inalámbricas o autenticación de conmutación.

Puedes usar este procedimiento para configurar NPS para omitir las propiedades de marcado de la cuenta de usuario. Si una solicitud de conexión coincide con la directiva de red donde esta casilla está activada, NPS no usa las propiedades de marcado de la cuenta de usuario para determinar si el usuario o el equipo está autorizado para acceder a la red. la configuración en la directiva de red se usa para determinar la autorización.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.

2. Haz doble clic en **directivas**, haz clic en **las directivas de red**y, a continuación, en el panel de detalles, haz doble clic en la directiva que quieras configurar.

3. En la directiva **propiedades** cuadro de diálogo, en la **Introducción** pestaña **permiso de acceso**, selecciona el **Omitir propiedades de marcado de la cuenta de usuario** y, a continuación, haz clic en **Aceptar**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Para configurar NPS para omitir las propiedades de marcado de la cuenta de usuario



## <a name="configure-nps-for-vlans"></a>Configuración de NPS para VLAN

Con los servidores de acceso de red compatibles con VLAN y NPS en Windows Server 2016, puedes proporcionar grupos de usuarios con acceso solo a los recursos de red que son adecuadas para sus permisos de seguridad. Por ejemplo, puede proporcionar a los visitantes con acceso inalámbrico a Internet sin permitirles el acceso a la red de la organización. 

Además, VLAN permiten lógicamente recursos de red de grupo que existen en distintas ubicaciones físicas o en subredes físicas diferentes. Por ejemplo, los miembros de tu departamento de ventas y sus recursos de red, como impresoras, servidores y equipos cliente podrían encontrarse en varios edificios diferentes en la organización, pero puedes colocar todos estos recursos en una VLAN que usa el mismo intervalo de direcciones IP. A continuación, el VLAN funciones, desde la perspectiva del usuario final, como una única subred.

También puedes usar VLAN cuando quieras separar una red entre distintos grupos de usuarios. Después de que has determinado cómo quieres definir los grupos, puedes crear grupos de seguridad en el complemento de equipos y usuarios de Active Directory y, a continuación, agregar a miembros a los grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configurar una directiva de red para VLAN

Puedes usar este procedimiento para configurar una directiva de red que se asigna a los usuarios a una VLAN. Cuando usas compatible con VLAN hardware de red, como los enrutadores, los conmutadores y los controladores de acceso, puedes configurar la directiva de red para indicar a los servidores de acceso para colocar a los miembros de grupos específicos de Active Directory en VLAN específicas. Esta capacidad a los recursos de red de grupo lógicamente con VLAN proporciona flexibilidad al diseñar e implementar las soluciones de red.

Cuando configuras la configuración de una directiva de red NPS para su uso con VLAN, debes configurar los atributos **tipo de medio de túnel**, **Tunnel-Pvt-Group-ID**, **tipo de túnel**, y **túnel etiqueta**. 

Este procedimiento se proporciona como una regla; la configuración de red puede requerir distintas opciones de configuración que se describe a continuación.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar una directiva de red para VLAN

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.

2. Haz doble clic en **directivas**, haz clic en **las directivas de red**y, a continuación, en el panel de detalles, haz doble clic en la directiva que quieras configurar.

3. En la directiva **propiedades** cuadro de diálogo, haz clic en el **configuración** pestaña.

4. En la directiva **propiedades**, en **configuración**, en **atributos RADIUS**, asegúrate de que **estándar** está seleccionado.

5. En el panel de detalles, en **atributos**, la **tipo de servicio** atributo está configurado con un valor predeterminado de **trama**. De manera predeterminada, para las directivas con métodos de acceso de VPN y dial-up la **protocolo de trama** atributo está configurado con un valor de **PPP**. Para especificar los atributos de conexión adicionales necesarios para VLAN, haz clic en **agregar**. La **Agregar atributo de radio estándar** abre el cuadro de diálogo.

6. En **Agregar atributo de radio estándar**, en los atributos, desplázate hacia abajo hasta y agrega los siguientes atributos:

    - **Tipo de medio de túnel**. Selecciona un valor adecuado para las selecciones anteriores que ha realizado para la directiva. Por ejemplo, si va a configurar la directiva de red es una directiva inalámbrica, seleccione **valor: 802 (incluye 802 todos los medios más formato canónico Ethernet)**.

    - **Tunnel-Pvt-Group-ID**. Escribe el número entero que representa el número VLAN a la que se asignará los miembros del grupo. 

    - **Tipo de túnel**. Selecciona **LAN virtuales (VLAN)**.


7. En **Agregar atributo de radio estándar**, haz clic en **cerrar**. 

8. Si el servidor de acceso de red (NAS) requiere el uso de la **túnel etiqueta** atributo, usa los siguientes pasos para agregar la **túnel etiqueta** atributo a la directiva de red. Si la documentación de NAS no menciona este atributo, no agregues a la directiva. Si es necesario, agrega los atributos como sigue:

    - En la directiva **propiedades**, en **configuración**, en **atributos RADIUS**, haz clic en **proveedor específico**. 

    - En el panel de detalles, haz clic en **agregar**. La **agregar atributos específicos del proveedor** abre el cuadro de diálogo.

    - En **atributos**, desplácese hacia abajo y selecciona **túnel etiqueta**y, a continuación, haz clic en **agregar**. La **atributo información** abre el cuadro de diálogo. 

    - En **valor de atributo**, escribe el valor que obtiene de la documentación del hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar el tamaño de la carga EAP

En algunos casos, los enrutadores o firewalls quitar paquetes ya que se configuran para descartar los paquetes que requieren la fragmentación.

Cuando implementas NPS con las directivas de red que usan el protocolo de autenticación Extensible de \(EAP\) con seguridad de la capa de transporte \(TLS\) o EAP-TLS, como un método de autenticación, es la unidad de transmisión máxima predeterminada \(MTU\) que usa NPS para cargas EAP de 1500 bytes. 

Este tamaño máximo para la carga EAP puede crear mensajes de radio que requieren la fragmentación mediante un enrutador o firewall entre un cliente RADIUS y el servidor NPS. Si este es el caso, un enrutador o firewall que se coloca entre el cliente RADIUS y el servidor NPS puede descartar algunos fragmentos, provocará un error de autenticación y la incapacidad del cliente de acceso para conectarse a la red de forma silenciosa.

Usa el siguiente procedimiento para reducir el tamaño máximo que NPS usa cargas de EAP ajustando el atributo de trama MTU en una directiva de red en un valor que no supere los 1344.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar el atributo de trama MTU

1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.
 
2. Haz doble clic en **directivas**, haz clic en **las directivas de red**y, a continuación, en el panel de detalles, haz doble clic en la directiva que quieras configurar.

3. En la directiva **propiedades** cuadro de diálogo, haz clic en el **configuración** pestaña.

4. En **configuración**, en **atributos RADIUS**, haz clic en **estándar**. En el panel de detalles, haz clic en **agregar**. La **Agregar atributo de radio estándar** abre el cuadro de diálogo.

5. En **atributos**, desplácese hacia abajo y haz clic en **trama MTU**y, a continuación, haz clic en **agregar**. La **atributo información** abre el cuadro de diálogo.

6. En **valor de atributo**, escribe un valor menor o igual que **1344**. Haz clic en **Aceptar**, haz clic en **cerrar**y, a continuación, haz clic en **Aceptar**.



Para obtener más información acerca de las directivas de red, consulta [las directivas de red](nps-np-overview.md).

Para los ejemplos de la sintaxis de coincidencia para especificar los atributos de directiva de red, consulta [usa las expresiones regulares de NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
