---
title: Configurar las directivas de red
description: En este tema se proporciona información general sobre la configuración de directivas de redes para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 88c35f983ae998312ad3353245927c943869d7f0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952060"
---
# <a name="configure-network-policies"></a>Configurar las directivas de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para configurar directivas de red en NPS.

## <a name="add-a-network-policy"></a>Add a Network Policy

El servidor de \( directivas \) de redes NPS usa directivas de red y las propiedades de acceso telefónico de las cuentas de usuario para determinar si una solicitud de conexión está autorizada para conectarse a la red.

Puede usar este procedimiento para configurar una nueva Directiva de red en la consola de NPS o en la consola de acceso remoto.

### <a name="performing-authorization"></a>Proceso de autorización

Cuando NPS autoriza una solicitud de conexión, compara la solicitud con cada una de las directivas de red de la lista ordenada de directivas, comenzando por la primera y, posteriormente, desplazándose en sentido descendente por el resto de las directivas. Si NPS encuentra una directiva cuyas condiciones coinciden con la solicitud de conexión, usa la directiva coincidente y las propiedades de acceso telefónico de la cuenta de usuario para realizar la autorización. Si las propiedades de acceso telefónico de la cuenta de usuario se configuran para conceder acceso o controlar el acceso a través de la directiva de red y la solicitud de conexión se autoriza, NPS aplica los valores configurados en la directiva de red para la conexión.

Si NPS no encuentra una directiva de red que coincida con la solicitud de conexión, la solicitud se rechazará a menos que las propiedades de acceso telefónico de la cuenta de usuario se hayan configurado para conceder acceso.

Si las propiedades de acceso telefónico de la cuenta de usuario se configuran para denegar el acceso, NPS rechazará la solicitud de conexión.

### <a name="key-settings"></a>Configuración importante

Cuando se usa el Asistente para nueva Directiva de red para crear una directiva de red, el valor que se especifica en el **método de conexión de red** se usa para configurar automáticamente la condición de tipo de **Directiva** :

- Si mantiene el valor predeterminado de sin especificar, NPS evalúa la Directiva de red que cree para todos los tipos de conexión de red que usan cualquier tipo de servidor de acceso a la red (NAS).
- Si especifica un método de conexión a red, NPS sólo evalúa la directiva de red si la solicitud de conexión se origina en el tipo de servidor de acceso a red que haya especificado.

En la página **permiso de acceso** , debe seleccionar **acceso concedido** si desea que la Directiva permita a los usuarios conectarse a la red. Si desea que la Directiva impida que los usuarios se conecten a la red, seleccione **acceso denegado**.

Si desea que el permiso de acceso esté determinado por las propiedades de acceso telefónico de la cuenta de usuario en Active Directory &reg; AD DS de servicios de dominio \( \) , puede seleccionar la casilla el **acceso está determinado por las propiedades de marcado del usuario** .

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-network-policy"></a>Para agregar una directiva de red

1. Abra la consola de NPS y, a continuación, haga doble clic en **directivas**.

2. En el árbol de consola, haga clic con el botón secundario en **directivas de red**y haga clic en **nuevo**. Se abre el asistente para nueva directiva de red.

3. Use este asistente para crear una directiva.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Crear directivas de red para acceso telefónico o VPN con un asistente

Puede usar este procedimiento para crear las directivas de solicitud de conexión y las directivas de red necesarias para implementar servidores de acceso telefónico o servidores VPN de red privada virtual \( \) como servicio de autenticación remota telefónica de usuario \( \) clientes RADIUS en el servidor RADIUS NPS.

>[!NOTE]
>Los equipos cliente, como los equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso a la red, como los puntos de acceso inalámbrico, los conmutadores de autenticación de 802.1 X, los servidores VPN de red privada virtual \( \) y los servidores de acceso telefónico, ya que estos dispositivos usan el protocolo RADIUS para comunicarse con servidores RADIUS como NPSs.

En este procedimiento se explica cómo abrir el Asistente para nuevas conexiones de acceso telefónico o de red privada virtual en NPS.

Una vez que ejecute el asistente, se crean las siguientes directivas:

- Una directiva de solicitud de conexión
- Una directiva de red

Puede ejecutar el asistente para nuevas conexiones de acceso telefónico o de red privada virtual cada vez que necesite crear nuevas directivas para servidores de acceso telefónico y servidores VPN.

Ejecutar el Asistente para nuevas conexiones de acceso telefónico o de red privada virtual no es el único paso necesario para implementar servidores de acceso telefónico o VPN como clientes RADIUS en el NPS. Ambos métodos de acceso a la red precisan la implementación de componentes adicionales de hardware y software.

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Para crear directivas para acceso telefónico o VPN con un asistente

1. Abra la consola de NPS. Si aún no está seleccionada, haga clic **en \( NPS \) local**. Si desea crear directivas en un NPS remoto, seleccione el servidor.

2. En **Introducción** y **Configuración estándar**, seleccione **servidor RADIUS para conexiones VPN o de acceso telefónico**. El texto y los vínculos del texto cambian para reflejar la selección.

3. Haga clic en **Configurar VPN o acceso telefónico con un asistente**. Se abre el asistente para nueva conexión de acceso telefónico o de red privada virtual.

4. Siga las instrucciones del Asistente para completar la creación de las nuevas directivas.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Creación de directivas de red para 802.1 X cableadas o inalámbricas con un asistente

Puede usar este procedimiento para crear la Directiva de solicitud de conexión y la Directiva de red necesarias para implementar conmutadores de autenticación 802.1 X o puntos de acceso inalámbricos 802.1 X como clientes de Servicio de autenticación remota telefónica de usuario (RADIUS) en el servidor RADIUS NPS.

En este procedimiento se explica cómo iniciar el nuevo Asistente para conexiones inalámbricas y cableadas seguras IEEE 802.1 X en NPS.

Una vez que ejecute el asistente, se crean las siguientes directivas:

- Una directiva de solicitud de conexión
- Una directiva de red

Puede ejecutar el asistente para Nuevas conexiones cableadas e inalámbricas seguras IEEE 802.1X todas las veces que necesite para crear nuevas directivas para acceso 802.1X.

La ejecución del Asistente para nuevas conexiones cableadas e inalámbricas de IEEE 802.1 X Secure no es el único paso necesario para implementar conmutadores de autenticación de 802.1 X y puntos de acceso inalámbricos como clientes RADIUS en NPS. Ambos métodos de acceso a la red precisan la implementación de componentes adicionales de hardware y software.

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Para crear directivas para conexiones cableadas o inalámbricas 802.1X con un asistente

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. Si aún no está seleccionada, haga clic **en \( NPS \) local**. Si desea crear directivas en un NPS remoto, seleccione el servidor.

3. En **Introducción** y **Configuración estándar**, seleccione **servidor RADIUS para conexiones cableadas o inalámbricas 802.1 x**. El texto y los vínculos del texto cambian para reflejar la selección.

4. Haga clic en **configurar 802.1 x con un asistente**. Se abre el asistente para Nuevas conexiones cableadas e inalámbricas seguras IEEE 802.1X.

5. Siga las instrucciones del Asistente para completar la creación de las nuevas directivas.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configuración de NPS para omitir las propiedades de acceso telefónico de cuentas de usuario

Utilice este procedimiento para configurar una directiva de red de NPS para omitir las propiedades de acceso telefónico de las cuentas de usuario en Active Directory durante el proceso de autorización. Las cuentas de usuario de Active Directory usuarios y equipos tienen propiedades de marcado que NPS evalúa durante el proceso de autorización, a menos que la propiedad **permiso de acceso a la red** de la cuenta de usuario esté configurada para **controlar el acceso a través de la Directiva de red NPS**.

Hay dos circunstancias en las que puede que desee configurar NPS para que omita las propiedades de acceso telefónico de las cuentas de usuario en Active Directory:

- Si desea simplificar la autorización de NPS mediante la Directiva de red, pero no todas las cuentas de usuario tienen la propiedad **permiso de acceso** a la red establecida en **controlar el acceso a través de la Directiva de red NPS**. Por ejemplo, es posible que algunas cuentas de usuario tengan la propiedad **permiso de acceso a la red** de la cuenta de usuario configurada para **denegar el acceso** o permitir el **acceso**.

- Cuando otras propiedades de acceso telefónico de las cuentas de usuario no son aplicables al tipo de conexión que está configurado en la Directiva de red. Por ejemplo, las propiedades que no son la configuración de **permisos de acceso** a la red solo se aplican a conexiones VPN o de acceso telefónico, pero la Directiva de red que está creando es para conexiones de conmutadores inalámbricos o de autenticación.

Puede usar este procedimiento para configurar NPS para omitir las propiedades de acceso telefónico de la cuenta de usuario. Si una solicitud de conexión coincide con la Directiva de red donde está activada esta casilla, NPS no utiliza las propiedades de acceso telefónico de la cuenta de usuario para determinar si el usuario o el equipo tiene autorización para obtener acceso a la red. solo se usan los valores de la Directiva de red para determinar la autorización.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. Haga doble clic en **directivas**, haga clic en **directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la Directiva que desea configurar.

3. En el cuadro de diálogo **propiedades** de Directiva, en la ficha **información general** , en **permiso de acceso**, active la casilla **omitir las propiedades de acceso telefónico de cuentas de usuario** y, a continuación, haga clic en **Aceptar**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Para configurar NPS para omitir las propiedades de acceso telefónico de la cuenta de usuario



## <a name="configure-nps-for-vlans"></a>Configuración de NPS para VLAN

Mediante el uso de servidores de acceso a la red compatibles con VLAN y NPS en Windows Server 2016, puede proporcionar a los grupos de usuarios acceso solo a los recursos de red adecuados para sus permisos de seguridad. Por ejemplo, puede proporcionar a los visitantes acceso inalámbrico a Internet sin permitirles el acceso a la red de su organización.

Además, las VLAN permiten agrupar lógicamente los recursos de red que existen en diferentes ubicaciones físicas o en distintas subredes físicas. Por ejemplo, los miembros del Departamento de ventas y sus recursos de red, como equipos cliente, servidores e impresoras, pueden encontrarse en varios edificios de la organización, pero puede colocar todos estos recursos en una VLAN que use el mismo intervalo de direcciones IP. A continuación, la VLAN funciona, desde la perspectiva del usuario final, como una sola subred.

También puede usar VLAN cuando desee separar una red entre diferentes grupos de usuarios. Después de determinar cómo desea definir los grupos, puede crear grupos de seguridad en el complemento usuarios y equipos de Active Directory y, a continuación, agregar miembros a los grupos.

### <a name="configure-a-network-policy-for-vlans"></a>Configuración de una directiva de red para VLAN

Puede usar este procedimiento para configurar una directiva de red que asigne usuarios a una VLAN. Al usar hardware de red compatible con VLAN, como enrutadores, conmutadores y controladores de acceso, puede configurar la Directiva de red para indicar a los servidores de acceso que coloquen miembros de grupos de Active Directory específicos en VLAN específicas. Esta capacidad de agrupar los recursos de red de forma lógica con VLAN proporciona flexibilidad a la hora de diseñar e implementar soluciones de red.

Al configurar las opciones de una directiva de red de NPS para su uso con VLAN, debe configurar los atributos **Tunnel-Medium-Type**, **Tunnel-Pvt-Group-ID**, **Tunnel-Type**y **Tunnel-Tag**.

Este procedimiento se proporciona como una directriz; la configuración de red puede requerir una configuración diferente a la que se describe a continuación.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-a-network-policy-for-vlans"></a>Para configurar una directiva de red para VLAN

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. Haga doble clic en **directivas**, haga clic en **directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la Directiva que desea configurar.

3. En el cuadro de diálogo **propiedades** de la Directiva, haga clic en la pestaña **configuración** .

4. En **propiedades**de la Directiva, en **configuración**, en **atributos RADIUS**, asegúrese de que está seleccionado **estándar** .

5. En el panel de detalles, en **atributos**, el atributo **de tipo de servicio** se configura con un valor predeterminado de **entramado**. De forma predeterminada, para las directivas con métodos de acceso de VPN y de acceso telefónico, el atributo **Framed-Protocol** se configura con un valor de **PPP**. Para especificar los atributos de conexión adicionales necesarios para las VLAN, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar atributo RADIUS estándar** .

6. En **Agregar atributo RADIUS estándar**, en atributos, desplácese hacia abajo hasta y agregue los siguientes atributos:

    - **Tunnel: tipo medio**. Seleccione un valor adecuado para las selecciones anteriores que ha realizado para la Directiva. Por ejemplo, si la Directiva de red que está configurando es una directiva de redes inalámbricas, seleccione **valor: 802 (incluye 802 todo el formato canónico de Ethernet Plus)**.

    - **Tunnel-Pvt-Group-ID**. Escriba el entero que representa el número de VLAN al que se asignarán los miembros del grupo.

    - **Tipo de túnel**. Seleccione **redes virtuales (VLAN)**.


7. En **Agregar atributo RADIUS estándar**, haga clic en **cerrar**.

8. Si el servidor de acceso a la red (NAS) requiere el uso del atributo de la **etiqueta de túnel** , siga estos pasos para agregar el atributo de **etiqueta de túnel** a la Directiva de red. Si la documentación de NAS no menciona este atributo, no lo agregue a la Directiva. Si es necesario, agregue los atributos de la siguiente manera:

    - En **propiedades**de la Directiva, en **configuración**, en **atributos RADIUS**, haga clic en **específico del proveedor**.

    - En el panel de detalles, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar atributo específico del proveedor** .

    - En **atributos**, desplácese hacia abajo hasta y seleccione **tuneliza-etiqueta**y, a continuación, haga clic en **Agregar**. Se abre el cuadro de diálogo **información de atributos** .

    - En **valor de atributo**, escriba el valor que obtuvo de la documentación del hardware.

## <a name="configure-the-eap-payload-size"></a>Configurar el tamaño de carga de EAP

En algunos casos, los enrutadores o los firewalls quitan paquetes porque están configurados para descartar paquetes que requieren fragmentación.

Cuando se implementa NPS con directivas de red que usan el protocolo de autenticación extensible \( EAP \) con TLS de seguridad de la capa de transporte \( \) , o EAP-TLS, como método de autenticación, la \( MTU de unidad de transmisión máxima predeterminada \) que NPS usa para las cargas de EAP es de 1500 bytes.

Este tamaño máximo de la carga de EAP puede crear mensajes RADIUS que requieran la fragmentación de un enrutador o firewall entre el NPS y un cliente RADIUS. En este caso, un enrutador o firewall colocado entre el cliente RADIUS y el NPS podría descartar de forma silenciosa algunos fragmentos, lo que provoca un error de autenticación y la incapacidad del cliente de acceso para conectarse a la red.

Use el procedimiento siguiente para reducir el tamaño máximo que usa NPS para las cargas de EAP mediante el ajuste del atributo Framed-MTU en una directiva de red en un valor no superior a 1344.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-configure-the-framed-mtu-attribute"></a>Para configurar el atributo Framed-MTU

1. En el NPS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola NPS.

2. Haga doble clic en **directivas**, haga clic en **directivas de red**y, a continuación, en el panel de detalles, haga doble clic en la Directiva que desea configurar.

3. En el cuadro de diálogo **propiedades** de la Directiva, haga clic en la pestaña **configuración** .

4. En **configuración**, en **atributos RADIUS**, haga clic en **estándar**. En el panel de detalles, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar atributo RADIUS estándar** .

5. En **atributos**, desplácese hacia abajo y haga clic en **tramad-MTU**y, a continuación, haga clic en **Agregar**. Se abre el cuadro de diálogo **información de atributos** .

6. En **valor de atributo**, escriba un valor igual o menor que **1344**. Haga clic en **Aceptar**, en **cerrar**y, a continuación, en **Aceptar**.



Para obtener más información acerca de las directivas de red, consulte [directivas de red](nps-np-overview.md).

Para obtener ejemplos de sintaxis de coincidencia de patrones para especificar atributos de directiva de red, consulte [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
