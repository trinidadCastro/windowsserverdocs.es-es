---
title: Configurar las directivas de red
description: En este tema se proporciona información general de configuración de directiva de red de servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a03a6c67ddd59549cefc9742ca92e0702f92a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842006"
---
# <a name="configure-network-policies"></a>Configurar las directivas de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para configurar directivas de redes en NPS.

## <a name="add-a-network-policy"></a>Agregar una directiva de red

Servidor de directivas de red \(NPS\) usa las directivas y las propiedades de marcado de cuentas de usuario para determinar si una solicitud de conexión está autorizada para conectarse a la red de red.

Puede usar este procedimiento para configurar una nueva directiva de red en la consola de NPS o la consola de acceso remoto.

### <a name="performing-authorization"></a>Realiza la autorización

Cuando NPS autoriza una solicitud de conexión, compara la solicitud con cada directiva de red en la lista ordenada de directivas, empezando por la primera directiva y, a continuación, mover hacia abajo en la lista de las directivas configuradas. Si NPS encuentra una directiva cuyas condiciones coinciden con la solicitud de conexión, NPS usa la directiva de coincidencia y las propiedades de acceso telefónico de la cuenta de usuario para realizar la autorización. Si se configuran las propiedades de acceso telefónico de la cuenta de usuario para conceder acceso o controlar el acceso a través de la directiva de red y la solicitud de conexión se autoriza, NPS aplica la configuración que se configura en la directiva de red para la conexión.

Si NPS no encuentra una directiva de red que coincida con la solicitud de conexión, se rechaza la solicitud de conexión, a menos que se establecen las propiedades de marcado de la cuenta de usuario para conceder acceso.

Si se establecen las propiedades de acceso telefónico de la cuenta de usuario para denegar el acceso, NPS rechaza la solicitud de conexión.

### <a name="key-settings"></a>Configuración de claves

Cuando usa el Asistente para nueva directiva de red para crear una directiva de red, el valor que especifique en **método de conexión de red** se usa para configurar automáticamente la **tipo de directiva** condición: 

- Si mantiene el valor predeterminado sin especificar, NPS evalúa la directiva de red que cree para todos los tipos de conexión de red que usan cualquier tipo de servidor de acceso de red (NAS).
- Si especifica un método de conexión de red, NPS evalúa la directiva de red solo si la solicitud de conexión se origina en el tipo de servidor de acceso de red que especifique.

En el **permiso de acceso** página, debe seleccionar **acceso concedido** si desea que la directiva para permitir que los usuarios se conecten a la red. Si desea que la directiva para evitar que los usuarios conectarse a la red, seleccione **acceso denegado**. 

Si desea que el permiso de acceso quedará determinado por el usuario marcado en Propiedades de la cuenta en Active Directory&reg; servicios de dominio \(AD DS\), puede seleccionar la **acceso viene determinado por el usuario propiedades de acceso telefónico** casilla de verificación.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-add-a-network-policy"></a>Para agregar una directiva de red 

1. Abra la consola de NPS y, a continuación, haga doble clic en **directivas**.

2. En el árbol de consola, haga clic en **las directivas de red**y haga clic en **New**. Se abre el Asistente para nueva directiva de red.

3. Utilice al Asistente para nueva directiva de red para crear una directiva.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Crear directivas de red para acceso telefónico o VPN con un asistente

Puede usar este procedimiento para crear directivas de solicitud de la conexión y directivas necesarias para implementar servidores de acceso telefónico o red privada virtual de red \(VPN\) servidores como autenticación remota telefónica de usuario servicio \(RADIUS\) clientes en el servidor RADIUS NPS.

>[!NOTE]
>Los equipos cliente, como equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico, conmutadores de autenticación 802.1X, red privada virtual \(VPN\) servidores y servidores de acceso telefónico, ya que estos dispositivos usan el protocolo RADIUS para comunicarse con servidores RADIUS como NPSs.

Este procedimiento explica cómo abrir al Asistente para nuevo acceso telefónico o conexiones de red privada Virtual en NPS.

Después de ejecutar al asistente, se crean las siguientes directivas:

- Directiva de solicitud de una conexión
- Una directiva de red

Puede ejecutar al Asistente para nuevo acceso telefónico o conexiones de red privada Virtual cada vez que necesita crear nuevas directivas para servidores de acceso telefónico y VPN.

Ejecuta al Asistente para nuevo acceso telefónico o conexiones de red privada Virtual no es el único paso necesario para implementar el acceso telefónico o servidores VPN como clientes RADIUS en NPS. Ambos métodos de acceso de red precisan la implementación de componentes de hardware y software adicionales.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para crear directivas para acceso telefónico o VPN con un asistente

1. Abra la consola NPS. Si aún no está seleccionada, haga clic en **NPS \(Local\)**. Si desea crear directivas en un NPS remoto, seleccione el servidor.

2. En **Introducción** y **configuración estándar**, seleccione **servidor RADIUS para conexiones VPN o telefónico**. El texto y los vínculos del texto cambian para reflejar la selección.

3. Haga clic en **configurar VPN o acceso telefónico con un asistente**. Se abre el Asistente para nuevo acceso telefónico o conexiones de red privada Virtual.

4. Siga las instrucciones del Asistente para completar la creación de las directivas nuevas.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Creación de directivas de red para 802.1X con cable o inalámbrica con un asistente

Puede usar este procedimiento para crear la directiva de solicitud de conexión y la directiva de red que son necesarios para implementar conmutadores de autenticación 802.1X o puntos de acceso inalámbrico 802.1X como clientes de servicio de autenticación remota telefónica de usuario (RADIUS) en NPS Servidor RADIUS.

Este procedimiento explica cómo iniciar al Asistente para nuevo IEEE 802.1X proteger conexiones cableadas e inalámbricas conexiones en NPS.

Después de ejecutar al asistente, se crean las siguientes directivas:

- Directiva de solicitud de una conexión
- Una directiva de red

Puede ejecutar al Asistente para nuevo IEEE 802.1X proteger conexiones cableadas e inalámbricas conexiones cada vez que necesita crear nuevas directivas para acceso 802.1X.

Ejecutar al Asistente para nuevo IEEE 802.1X proteger conexiones cableadas e inalámbricas no es el único paso necesario para implementar la autenticación 802.1X conmutadores y puntos de acceso inalámbricos como clientes RADIUS en NPS. Ambos métodos de acceso de red precisan la implementación de componentes de hardware y software adicionales.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para crear directivas para 802.1X con cable o inalámbrica con un asistente

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS. 

2. Si aún no está seleccionada, haga clic en **NPS \(Local\)**. Si desea crear directivas en un NPS remoto, seleccione el servidor.

3. En **Introducción** y **configuración estándar**, seleccione **servidor RADIUS para conexiones cableadas o inalámbricas 802.1X**. El texto y los vínculos del texto cambian para reflejar la selección.

4. Haga clic en **configurar 802.1X mediante un asistente**. Se abre el Asistente para nuevo IEEE 802.1X proteger conexiones cableadas e inalámbricas conexiones.

5. Siga las instrucciones del Asistente para completar la creación de las directivas nuevas.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configuración de NPS para que omita las propiedades de acceso telefónico de la cuenta de usuario

Utilice este procedimiento para configurar una directiva de red NPS para que omita las propiedades de marcado de cuentas de usuario en Active Directory durante el proceso de autorización. Cuentas de usuario en usuarios y equipos de usuarios de Active Directory tienen propiedades de acceso telefónico que NPS evalúa durante el proceso de autorización, a menos que el **permiso de acceso de red** propiedad de la cuenta de usuario está establecida en **Control acceso a través de la directiva de red NPS**. 

Hay dos circunstancias donde desea configurar NPS para que omita las propiedades de marcado de cuentas de usuario en Active Directory:

- Cuando desea simplificar la autorización de NPS mediante la directiva de red, pero no todas las cuentas de usuario tienen el **permiso de acceso de red** propiedad establecida en **controlar el acceso a través de la directiva de red NPS**. Por ejemplo, podrían tener algunas cuentas de usuario la **permiso de acceso de red** establece la propiedad de la cuenta de usuario en **denegar el acceso** o **permitir el acceso**.

- Cuando otras propiedades de acceso telefónico de cuentas de usuario no son aplicables al tipo de conexión que se configura en la directiva de red. Por ejemplo, las propiedades que no sea el **permiso de acceso de red** configuración solo son aplicables a conexiones VPN o acceso telefónico, pero la directiva de red que se va a crear es para las conexiones del conmutador inalámbrico o autenticación.

Puede usar este procedimiento para configurar NPS para que omita las propiedades de acceso telefónico de la cuenta de usuario. Si una solicitud de conexión coincide con la directiva de red donde se selecciona esta casilla, NPS no usa las propiedades de acceso telefónico de la cuenta de usuario para determinar si el usuario o el equipo está autorizado para tener acceso a la red. solo la configuración de la directiva de red se usa para determinar la autorización.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2. Haga doble clic en **directivas**, haga clic en **las directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la directiva que desea configurar.

3. En la directiva **propiedades** cuadro de diálogo el **Overview** ficha **permiso de acceso**, seleccione el **Omitir propiedades cuentas de usuario de acceso telefónico**casilla de verificación y, a continuación, haga clic en **Aceptar**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Para configurar NPS para que omita las propiedades de acceso telefónico de la cuenta de usuario



## <a name="configure-nps-for-vlans"></a>Configuración de NPS para VLAN

Mediante el uso de servidores de acceso de red basadas en VLAN y NPS en Windows Server 2016, puede proporcionar los grupos de usuarios con acceso únicamente a los recursos de red que son adecuadas para sus permisos de seguridad. Por ejemplo, puede proporcionar a los visitantes con acceso inalámbrico a Internet sin permitirle el acceso a la red de su organización. 

Además, las VLAN le permiten agrupar de lógicamente los recursos de red que existen en diferentes ubicaciones físicas o en diferentes subredes físicas. Por ejemplo, los miembros de su departamento de ventas y sus recursos de red, como los equipos cliente, servidores e impresoras, podrían encontrarse en varios edificios diferentes en su organización, pero todos estos recursos se coloca en una VLAN que usa la misma dirección IP intervalo de direcciones. Las VLAN, a continuación, las funciones, desde la perspectiva del usuario final, como una sola subred.

También puede utilizar VLAN cuando desee repartir una red entre los distintos grupos de usuarios. Después de determinar cómo desea definir los grupos, puede crear grupos de seguridad en el complemento de equipos y usuarios de Active Directory y, a continuación, agregar a miembros a los grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configurar una directiva de red para redes VLAN

Puede usar este procedimiento para configurar una directiva de red que se asigna a los usuarios a una VLAN. Al usar hardware de red basadas en VLAN, como enrutadores, conmutadores y controladores de acceso, puede configurar directivas de redes para indicar a los servidores de acceso que coloquen a miembros de grupos específicos de Active Directory en las VLAN específicas. Esta capacidad para agrupar los recursos de red lógica con VLAN proporciona flexibilidad al diseñar e implementar soluciones de red.

Al configurar la configuración de una directiva de red NPS para su uso con VLAN, debe configurar los atributos **Tunnel-Medium-Type**, **Tunnel-Pvt-Group-ID**, **Tunnel-Type**, y **Tunnel-Tag**. 

Este procedimiento se proporciona como guía; la configuración de red puede requerir valores diferentes a los que se describe a continuación.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar una directiva de red para redes VLAN

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2. Haga doble clic en **directivas**, haga clic en **las directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la directiva que desea configurar.

3. En la directiva **propiedades** cuadro de diálogo, haga clic en el **configuración** ficha.

4. En la directiva **propiedades**, en **configuración**, en **atributos RADIUS**, asegúrese de que **estándar** está seleccionada.

5. En el panel de detalles, en **atributos**, **Service-Type** atributo está configurado con un valor predeterminado de **entramado**. De forma predeterminada, para las directivas con métodos de acceso de VPN y telefónico, el **protocolo entramado** atributo está configurado con un valor de **PPP**. Para especificar atributos de conexión adicionales necesarios para las VLAN, haga clic en **agregar**. El **atributo RADIUS estándar agregar** abre el cuadro de diálogo.

6. En **atributo RADIUS estándar agregar**, en los atributos, desplácese hacia abajo y agregue los siguientes atributos:

    - **Tipo de medio del túnel**. Seleccione un valor adecuado para las selecciones anteriores que ha realizado en la directiva. Por ejemplo, si la directiva de red que está configurando es una directiva inalámbrica, seleccione **valor: 802 (incluye 802 todos los medios más el formato canónico Ethernet)**.

    - **Tunnel-Pvt-Group-ID**. Escriba el número entero que representa el número VLAN al que se asignarán los miembros del grupo. 

    - **Tipo de túnel**. Seleccione **LAN virtuales (VLAN)**.


7. En **atributo RADIUS estándar agregar**, haga clic en **cerrar**. 

8. Si el servidor de acceso de red (NAS) requiere el uso de la **Tunnel-Tag** atributo, use los pasos siguientes para agregar la **Tunnel-Tag** atributo a la directiva de red. Si la documentación de NAS no menciona este atributo, no lo agregue a la directiva. Si es necesario, agregue los atributos de la manera siguiente:

    - En la directiva **propiedades**, en **configuración**, en **atributos RADIUS**, haga clic en **específico del proveedor**. 

    - En el panel de detalles, haga clic en **agregar**. El **agregar atributos específicos del proveedor** abre el cuadro de diálogo.

    - En **atributos**, desplácese hacia abajo y seleccione **Tunnel-Tag**y, a continuación, haga clic en **agregar**. El **información del atributo** abre el cuadro de diálogo. 

    - En **valor del atributo**, escriba el valor que ha obtenido en la documentación del hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar el tamaño de la carga EAP

En algunos casos, enrutador o Firewall quitar paquetes porque están configurados para descartar los paquetes que requieren la fragmentación.

Cuando implementa NPS con las directivas de red que utilizan el protocolo de autenticación Extensible \(EAP\) con Transport Layer Security \(TLS\), o EAP-TLS como método de autenticación, el valor máximo predeterminado unidad de transmisión \(MTU\) que usa NPS para las cargas EAP es de 1500 bytes. 

Este tamaño máximo para la carga EAP puede crear mensajes RADIUS que requieren la fragmentación por un enrutador o firewall entre el NPS y un cliente RADIUS. Si este es el caso, un enrutador o firewall colocado entre el cliente RADIUS y el NPS podría descartan silenciosamente algunos fragmentos, dando como resultado un error de autenticación y la imposibilidad de que el cliente de acceso para conectarse a la red.

Use el procedimiento siguiente para reducir el tamaño máximo que usa NPS para las cargas EAP ajustando el atributo MTU de entramado en una directiva de red en un valor no superior a 1344.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar el atributo MTU de entramado

1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.
 
2. Haga doble clic en **directivas**, haga clic en **las directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la directiva que desea configurar.

3. En la directiva **propiedades** cuadro de diálogo, haga clic en el **configuración** ficha.

4. En **configuración**, en **atributos RADIUS**, haga clic en **estándar**. En el panel de detalles, haga clic en **agregar**. El **atributo RADIUS estándar agregar** abre el cuadro de diálogo.

5. En **atributos**, desplácese hacia abajo y haga clic en **MTU de entramado**y, a continuación, haga clic en **agregar**. El **información del atributo** abre el cuadro de diálogo.

6. En **valor del atributo**, escriba un valor igual o menor que **1344**. Haga clic en **Aceptar**, haga clic en **cerrar**y, a continuación, haga clic en **Aceptar**.



Para obtener más información acerca de las directivas de red, consulte [las directivas de red](nps-np-overview.md).

Para obtener ejemplos de sintaxis de coincidencia de patrón para especificar los atributos de directiva de red, consulte [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
