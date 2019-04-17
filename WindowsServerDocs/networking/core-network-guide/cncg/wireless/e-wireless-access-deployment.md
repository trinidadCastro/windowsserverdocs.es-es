---
title: Implementación de acceso inalámbrico
description: Este tema es parte de la Guía de redes de Windows Server 2016 "Implementar basada en contraseña 802.1X X autenticados Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f78da14b91e5c1e9a2dc22f92ea95c89153befa8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment"></a>Implementación de acceso inalámbrico

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Sigue estos pasos para implementar el acceso inalámbrico:

- [Implementar y configurar puntos de acceso inalámbricos](#bkmk_aps)

- [Crear un grupo de seguridad de los usuarios de conexión inalámbrica](#bkmk_groups)

- [Configurar red inalámbrica \ (IEEE 802.11\) directivas](#bkmk_policies)

- [Configurar los servidores NPS](#bkmk_nps)

- [Únete a los nuevos equipos inalámbricos al dominio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implementar y configurar puntos de acceso inalámbricos

Sigue estos pasos para implementar y configurar puntos de acceso inalámbricos:

- [Especifica el punto de acceso inalámbrico frecuencias de canal](#bkmk_channel)

- [Configurar puntos de acceso inalámbricos](#bkmk_wirelessaps)

>[!NOTE]
>Los procedimientos descritos en esta guía no incluyen instrucciones para casos en que la **Control de cuentas de usuario** abre el cuadro de diálogo para solicitar su permiso para continuar. Si se abre este cuadro de diálogo mientras realizan los procedimientos descritos en esta guía y si el cuadro de diálogo se abrió en respuesta a las acciones, haz clic en **continuar**.

### <a name="bkmk_channel"></a>Especifica el punto de acceso inalámbrico frecuencias de canal

Cuando implementas varios puntos de acceso inalámbricos en un solo sitio geográfico, debes configurar puntos de acceso inalámbricos que tienen superpuestas señales usar frecuencias de canal exclusivo para reduce las interferencias entre puntos de acceso inalámbricos.

Puedes usar las siguientes directrices que te ayudará a elegir frecuencias de canal que no entren en conflicto con otras redes inalámbricas en la ubicación geográfica de la red inalámbrica.

- Si hay otras organizaciones que tienen oficinas cercanos o en el mismo edificio que la organización, identifican si hay cualquier propiedad de las organizaciones de redes inalámbricas. Averigua la ubicación y el canal asignado frecuencias de su punto de acceso inalámbrico porque es necesario que asignes frecuencias de canal diferente para el punto de acceso y que necesitas para determinar la mejor ubicación para instalar el punto de acceso

- Identificar superpuestas señales inalámbricas en plantas contiguas dentro de la organización. Después de identificar las áreas de cobertura fuera y dentro de la organización, superpuestas asignar frecuencias de canal para los puntos de acceso inalámbricos, asegurarse de que dos puntos de acceso inalámbricos con cobertura superpuesta se asignan frecuencias de canal diferente.

### <a name="bkmk_wirelessaps"></a>Configurar puntos de acceso inalámbricos

Usa la siguiente información junto con la documentación del producto suministrada por el fabricante de punto de acceso inalámbrico para configurar los puntos de acceso inalámbricos.

Este procedimiento enumera los elementos que normalmente se configuran en un punto de acceso inalámbrico. Los nombres de elementos pueden variar según la marca y modelo y pueden ser diferentes de las que están en la siguiente lista. Para obtener información detallada, consulta la documentación del punto de acceso inalámbrica.

#### <a name="to-configure-your-wireless-aps"></a>Para configurar los puntos de acceso inalámbricos  

- **SSID**. Especificar el nombre de la network\(s\) inalámbrica \ (por ejemplo, ExampleWLAN\). Este es el nombre que se anuncia a los clientes inalámbricos.

- **Cifrado**. Especificar \(preferred\) WPA2\ Enterprise o WPA\ de empresa y AES \(preferred\) o cifrado TKIP, dependiendo de qué versiones son compatibles con los adaptadores de red del equipo cliente inalámbrico.

- **Inalámbrica AP IP dirección \(static\)**. En cada punto de acceso, configurar una dirección IP estática única que se encuentre dentro del intervalo de exclusión del ámbito de DHCP para la subred. Con una dirección que se excluye de asignación de DHCP, impide que el servidor DHCP asignar la misma dirección IP en un equipo u otro dispositivo.

- **Máscara de subred**. Configurarlo para que coincida con la configuración de máscara de subred de la red LAN a la que se ha conectado el punto de acceso inalámbrico.  

- **El nombre DNS**. Algunos puntos de acceso inalámbricos pueden configurarse con un nombre DNS. El servicio DNS en la red puede resolver nombres DNS en una dirección IP. En cada punto de acceso inalámbrico compatible con esta característica, escribe un nombre único para la resolución DNS.  

- **Servicio DHCP**. Si el punto de acceso inalámbrico tiene un servicio DHCP built\, deshabilitarlo.  

- **Secreto compartido**. Usa un secreto compartido de RADIUS único para cada punto de acceso inalámbrico, a menos que vas a configurar puntos de acceso como clientes RADIUS en NPS por grupo. Si tienes previsto configurar puntos de acceso al grupo en NPS, el secreto compartido debe ser el mismo para todos los miembros del grupo. Además, cada secreto compartido que uses debe ser una secuencia aleatoria de al menos 22 caracteres que se mezclan mayúsculas y minúsculas, números y signos de puntuación. Para garantizar la selección aleatoria, puedes usar un generador de caracteres aleatorios, como el generador de carácter aleatorio que se encuentra en el NPS **configurar 802.1X** Asistente para crear los secretos compartidos.

>[!TIP]
>Registra la clave secreta compartida para cada punto de acceso inalámbrico y guardarla en una ubicación segura, como una oficina segura. Debes conocer el secreto compartido para cada punto de acceso inalámbrico al configurar los clientes RADIUS en NPS.  

- **Dirección IP del servidor RADIUS**. Escribe la dirección IP del servidor NPS.

- **UDP port\(s\)**. De forma predeterminada, NPS usa los puertos UDP 1812 y 1645 para mensajes de autenticación y puertos UDP 1813 y 1646 para mensajes de cuentas. Se recomienda usar estos mismos puertos UDP en los puntos de acceso, pero si tienes una razón válida para usar diferentes puertos, asegúrate de que no solo configurar los puntos de acceso con los nuevos números de puerto pero también volver a configurar todos los servidores NPS para usar los mismos números de puerto como los puntos de acceso. Si los puntos de acceso y los servidores NPS no están configurados con los mismos puertos UDP, NPS no puede recibir o procesar solicitudes de conexión de los puntos de acceso y se producirá un error en todos los intentos de conexión inalámbrica en la red.

- **VSA**. Algunos puntos de acceso inalámbricos requieren atributos específicos vendor\ \(VSAs\) para proporcionar la funcionalidad de punto de acceso inalámbrica completo. VSA se agregan en la directiva de red NPS.

- **Filtrado de DHCP**. Configurar puntos de acceso inalámbricos para bloquear a los clientes inalámbricos envíe paquetes IP de puerto UDP 68a la red, proporcionada por el fabricante del punto de acceso inalámbrico.

- **Filtrado de DNS**. Configurar puntos de acceso inalámbricos para bloquear a los clientes inalámbricos envíe paquetes IP de puerto TCP o UDP 53a la red, proporcionada por el fabricante del punto de acceso inalámbrico.

## <a name="create-security-groups-for-wireless-users"></a>Crear grupos de seguridad para los usuarios inalámbricos

Sigue estos pasos para crear uno o varios usuarios inalámbricos grupos de seguridad y, a continuación, agrega a los usuarios al grupo de seguridad de los usuarios inalámbrica adecuados:

- [Crear un grupo de seguridad de los usuarios de conexión inalámbrica](#bkmk_groups)

- [Agregar usuarios al grupo de seguridad de conexión inalámbrica](#bkmk_addusers)

### <a name="bkmk_groups"></a>Crear un grupo de seguridad de los usuarios de conexión inalámbrica

Puedes usar este procedimiento para crear un grupo de seguridad inalámbrica en los usuarios de Active Directory y equipos de Microsoft Management Console \(MMC\) snap\-de.  

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

#### <a name="to-create-a-wireless-users-security-group"></a>Para crear un grupo de seguridad de los usuarios inalámbricos

1. Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Lo equipos y usuarios de Active Directory en snap\ se abre. Si no está ya seleccionado, haz clic en el nodo de tu dominio. Por ejemplo, si el dominio es example.com, haz clic en **example.com**.

2. En el panel de detalles, right\ y haga clic en la carpeta en las que quieras agregar un nuevo grupo \ (por ejemplo, right\ clic **a los usuarios**\), apunta a **nueva**y, a continuación, haz clic en **grupo**.

3. En **nuevo objeto: grupo**, en **nombre del grupo**, escribe el nombre del nuevo grupo. Por ejemplo, escribe **grupo inalámbrica**.

4. En **ámbito de grupo**, selecciona una de las siguientes opciones:

    - **Dominio local**

    - **Global**

    - **Universal**

5. En **grupo tipo**, selecciona **seguridad**.

6. Haz clic en **Aceptar**.

Si necesitas más de un grupo de seguridad para usuarios inalámbricos, repite estos pasos para crear grupos de usuarios inalámbrica adicionales. Más adelante puedes crear directivas de red individuales en NPS para aplicar diferentes condiciones y las restricciones para cada grupo, que proporciona permisos de acceso diferentes y reglas de conectividad.

### <a name="bkmk_addusers"></a>Agregar usuarios al grupo de seguridad de los usuarios de conexión inalámbrica

Puedes usar este procedimiento para agregar un usuario, un equipo o un grupo a tu grupo de seguridad inalámbrica en los usuarios de Active Directory y equipos de Microsoft Management Console \(MMC\) snap\-de.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para agregar usuarios al grupo de seguridad inalámbrica

1. Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre el MMC de equipos y usuarios de Active Directory. Si no está ya seleccionado, haz clic en el nodo de tu dominio. Por ejemplo, si el dominio es example.com, haz clic en **example.com**.

2. En el panel de detalles, double\ y haga clic en la carpeta que contiene el grupo de seguridad inalámbrica.

3. En el panel de detalles y right\ y haga clic en el grupo de seguridad inalámbrica **propiedades**. La **propiedades** abre el cuadro de diálogo para el grupo de seguridad.

4. En la **miembros**, haga clic **agregar**y, a continuación, realice uno de los siguientes procedimientos para agregar un equipo o agregar un usuario o grupo.

##### <a name="to-add-a-user-or-group"></a>Para agregar un usuario o grupo

1. En **escriba los nombres de objeto para seleccionar**, escribe el nombre del usuario o grupo que desees agregar y, a continuación, haz clic en **Aceptar**.

2. Para asignar la pertenencia a otros usuarios o grupos, repite el paso 1 de este procedimiento.  

##### <a name="to-add-a-computer"></a>Para agregar un equipo

1. Haz clic en **tipos de objetos**. La **tipos de objeto** abre el cuadro de diálogo.

2. En **tipos de objetos**, selecciona **equipos**y, a continuación, haz clic en **Aceptar**.

3. En **escriba los nombres de objeto para seleccionar**, escribe el nombre del equipo que desea agregar y, a continuación, haz clic en **Aceptar**.

4. Para asignar la pertenencia al grupo en otros equipos, repite los pasos 3-1\ de este procedimiento.

## <a name="bkmk_policies"></a>Configurar red inalámbrica \ (IEEE 802.11\) directivas

Sigue estos pasos para configurar la red inalámbrica \ (IEEE 802.11\) extensión de directiva de grupo de directivas:

- [Abrir o agregar y abrir un objeto de directiva de grupo](#bkmk_opengpme)

- [Activar la red inalámbrica de forma predeterminada \ (IEEE 802.11\) directivas](#bkmk_activate)

- [Configurar la nueva directiva de red inalámbrica](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir o agregar y abrir un objeto de directiva de grupo

De manera predeterminada, la característica de administración de directivas de grupo está instalada en equipos que ejecutan Windows Server 2016 cuando se instala el rol de servidor de servicios de dominio de Active Directory \(AD DS\) y el servidor está configurado como un controlador de dominio. El siguiente procedimiento que se describe cómo abrir la \(GPMC\) consola de administración de directivas de grupo en el controlador de dominio. A continuación, el procedimiento describe cómo abrir una directiva de grupo de nivel domain\ existente objeto \(GPO\) para su edición, o crear un nuevo GPO de dominio y abrirlo y editarlo.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto de directiva de grupo

1. En el controlador de dominio, haz clic en **inicio**, haz clic en **herramientas administrativas de Windows**y, a continuación, haz clic en **Group Policy Management**. Abre la consola de administración de directivas de grupo.  

2. En el panel izquierdo, double\ y haga clic en el bosque. Por ejemplo, double\ clic **bosque: example.com**.  

3. En el panel izquierdo, pulse double\ **dominios**y, a continuación, double\ haz clic en el dominio para que va a administrar un objeto de directiva de grupo. Por ejemplo, double\ clic **example.com**.  

4. Realiza una de las siguientes acciones:

    -   **Para abrir un GPO de nivel de domain\ existente para su edición**, haz doble clic en el dominio que contiene el objeto de directiva de grupo que quieras administrar, right\ y haga clic en la directiva de dominio que quieras administrar, como la directiva predeterminada de dominio y, a continuación, haz clic en **editar**. **Editor de administración de directivas de grupo** se abre.

    -   **Para crear un nuevo objeto de directiva de grupo y abrir para su edición**, right\ y haga clic en el dominio para que desea crear un nuevo objeto de directiva de grupo y, a continuación, haz clic en **crear un GPO en este dominio y vincularlo aquí**.

        En **nuevo GPO**, en **nombre**, escribe un nombre para el nuevo objeto de directiva de grupo y, a continuación, haz clic en **Aceptar**.

        Right\ y haga clic en el nuevo objeto de directiva de grupo y después haz clic en **editar**. **Editor de administración de directivas de grupo** se abre.

En la siguiente sección, usarás el Editor de administración de directivas de grupo para crear directiva inalámbrica.

### <a name="bkmk_activate"></a>Activar la red inalámbrica de forma predeterminada \ (IEEE 802.11\) directivas

Este procedimiento describe cómo activar el valor predeterminado de red inalámbrica \ (IEEE 802.11\) directivas mediante el Editor de administración de directivas de grupo \(GPME\).

>[!NOTE]
>Después de activar la **Windows Vista y versiones posteriores** versión de la red inalámbrica \ (IEEE 802.11\) directivas o **Windows XP** versión, se quitará automáticamente la opción de versión de la lista de opciones al right\ clic **red inalámbrica \ (IEEE 802.11\) directivas**. Esto ocurre porque después de seleccionar una versión de directiva, se agrega la directiva en el panel de detalles de la GPME cuando seleccionas la **red inalámbrica \ (IEEE 802.11\) directivas** nodo. Este estado permanece a menos que elimine la directiva inalámbrica, momento en que la versión de la directiva inalámbrica volverá al menú right\ y haga clic para **red inalámbrica \ (IEEE 802.11\) directivas** en el GPME. Además, las directivas inalámbricas solo aparecen en el panel de detalles GPME cuando la **red inalámbrica \ (IEEE 802.11\) directivas** nodo está seleccionado.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para activar predeterminado red inalámbrica \ (IEEE 802.11\) directivas  

1. Seguir el procedimiento anterior, **para abrir o agregar y abrir un objeto de directiva de grupo** para abrir el GPME.

2. En GPME, en el panel izquierdo, double\ clic **configuración del equipo**, double\ y haga clic en **directivas**, double\ y haga clic en **configuración de Windows**y, a continuación, haga clic double\ **la configuración de seguridad**.

![Directiva de grupo de 802.1X X inalámbrica](../../../media/Wireless-GP/Wireless-GP.jpg)

3. En **la configuración de seguridad**, right\ clic **red inalámbrica \ (IEEE 802.11\) directivas**y, a continuación, haz clic en **crear una nueva directiva inalámbrica para Windows Vista y versiones posteriores**. 

![802.1X directiva inalámbrica](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. La **nuevas propiedades de directiva de red inalámbrica** abre el cuadro de diálogo. En **nombre de la directiva**, escribe un nombre nuevo para la directiva o mantener el nombre predeterminado. Haz clic en **Aceptar** para guardar la directiva. La directiva predeterminada se activa y se enumeran en el panel de detalles de la GPME con el nuevo nombre que proporcionaste o con el nombre predeterminado **nueva directiva de red inalámbrica**.

![Nuevas propiedades de la directiva de red inalámbrica](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. En el panel de detalles, double\ clic **nueva directiva de red inalámbrica** para abrirlo.

En la siguiente sección puede realizar la configuración de directiva, el orden de preferencia de procesamiento de directivas y permisos de red.

### <a name="bkmk_policyconfig"></a>Configurar la nueva directiva de red inalámbrica

Puedes usar los procedimientos descritos en esta sección para configurar la red inalámbrica \ (IEEE 802.11\) Directiva. Esta directiva permite configurar la configuración de seguridad y autenticación, administrar perfiles inalámbricos y especifica permisos para redes inalámbricas que no están configurados como redes preferidas.

- [Configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAPv2](#bkmk_configureprofile)  

- [Establecer el orden de preferencia de los perfiles de conexión inalámbrica](#bkmk_preferenceorder)  

- [Definir los permisos de red](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAPv2

Este procedimiento proporciona los pasos necesarios para configurar un perfil inalámbrico de PEAP\-MS\-CHAP v2.  

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAPv2

1. En GPME, en el cuadro de diálogo de propiedades de red inalámbrica de la directiva que acabas de crear, en la **General** pestaña y, en **descripción**, escribe una breve descripción de la directiva.

2. Para especificar que la configuración automática de WLAN se usa para configurar el adaptador de red inalámbrica, asegúrate de que **configuración automática de WLAN de Windows usa el servicio para clientes** está seleccionado.  

3. En **conectarse a redes disponibles en el orden de los siguientes perfiles**, haz clic en **agregar**y, a continuación, selecciona **infraestructura**. La **propiedades nuevo perfil** abre el cuadro de diálogo.

4. En la**propiedades nuevo perfil** cuadro de diálogo, en la **conexión** pestaña la **nombre del perfil**, a continuación, escribe un nombre nuevo para el perfil. Por ejemplo, escribe **Example.com perfil WLAN para Windows 10**.

5. En **red Name\(s\) \(SSID\)**, escriba el SSID que se corresponde con el SSID configurado en los puntos de acceso inalámbricos y, a continuación, haz clic en **agregar**.

    Si la implementación usa varios SSID y cada punto de acceso inalámbrico utiliza la misma configuración de seguridad inalámbrica, repite este paso para agregar el SSID para cada punto de acceso inalámbrico al que quieres aplicar este perfil.

    Si la implementación usa varios SSID y la configuración de seguridad para cada SSID no coinciden, configurar un perfil diferente para cada grupo de SSID que use la misma configuración de seguridad. Por ejemplo, si tienes un grupo de acceso inalámbrico configurados para usar WPA2\ Enterprise y AES y otro grupo de puntos de acceso inalámbricos para usar WPA\ Enterprise y TKIP, configurar un perfil para cada grupo de puntos de acceso inalámbricos.

6. Si el texto predeterminado **NEWSSID** presente, selecciónala y, a continuación, haz clic en **quitar**.

7. Si implementaste puntos de acceso inalámbrico que están configurados para suprimir la difusión de señales, seleccione **conectarse aunque la red no está difundiendo**.

    > [!NOTE]
    > Al habilitar esta opción puede crear un riesgo de seguridad porque intentará conexiones a cualquier red inalámbrica y sondeo para los clientes inalámbricos. De manera predeterminada, esta configuración no está habilitada.  

8. Haz clic en el **seguridad**, haga clic **avanzadas**y, a continuación, configura lo siguiente:

    1. Para configurar la 802.1X configuración, avanzada en **IEEE 802.1X**, selecciona **aplicar 802.1X configuración avanzada**.

        Cuando la 802.1X avanzada se aplica la configuración, los valores predeterminados de **Max Eapol\ inicio mensajes**, **período de retención**, **inicio período**, y **período de autenticación** son suficientes para las implementaciones típicas de inalámbricas. Por este motivo, no es necesario cambiar los valores predeterminados a menos que tengas un motivo específico para hacerlo.

    2. Para habilitar el inicio de sesión único, selecciona **habilitar inicio de sesión único en esta red **.

    3. Los demás valores predeterminados en **inicio de sesión único** son suficientes para las implementaciones típicas de inalámbricas.

    4. En **Movilidad rápida**, si el punto de acceso inalámbrico está configurado para la autenticación pre\, selecciona **esta red usa autenticación de pre\**.

9. Para especificar que comunicaciones inalámbricas los estándares de FIPS 140\-2, selecciona **realizar criptografía en el modo FIPS 140\ 2 certificado **.

10. Haz clic en **Aceptar** para volver a la **seguridad** pestaña. En **selecciona los métodos de seguridad para esta red**, en **autenticación**, selecciona **WPA2\ Enterprise** si es compatible con el punto de acceso inalámbrico y adaptadores de red inalámbrica de cliente. De lo contrario, selecciona **WPA\ Enterprise **.

11. En **cifrado**, si el punto de acceso inalámbrico y adaptadores de red inalámbrica del cliente, seleccionados admiten **AES-CCMP **. Si estás usando puntos de acceso y adaptadores de red inalámbrica que admiten 802.11ac, selecciona **AES-GCMP **. De lo contrario, selecciona **TKIP **.

    > [!NOTE]  
    > La configuración para ambos **autenticación** y **cifrado** debe coincidir con la configuración establecida en los puntos de acceso inalámbricos. La configuración predeterminada para **modo de autenticación**, **errores de autenticación Max**, y **almacenar en caché información de usuario para las conexiones posteriores a esta red** son suficientes para las implementaciones típicas de inalámbricas.  

12. En **selecciona un método de autenticación de red**, selecciona **EAP protegido \(PEAP\)**y, a continuación, haz clic en **propiedades **. La **propiedades de EAP protegido** abre el cuadro de diálogo.

13. En **propiedades de EAP protegido**, confirma que **para comprobar la identidad del servidor, validar el certificado** está seleccionado.

14. En **entidades de certificación raíz de confianza**, selecciona la entidad de certificación raíz de confianza \(CA\) que emitió el servidor de certificados en el servidor NPS.

    > [!NOTE]  
    > Esta configuración limita la CA raíz que confían los clientes a las entidades emisoras seleccionado. Si no se selecciona ninguna CA de raíz de confianza, los clientes confiarán que todos CA raíz mostradas en su almacén de certificados de entidades de certificación raíz de confianza.  

15. En la **selecciona el método de autenticación**, seleccione **contraseña segura \ (v2\ EAP\-MS\-CHAP)**.

16. Haz clic en **configurar**. En la **propiedades de EAP MSCHAPv2** cuadro de diálogo, compruebe **usar automáticamente el nombre de inicio de sesión de Windows y la contraseña \ (y el dominio if any\)** está seleccionado y haz clic en **Aceptar **.

17. Para habilitar la reconexión rápida PEAP, asegúrate de que **Habilitar reconexión rápida** está seleccionado.

18. Para requerir servidor TLV con enlace de intentos de conexión, selecciona **desconectar si el servidor no presenta TLV con enlace **.

19. Para especificar que se enmascara identidad de usuario en la fase uno de autenticación, selecciona **Habilitar privacidad de identidad**y en el cuadro de texto, escribe un nombre de identidad anónima, o deja que el cuadro de texto en blanco.

    >[! NOTAS]
    >- La directiva NPS para inalámbricas 802.1X debe crearse mediante NPS **directiva de solicitud de conexión **. Si la directiva NPS se crea mediante el uso de NPS **directiva de red**, y privacidad de identidad no funcionará.
    >- Privacidad de identidad EAP proporciona algunos métodos EAP dónde vacío o una identidad anónima \ (distinta de la identity\ real) se envía en respuesta a la solicitud de identidad EAP. PEAP envía la identidad dos veces durante la autenticación. En la primera fase, se envía la identidad en texto sin formato y esta identidad se usa para fines de enrutamiento, no para la autenticación de cliente. La identidad real: usado para la autenticación, se envía la segunda fase de la autenticación en el túnel seguro que se establece en la primera fase. Si **Habilitar privacidad de identidad** está seleccionada la casilla de verificación, el nombre de usuario se reemplaza con la entrada especificada en el cuadro de texto. Por ejemplo, supongamos **Habilitar privacidad de identidad** está seleccionado y el alias de privacidad de identidad **anónima** se especifica en el cuadro de texto. Para que un usuario con un alias de identidad real **jdoe@example.com**, se cambiará la identidad que se envía en la primera fase de autenticación a **anonymous@example.com**. No se modifica la parte de la identidad de la fase 1 territorio que se usan para el enrutamiento.  

20. Haz clic en **Aceptar** para cerrar la **propiedades de EAP protegido** cuadro de diálogo.
21. Haz clic en **Aceptar** para cerrar la **seguridad** pestaña.
22. Si quieres crear perfiles adicionales, haz clic en **agregar**y, a continuación, repite los pasos anteriores, las diferentes opciones para personalizar cada perfil de los clientes inalámbricos y de la red a la que deseas que el perfil que se aplica. Cuando hayas terminado de agregar perfiles, haz clic en **Aceptar** para cerrar el cuadro de diálogo de propiedades de directiva de red inalámbrica.

En la siguiente sección puede solicitar los perfiles de directiva de seguridad óptima.

#### <a name="bkmk_preferenceorder"></a>Establecer el orden de preferencia de los perfiles de conexión inalámbrica
Puedes usar este procedimiento si has creado varios perfiles inalámbricos en la directiva de red inalámbrica y desea ordenar los perfiles de seguridad y eficacia óptima.

Para garantizar que los clientes inalámbricos conectan con el máximo nivel de seguridad que se admiten, coloca las directivas restrictivas más en la parte superior de la lista.

Por ejemplo, si tienes dos perfiles, uno para los clientes que admiten WPA2 y otro para los clientes que son compatibles con WPA, coloca el perfil de WPA2 superior en la lista. Esto garantiza que los clientes que admiten WPA2 podrán usar ese método para la conexión, en lugar de WPA menos segura.

Este procedimiento proporciona los pasos para especificar el orden en que se usan perfiles de conexión inalámbrica para clientes inalámbricos de miembros de dominio de conectarse a redes inalámbricas.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para establecer el orden de preferencia para los perfiles de conexión inalámbrica

1. En GPME, en el cuadro de diálogo de propiedades de red inalámbrica de la directiva que acaba de configurar, haga clic en el **General** pestaña.

2. En la **General** pestaña **conectarse a redes disponibles en el orden de los siguientes perfiles**, selecciona el perfil que quieres mover en la lista y, a continuación, haga clic en el "flecha arriba" botón "flecha o hacia abajo" botón para mover el perfil a la ubicación deseada en la lista.

3.  Repite el paso 2 para cada perfil que quieres mover en la lista.  

4.  Haz clic en **Aceptar** para guardar todos los cambios.

En la siguiente sección, puedes definir permisos de red para la directiva inalámbrica.

#### <a name="bkmk_permissions"></a>Definir los permisos de red
Puedes establecer la configuración en el **permisos de red** ficha para los miembros de dominio a qué red inalámbrica \ (IEEE 802.11\) las directivas se aplican.

Sólo se puede aplicar la siguiente configuración para redes inalámbricas que no están configuradas en la **General** pestaña en la **propiedades de la directiva de red inalámbrica** página:

- Permitir o denegar conexiones a redes inalámbricas específicas que especifiques por tipo de red y el identificador de conjunto de servicio \(SSID\)

- Permitir o denegar conexiones a redes ad hoc

- Permitir o denegar conexiones a redes de infraestructura

- Permitir o denegar a los usuarios ver los tipos de red \ (ad hoc o infrastructure\) a la que se les deniega el acceso

- Permitir o denegar a los usuarios crear un perfil que se aplica a todos los usuarios

- Los usuarios solo pueden conectarse a redes permitidas mediante perfiles de directiva de grupo

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar estos procedimientos.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir o denegar conexiones a redes inalámbricas específicas

1. En GPME, en el cuadro de diálogo de propiedades de red inalámbrica, haz clic en el **permisos de red** pestaña.

2. En la **permisos de red**, haga clic **agregar **. La **nueva entrada de permiso** abre el cuadro de diálogo.

3. En la **nueva entrada de permiso** cuadro de diálogo, en la **nombre de red \(SSID\)** campo, escribe la red SSID de la red para las que quieras definir permisos.

4.  En **tipo de red**, selecciona **infraestructura** o **Ad hoc **.

    > [!NOTE]  
    > Si no estás seguro de si la red de emisión es una infraestructura o una red ad hoc, puedes configurar dos entradas de permisos de red, uno para cada tipo de red.

5. En **permiso**, selecciona **permitir** o **denegar **.

6. Haz clic en **Aceptar**, para volver a la **permisos de red** pestaña.

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar la red adicionales permisos \(Optional\)

1.  En la **permisos de red** pestaña, configura uno o todos los siguientes pasos:  

    -   Para denegar el acceso a redes ad hoc los miembros de dominio, selecciona **impedir conexiones a redes ad\-hoc **.

    -   Para los miembros del dominio denegar el acceso a las redes de infraestructura, selecciona **impedir conexiones a redes de infraestructura **.  

    -   Para permitir que los miembros de dominio ver los tipos de red \ (ad hoc o infrastructure\) a la que se les deniega el acceso, selecciona **permitir al usuario ver las redes denegadas **.

    -   Para permitir que los usuarios crear perfiles que se aplican a todos los usuarios, selecciona **permitir que todos los usuarios crear todos los perfiles de usuario**.

    -   Para especificar que los usuarios solamente puedan conectarte a redes permitidas mediante perfiles de directiva de grupo, selecciona **solo usar perfiles de directiva de grupo para las redes permitidas**.

## <a name="bkmk_nps"></a>Configurar los servidores NPS
Sigue estos pasos para configurar los servidores NPS para realizar la autenticación 802.1X de acceso inalámbrico:

- [Registrar NPS en los servicios de dominio de Active Directory](#bkmk_npsreg)

- [Configurar un punto de acceso inalámbrico como un cliente RADIUS de NPS](#bkmk_radiusclient)

- [Creación de directivas de NPS para inalámbricas 802.1X usar un asistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar NPS en los servicios de dominio de Active Directory
Puedes usar este procedimiento para registrar un servidor que ejecuta el servidor de directivas de red \(NPS\) en \(AD DS\) los servicios de dominio de Active Directory en el que el servidor NPS es miembro de dominio. Para que servidores NPS concederse permiso para leer las propiedades de dial\ de cuentas de usuario durante el proceso de autorización, cada servidor NPS debe registrarse en AD DS. Registrar un servidor NPS agrega el servidor para la **RAS y servidores IAS** grupo de seguridad en AD DS.

>[!NOTE]
>Puedes instalar NPS en un controlador de dominio o en un servidor dedicado. Ejecuta el siguiente comando de Windows PowerShell para instalar NPS si aún no lo ha hecho:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

#### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar un servidor NPS en su dominio predeterminado

1. En el servidor NPS, en **administrador del servidor**, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre el NPS en snap\.

2. Clic Right\ **NPS \(Local\)**y, a continuación, haz clic en **registrar el servidor en Active Directory**. La **el servidor de directivas de red** abre el cuadro de diálogo.

3. En **el servidor de directivas de red**, haz clic en **Aceptar**y, a continuación, haz clic en **Aceptar** nuevamente.

### <a name="bkmk_radiusclient"></a>Configurar un punto de acceso inalámbrico como un cliente RADIUS de NPS
Puedes usar este procedimiento para configurar un punto de acceso, también conocido como un *servidor de acceso de red \(NAS\)*, como un cliente remoto servicio de autenticación de usuario Dial\-In \(RADIUS\) utilizando el NPS en snap\. 

>[!IMPORTANT]
>Los equipos cliente, como equipos portátiles inalámbricos u otros equipos que ejecutan sistemas operativos cliente, no son clientes de RADIUS. Clientes de RADIUS son servidores de acceso de red, como los puntos de acceso inalámbrico, 802.1X\-conmutadores compatibles, servidores de red privada virtual \(VPN\) y servidores dial\, ya que usan el protocolo RADIUS para comunicarse con los servidores RADIUS, como servidores NPS.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso de red como un cliente RADIUS en NPS

1. En el servidor NPS, en **administrador del servidor**, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre el NPS en snap\.

2. En el NPS snap\ en double\ clic **clientes y servidores RADIUS**. Clic Right\ **clientes RADIUS**y, a continuación, haz clic en **nueva**.

3. En **nuevo cliente RADIUS**, comprueba que la **habilitar este cliente RADIUS** casilla está activada.

4. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escribe un nombre para mostrar para el punto de acceso inalámbrico.

    Por ejemplo, si quieres agregar un acceso inalámbrico elija \(AP\) denominado AP\-01, escriba **AP\-01**.

5. En **dirección \(IP or DNS\)**, escribe la dirección IP o completo \(FQDN\) de nombre de dominio para NAS.

    Si escribes el FQDN, para comprobar que el nombre es correcto y que se asigna a una dirección IP válida, haz clic en **comprobar**y, a continuación, en **Comprobar dirección**, en la **dirección**, a continuación, haz clic en **resolver**. Si el nombre FQDN se asigna a una dirección IP, la dirección IP de ese NAS aparecerá automáticamente en **dirección IP**. Si no se resuelve el FQDN en una dirección IP que recibirás un mensaje que indica que no host desconocido. Si esto ocurre, compruebe que tiene el nombre de punto de acceso correcto y que el punto de acceso está encendido y conectado a la red.  

    Haz clic en **Aceptar** cerrar **Comprobar dirección**.  

6. En **nuevo cliente RADIUS**, en **clave compartida**, realiza una de las siguientes acciones:  

    -   Para configurar manualmente un secreto compartido, selecciona **Manual**y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en NAS. Vuelva a escribir la clave secreta compartida en **Confirmar secreto compartido**.  

    -   Para generar automáticamente una clave secreta compartida, selecciona el **generar** y, a continuación, haz clic en el **generar** botón. Guardar la clave secreta compartida generada y, a continuación, usar ese valor para configurar el NAS para que se pueda comunicar con el servidor NPS.  

        >[!IMPORTANT]
        >El secreto compartido que especifiques para tu virtual del punto de acceso en NPS debe coincidir exactamente con el secreto compartido que está configurado en su real PA inalámbrico Si usas la opción de NPS para generar un secreto compartido, debes configurar coincidente real punto de acceso inalámbrico con el secreto compartido que se generó mediante NPS.

7. En **nuevo cliente RADIUS**, en la **avanzadas** pestaña **nombre de proveedor**, especificar el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, selecciona **estándar RADIUS**.

8. En **opciones adicionales**, si estás usando los métodos de autenticación que no sean EAP y PEAP y si el servidor NAS admite el uso del atributo de autenticación de mensaje, selecciona **mensajes de solicitud de acceso deben tener el atributo autenticador de Message\**.

9. Haz clic en **Aceptar**. NAS aparece en la lista de clientes RADIUS configurado en el servidor NPS.

### <a name="bkmk_npspolicy"></a>Creación de directivas NPS para inalámbricas 802.1X usar un asistente
Puedes usar este procedimiento para crear directivas de solicitud de la conexión y de red directivas necesarias para implementar cualquier 802.1X\-puntos de acceso inalámbrico compatible con como clientes remotos servicio de autenticación de usuario Dial\-In \(RADIUS\) al servidor RADIUS con el servidor de directivas de red \(NPS\).  
Después de ejecutar al asistente, se crean las directivas siguientes:

- Directiva de solicitud de una conexión

- Una directiva de red

>[!NOTE]
>Puedes ejecutar al Asistente para nuevo IEEE 802.1X seguro cableadas y conexiones inalámbricas cada vez que necesitas para crear nuevas directivas de autenticación 802.1X.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Crear directivas para 802.1X inalámbrica autenticado mediante un asistente

1. Abre el NPS en snap\. Si no está ya seleccionado, haz clic en **NPS \(Local\)**. Si se están ejecutando el MMC NPS snap\ en y para crear las directivas en un servidor NPS remoto, selecciona el servidor.

2. En **Introducción**, en **configuración estándar**, selecciona **RADIUS server para conexiones con cable o inalámbricas 802.1X**. El texto y los vínculos debajo del texto cambiarán para reflejar la selección.

3. Haz clic en **configurar 802.1X X**. Abre el asistente Configurar 802.1X X.

4.  En la **selecciona el tipo de conexiones 802.1X** Asistente para la página, en **tipo de conexiones de 802.1X X**, selecciona **conexiones inalámbricas seguras**y en **nombre**, escribe un nombre de la directiva o dejar el nombre predeterminado **conexiones inalámbricas seguras**. Haz clic en **siguiente**.

5.  En la **especificar conmutadores de 802.1X** Asistente para la página, en **clientes RADIUS**, todos los conmutadores y 802.1X puntos de acceso inalámbrico que haya agregado como se muestran a los clientes de RADIUS en NPS snap\ en. Realiza una de las siguientes acciones:

    -   Agregar red adicionales acceso servidores \(NASs\), como puntos de acceso inalámbricos en **clientes RADIUS**, haz clic en **agregar**y, a continuación, en **cliente RADIUS nuevo**, escribe la información de: **nombre descriptivo**, **dirección \(IP or DNS\)**, y **clave compartida**.

    -   Para modificar la configuración de cualquier NAS, en **clientes RADIUS**, selecciona el punto de acceso para el que quieres modificar la configuración y, a continuación, haz clic en **editar**. Modificar la configuración según sea necesario.

    -   Para quitar un NAS en la lista, en **clientes RADIUS**, selecciona el NAS y, a continuación, haz clic en **quitar**.

        >[!WARNING]
        >Quitar un cliente RADIUS desde el **configurar 802.1X** asistente elimina el cliente de la configuración del servidor NPS. Todas las adiciones, modificaciones y eliminaciones que realices dentro de la **configurar 802.1X** asistente a los clientes RADIUS se reflejan en el NPS snap\ en, en la **clientes RADIUS** nodo bajo **NPS** \/ **clientes y servidores RADIUS**. Por ejemplo, si usas el Asistente para quitar un 802.1X cambiar, también se elimina el modificador de NPS en snap\.

6. Haz clic en **siguiente**. En la **configurar un método de autenticación** Asistente para la página, en **tipo \ (basado en el método de red y acceso configuration\)**, selecciona **Microsoft: EAP protegido \(PEAP\)**y, a continuación, haz clic en **configurar**.

    >[!TIP]
    >Si recibes un mensaje de error que indica que no se encuentra un certificado para su uso con el método de autenticación y ha configurado los servicios de certificados de Active Directory para automáticamente emitir certificados a servidores RAS e IAS en la red, primero asegúrate de haber seguido los pasos para registrar NPS en los servicios de dominio de Active Directory y luego usa los siguientes pasos para actualizar la directiva de grupo : Haz clic en **inicio**, haz clic en **sistema Windows**, haz clic en **ejecutar**y en **abrir**, tipo **gpupdate**, y, a continuación, presione ENTRAR. Cuando el comando devuelve los resultados que indica que la directiva de grupo del equipo y usuario se han actualizado correctamente, seleccione **Microsoft: EAP protegido \(PEAP\)** de nuevo y, a continuación, haz clic en **configurar**.
    >
    >Si después de actualizar la directiva de grupo, sigues recibiendo el mensaje de error que indica que no se encuentra un certificado para su uso con el método de autenticación, el certificado no se muestra porque no cumple los requisitos de certificado de servidor mínimo que se indican en la guía básica de complemento de red: [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Si esto ocurre, debe dejar de configuración de NPS, revocar el certificado emitido para tu server\(s\) NPS y, a continuación, sigue las instrucciones para configurar un nuevo certificado con la Guía de implementación de certificados de servidor.

7.  En la **editar propiedades de EAP protegido** Asistente para la página, en **certificado emitido**, asegúrate de que el certificado de servidor NPS correcto está seleccionado y, a continuación, haz lo siguiente:

    >[!NOTE]
    >Comprueba que el valor en **emisor** es correcto para el certificado seleccionado en **certificado emitido**. Por ejemplo, es el emisor esperado para un certificado emitido por una CA que ejecuta \(AD CS\) servicios de certificados de Active Directory denominado corp\DC1, en el dominio contoso.com, **corp\-DC1\-CA**.

    -   Para permitir a los usuarios sincronizar con sus equipos inalámbricos entre puntos de acceso sin necesidad de volver a autenticarlo cada vez que se asocian a un nuevo punto de acceso, seleccione **Habilitar reconexión rápida**.

    -   Para especificar que conectar clientes inalámbricos finalizará el proceso de autenticación de red si el servidor RADIUS no presenta con enlace \(TLV\) Type\ Length\-valor, selecciona **Desconectar clientes sin con enlace**.  

    -   Para modificar la directiva de configuración para el EAP escribe, en **tipos de EAP**, haz clic en **editar**, en **propiedades de EAP MSCHAPv2**, modificar la configuración según sea necesario y, a continuación, haz clic en **Aceptar**.  

8.  Haz clic en **Aceptar**. Cierra el cuadro de diálogo Editar propiedades de EAP protegido, vuelve a la **configurar 802.1X** asistente. Haz clic en **siguiente**.

9. En **especificar grupos de usuarios**, haz clic en **agregar**y, a continuación, escribe el nombre del grupo de seguridad que has configurado para los clientes inalámbricos en los equipos en snap\ y usuarios de Active Directory. Por ejemplo, si se llama tu grupo de seguridad inalámbrica grupo inalámbrica, escriba **grupo inalámbrica**. Haz clic en **siguiente**.

10. Haz clic en **configurar** configurar los atributos estándar RADIUS y atributos vendor\ específicos para virtual \(VLAN\) LAN según sea necesario y como especificado por la documentación proporcionada por el fabricante del hardware de punto de acceso inalámbrico. Haz clic en **siguiente**.

11. Revisa los detalles de resumen de configuración y, a continuación, haz clic en **finalizar**.

Ahora se crean las directivas de NPS y puede pasar al unir inalámbricos equipos al dominio.

## <a name="bkmk_domain"></a>Únete a los nuevos equipos inalámbricos al dominio
El método más sencillo para unirte a equipos inalámbricos nuevos al dominio es adjuntar físicamente el equipo a un segmento de la red LAN con cable \ (un segmento no controlado por una X switch\ 802.1X) antes de unir el equipo al dominio. Esto es más fácil porque la configuración de directiva de grupo inalámbrica se aplican automáticamente e inmediatamente y, si se ha implementado tu propia infraestructura de clave pública, el equipo recibe el certificado de CA y lo coloca en el almacén de certificados de entidades de certificación raíz de confianza, permitiendo que el cliente inalámbrico con emitidos por su entidad emisora de certificados de servidor de confianza servidores NPS.

De igual modo, después de un nuevo equipo inalámbrico está unido al dominio, es el método preferido para los usuarios iniciar sesión en el dominio realizar el registro en mediante una conexión por cable a la red.

### <a name="other-domain-join-methods"></a>Otros métodos de unión al dominio
En casos donde no es práctico para unirte a equipos al dominio mediante una conexión Ethernet por cable o en casos donde el usuario no puede iniciar sesión en el dominio por primera vez mediante el uso de una conexión por cable, debes usar un método alternativo.

- **Configuración del equipo de personal de TI**. Un miembro del personal de TI se une a un equipo inalámbrico al dominio y configura un inicio de sesión único arrancar perfil inalámbrico. Con este método, el Administrador de TI conecta el equipo inalámbrico a la red Ethernet por cable y el equipo une al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario se inicia el equipo sin usar una conexión por cable, las credenciales de dominio especifican manualmente para el inicio de sesión de usuario se usan tanto establece una conexión a la red inalámbrica y para iniciar sesión en el dominio.

Para obtener más información, consulta la sección [unir el dominio ni iniciar sesión mediante el método de configuración del equipo de personal de TI](#bkmk_itstaff)

-   **Arrancar en configuración del perfil inalámbrico por los usuarios**. El usuario configura el equipo inalámbrico con un perfil inalámbrico de inicio y une al dominio, en función de instrucciones adquiridas a partir de un administrador de TI manualmente. El perfil inalámbrico arrancar permite al usuario establecer una conexión inalámbrica y, a continuación, unir el dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para obtener más información, consulta la sección [unir el dominio ni iniciar sesión mediante la configuración del perfil de conexión inalámbrica de inicio por los usuarios](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Unir el dominio ni iniciar sesión mediante el método de configuración del equipo de personal de TI
Los usuarios con equipos cliente inalámbricos domain\ Unidos a un miembro del dominio pueden utilizar un perfil inalámbrico temporal para conectarse a una 802.1X\-autenticado red inalámbrica sin necesidad de conectar primero a la LAN con cable. Se llama a este perfil inalámbrico temporal un *arrancar perfil inalámbrico*.

Un perfil inalámbrico arrancar requiere que el usuario que especificar manualmente sus credenciales de cuenta de usuario de dominio y no valida el certificado del servidor remoto servicio de autenticación de usuario Dial\-In \(RADIUS\) \(NPS\) el servidor de directivas de red.

Una vez establecida la conectividad inalámbrica, se aplica la directiva de grupo en el equipo cliente inalámbrico y un nuevo perfil inalámbrico emitido automáticamente. La nueva directiva usa las credenciales de cuenta de usuario y de equipo para autenticar el cliente. 

Además, como parte de la autenticación mutua de PEAP\-MS\-CHAP v2 con el nuevo perfil en lugar del perfil de la secuencia de arranque, el cliente valida las credenciales del servidor RADIUS.

Después de unir el equipo al dominio, usa este procedimiento para configurar un inicio de sesión único arrancar perfil inalámbrico, antes de distribuir el equipo para el usuario domain\ miembro inalámbrico.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar un inicio de sesión único arrancar perfil inalámbrico

1. Crear un perfil de arranque mediante el procedimiento descrito en esta guía denominada [configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAPv2](#bkmk_configureprofile)y usa la siguiente configuración:

    - Autenticación de PEAP\-MS\-CHAPv2

    - Validar deshabilitado de certificado de servidor de radio

    - Inicio de sesión único habilitado

2. En las propiedades de la directiva de red inalámbrica en la que creó el nuevo perfil de arrancar en el **General**, seleccione el perfil de arranque y, a continuación, haz clic en **exportar** para exportar el perfil a un recurso compartido de red, unidad flash USB, u otra ubicación de fácil acceso. El perfil se guarda como un archivo *.xml a la ubicación que especifiques.

3. Unir el nuevo equipo inalámbrico al dominio \ (por ejemplo, mediante una conexión Ethernet que no requiere IEEE 802.1X X authentication\) y agregar el perfil inalámbrico de arrancar en el equipo mediante el **netsh wlan Agregar perfil** comando.

    >[!NOTE]
    >Para obtener más información, consulta los comandos Netsh para la red de área Local inalámbrica \(WLAN\) en [http:///\/technet.microsoft.com\/library\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuir el nuevo equipo inalámbrico al usuario con el procedimiento "Iniciar la sesión en el dominio con equipos que ejecutan Windows 10."

Cuando el usuario inicie el equipo, Windows pide al usuario que escriba su nombre de cuenta de usuario y contraseña. Porque el inicio de sesión único en está habilitado, el equipo usa las credenciales de cuenta de usuario de dominio para establecer una conexión con la red inalámbrica y, a continuación, inicie sesión en el dominio.

#### <a name="bkmk_w10"></a>Iniciar sesión en el dominio con equipos que ejecutan Windows 10

1. Cerrar sesión en el equipo o reiniciar el equipo.

2. Presiona cualquier tecla del teclado o haz clic en el escritorio. Aparece la pantalla de inicio de sesión con un nombre de cuenta de usuario local que muestran y un campo de entrada de contraseña debajo del nombre. No se inicie sesión con la cuenta de usuario local.

3. En la esquina inferior izquierda de la pantalla, haz clic en **otro usuario**. El registro de otro usuario en pantalla aparece con dos campos, uno para el nombre de usuario y una contraseña. Debajo de la contraseña de campo es el texto **iniciar sesión en:** y, a continuación, el nombre del dominio donde el equipo está unido. Por ejemplo, si el dominio se denomina example.com, lee el texto **iniciar sesión en: ejemplo**.

4. En **nombre de usuario**, escribe tu nombre de usuario de dominio.

5. En **contraseña**, escriba la contraseña del dominio y, a continuación, haz clic en la flecha o presione ENTRAR.

>[!NOTE]
>Si la **otro usuario** pantalla no incluye el texto **iniciar sesión en:** y el nombre de dominio, debes escribir tu nombre de usuario con el formato *dominio\\usuario *. Por ejemplo, para iniciar sesión en el dominio example.com con una cuenta denominada **User\-01**, tipo **example\\User\-01**.

### <a name="bkmk_userbootstrap"></a>Unir el dominio ni iniciar sesión mediante la configuración del perfil de conexión inalámbrica de inicio por los usuarios
Con este método, realiza los pasos en la sección de pasos generales, a continuación, proporcionan a los usuarios domain\ miembro con las instrucciones sobre cómo configurar manualmente un equipo inalámbrico con un perfil inalámbrico arrancar. El perfil inalámbrico arrancar permite al usuario establecer una conexión inalámbrica y, a continuación, unir el dominio. Después de que el equipo está unido al dominio y se reinicia, el usuario puede iniciar sesión en el dominio a través de una conexión inalámbrica.

#### <a name="general-steps"></a>Pasos generales

1. Configurar una cuenta de administrador del equipo local, en **Panel de Control**, para que el usuario.

    >[!IMPORTANT]
    >Para unirte a un equipo a un dominio, el usuario debe iniciar sesión en el equipo con la cuenta de administrador local. Como alternativa, el usuario debe proporcionar las credenciales de la cuenta de administrador local durante el proceso de unir el equipo al dominio. Además, el usuario debe tener una cuenta de usuario en el dominio al que el usuario quiere unir el equipo. Durante el proceso de unir el equipo al dominio, se pedirá al usuario las credenciales de cuenta de dominio \ (nombre de usuario y password\).

2. Proporcionar a los usuarios de dominio con las instrucciones para configurar un perfil inalámbrico arrancar, como se indica en el siguiente procedimiento **para configurar un perfil inalámbrico arrancar **.
3. Además, los usuarios dispongan de ambas credenciales de equipo local \ (nombre de usuario y password\) y las credenciales de dominio \ (nombre de cuenta de usuario de dominio y password\) en el formulario *Nombredominio\\nombreusuario*, así como los procedimientos para "Unir el equipo al dominio," y "Iniciar sesión en el dominio," como documentadas en el equipo con Windows Server 2016 [guía básica de red ](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar un perfil inalámbrico de inicio

1. Usar las credenciales proporcionadas por el Administrador de red o el soporte técnico profesional para iniciar sesión en el equipo con la cuenta de administrador del equipo local.

2. Right\ y haga clic en el escritorio y haz clic en el icono de red **abrir Centro de redes y recursos compartidos **. **Centro de redes y recursos compartidos** se abre. En **cambiar la configuración de red**, haz clic en **configurar una nueva conexión o red **. La **establecer una conexión o red** abre el cuadro de diálogo.

3. Haz clic en **conectarse manualmente a una red inalámbrica**y, a continuación, haz clic en **siguiente **.

4. En **conectarse manualmente a una red inalámbrica**, en **nombre de red**, escribe el nombre SSID del punto de acceso.  

5. En **tipo de seguridad**, selecciona la configuración proporcionada por el administrador.

6. En **tipo de cifrado** y **clave de seguridad**, selecciona o escribe la configuración proporcionada por el administrador.

7. Selecciona **iniciar esta conexión automáticamente**y, a continuación, haz clic en **siguiente **.

8. En **agregó correctamente *** su red SSID*, haz clic en **cambiar configuración de conexión**.

9. Haz clic en **cambiar configuración de conexión **. La *su red SSID* se abre el cuadro de diálogo de propiedades de red inalámbrica.

10. Haz clic en el **seguridad** pestaña y, a continuación, en **elegir un método de autenticación de red**, selecciona **EAP protegido \(PEAP\)**.

11. Haz clic en **configuración**. La **EAP protegido \(PEAP\) propiedades** se abre la página.

12. En la **EAP protegido \(PEAP\) propiedades** página, asegúrese de que **Validar certificado de servidor** no es seleccionado, haz clic en **Aceptar** dos veces y, a continuación, haz clic en **cerrar **.

13. Windows, a continuación, intenta conectarse a la red inalámbrica. La configuración del perfil inalámbrico arrancar especifica que debe proporcionar sus credenciales de dominio. Cuando Windows le solicita un nombre de cuenta y contraseña, escribe las credenciales de cuenta de dominio como sigue: *nombre DOMINIO\\Nombre de usuario de dominio*, *contraseña del dominio *.

##### <a name="to-join-a-computer-to-the-domain"></a>Para unir un equipo al dominio

1. Iniciar sesión en el equipo con la cuenta de administrador local.

2. En el cuadro de texto de búsqueda, escribe **PowerShell **. En los resultados de búsqueda, haz clic en **Windows PowerShell**y, a continuación, haz clic en **ejecutar como administrador **. Abre Windows PowerShell con un símbolo del sistema con privilegios elevados.

3. En Windows PowerShell, escribe el siguiente comando y, a continuación, presione ENTRAR. Asegúrate de que reemplaza el nombre de variable con el nombre del dominio que quieres unirte.
    
    Add-Computer DomainName
    
4. Cuando se te pida, escribe tu nombre de usuario de dominio y la contraseña y haz clic en **Aceptar **.
5. Reinicia el equipo.
6. Sigue las instrucciones en la sección anterior [iniciar sesión en el dominio con equipos que ejecutan Windows 10](#bkmk_w10).