---
title: Implementación de acceso inalámbrico
description: Este tema forma parte de la Guía de redes de Windows Server 2016 "Implementar mediante 802.1X basado en contraseña X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7042a501e69a69b613979229ce2e4a9d2c3e0915
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889686"
---
# <a name="wireless-access-deployment"></a>Implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para implementar el acceso inalámbrico:

- [Implementar y configurar puntos de acceso inalámbricos](#bkmk_aps)

- [Crear un grupo de seguridad de los usuarios inalámbricos](#bkmk_groups)

- [Configuración de red inalámbrica \(IEEE 802.11\) directivas](#bkmk_policies)

- [Configurar NPSs](#bkmk_nps)

- [Unir equipos inalámbricos nuevo al dominio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implementar y configurar puntos de acceso inalámbricos

Siga estos pasos para implementar y configurar tus AP inalámbricos:

- [Especifique las frecuencias de canal de AP inalámbrico](#bkmk_channel)

- [Configurar puntos de acceso inalámbricos](#bkmk_wirelessaps)

>[!NOTE]
>Los procedimientos de esta guía no incluyen instrucciones para los casos en que se abre el cuadro de diálogo **Control de cuentas de usuario** para solicitar permiso para continuar. Si aparece este cuadro de diálogo en respuesta a sus acciones mientras realiza los procedimientos de esta guía, haga clic en **Continuar**.

### <a name="bkmk_channel"></a>Especifique las frecuencias de canal de AP inalámbrico

Al implementar varios puntos de acceso inalámbricos en un único sitio geográfico, debe configurar puntos de acceso inalámbricos que tienen las señales que se superponen con frecuencias de canal único para reducir las interferencias entre puntos de acceso inalámbricos.

Puede usar las siguientes directrices para ayudarle a elegir las frecuencias de canal que no entren en conflicto con otras redes inalámbricas en la ubicación geográfica de la red inalámbrica.

- Si hay otras organizaciones que tienen oficinas en cercanos o en el mismo edificio que su organización, identifican si hay cualquier redes inalámbricas que pertenecen a las organizaciones. Descubra la colocación y el canal asignado frecuencias de su AP inalámbrico, porque debe asignar las frecuencias de canal diferentes a sus PA y deberá determinar la mejor ubicación para instalar el punto de acceso

- Identifique la superposición de las señales inalámbricas en plantas adyacentes dentro de su organización. Después de identificar las áreas de cobertura fuera y dentro de su organización, superpuestas asignar frecuencias de canal para tus AP inalámbricos, lo que garantiza dos AP inalámbricos con cobertura superpuesta se asigna las frecuencias de canal diferente.

### <a name="bkmk_wirelessaps"></a>Configurar puntos de acceso inalámbricos

Use la siguiente información junto con la documentación del producto proporcionada por el fabricante de AP inalámbrico para configurar tus AP inalámbricos.

Este procedimiento enumera los elementos que normalmente se configura un AP inalámbrico. Los nombres de elementos pueden variar según la marca y el modelo y pueden diferir de las de la lista siguiente. Para obtener información detallada, consulte la documentación de AP inalámbrica.

#### <a name="to-configure-your-wireless-aps"></a>Para configurar tus AP inalámbricos  

- **SSID**. Especifique el nombre de la red inalámbrica\(s\) \(por ejemplo, ExampleWLAN\). Este es el nombre que se anuncia a los clientes inalámbricos.

- **Cifrado**. Especificar WPA2\-Enterprise \(preferido\) o WPA\-Enterprise y cualquier AES \(preferido\) o cifrado TKIP, dependiendo de qué versiones son compatibles con su adaptadores de red del equipo cliente inalámbrico.

- **Inalámbrica de la dirección IP PA \(estático\)**. En cada punto de acceso, configurar una dirección IP estática única que se encuentre dentro del intervalo de exclusión del ámbito DHCP para la subred. Con una dirección que se excluye de la asignación DHCP impide que el servidor DHCP asigna la misma dirección IP a un equipo u otro dispositivo.

- **Máscara de subred**. Configure esto para que coincida con la configuración de máscara de subred de la LAN a la que se ha conectado el AP inalámbrico.  

- **Nombre DNS**. Algunos puntos de acceso inalámbricos pueden configurarse con un nombre DNS. El servicio DNS en la red puede resolver nombres DNS en una dirección IP. En cada punto de acceso inalámbrico que admita esta característica, escriba un nombre único para la resolución DNS.  

- **Servicio DHCP**. Si tu AP inalámbrico tiene integradas\-en el servicio DHCP, deshabilitarla.  

- **Secreto compartido**. Use un único secreto compartido para cada AP inalámbrico, a menos que se va a configurar puntos de acceso como clientes RADIUS en NPS por grupo. Si va a configurar puntos de acceso por grupo en NPS, el secreto compartido debe ser el mismo para todos los miembros del grupo. Además, cada secreto compartido que use debe ser una secuencia aleatoria de al menos de 22 caracteres que combine letras mayúsculas y minúsculas, números y signos de puntuación. Para asegurarse de aleatoriedad, puede usar un generador de caracteres aleatorios, como el generador de caracteres aleatorios que se encuentra en el NPS **configurar 802.1X** Asistente para crear los secretos compartidos.

>[!TIP]
>Registre el secreto compartido para cada AP inalámbrico y almacenarlo en una ubicación segura, como una oficina segura. Debe conocer el secreto compartido para cada AP inalámbrico al configurar clientes RADIUS en NPS.  

- **Dirección IP del servidor RADIUS**. Escriba la dirección IP del servidor que ejecuta NPS.

- **El puerto UDP\(s\)**. De forma predeterminada, NPS usa los puertos UDP 1812 y 1645 para los mensajes de autenticación y puertos UDP 1813 y 1646 para los mensajes de cuentas. Se recomienda usar estos mismos puertos UDP en tus AP, pero si tiene una razón válida para usar puertos diferentes, asegúrese de que no sólo configurar los puntos de acceso con los números de puerto nuevas pero también volver a configurar todos los NPSs a usar los mismos números de puerto como los puntos de acceso. Si los puntos de acceso y el NPSs no están configurados con los mismos puertos UDP, NPS no puede recibir ni procesar solicitudes de conexión de los puntos de acceso y se producirá un error en todos los intentos de conexión inalámbrica de la red.

- **Los VSA**. Algunos puntos de acceso inalámbricos requieren proveedor\-atributos específicos \(VSA\) para proporcionar funcionalidad de AP inalámbrica completa. Los VSA se agregan en la directiva de red NPS.

- **Filtros DHCP**. Configurar puntos de acceso inalámbricos para bloquear a clientes inalámbricos en el envío de paquetes IP del puerto UDP 68 a la red, como se documenta por el fabricante de AP inalámbrico.

- **Filtrado de DNS**. Configurar puntos de acceso inalámbricos para bloquear a clientes inalámbricos en el envío de paquetes IP del puerto TCP o UDP 53 a la red, como se documenta por el fabricante de AP inalámbrico.

## <a name="create-security-groups-for-wireless-users"></a>Crear grupos de seguridad para usuarios inalámbricos

Siga estos pasos para crear grupos de seguridad de uno o varios usuarios inalámbricos y, a continuación, agregar usuarios al grupo de seguridad usuarios inalámbricos adecuado:

- [Crear un grupo de seguridad de los usuarios inalámbricos](#bkmk_groups)

- [Agregar usuarios al grupo de seguridad inalámbrica](#bkmk_addusers)

### <a name="bkmk_groups"></a>Crear un grupo de seguridad de los usuarios inalámbricos

Puede usar este procedimiento para crear un grupo de seguridad inalámbrica en los usuarios de Active Directory y los equipos de Microsoft Management Console \(MMC\) ajustar\-en.  

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-create-a-wireless-users-security-group"></a>Para crear un grupo de seguridad usuarios inalámbricos

1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. El complemento Usuarios y equipos de usuarios de Active Directory\-en abre. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es ejemplo.com, haga clic en **ejemplo.com**.

2. En el panel de detalles a la derecha\-haga clic en la carpeta en la que desea agregar un nuevo grupo \(a la derecha, por ejemplo,\-haga clic en **usuarios**\), apunte a **New**, y, a continuación, haga clic en **grupo**.

3. En **Nuevo objeto: grupo**, en **Nombre de grupo**, escriba un nombre para el nuevo grupo. Por ejemplo, escriba **grupo inalámbrica**.

4. En **Ámbito de grupo**, seleccione una de las opciones siguientes:

    - **Dominio local**

    - **Global**

    - **Universal**

5. En **Tipo de grupo**, seleccione **Seguridad**.

6. Haga clic en **Aceptar**.

Si necesita más de un grupo de seguridad para usuarios inalámbricos, repita estos pasos para crear grupos de usuarios inalámbricos adicionales. Más adelante puede crear directivas de red individuales en NPS para aplicar diferentes condiciones y restricciones para cada grupo, que proporciona distintos permisos de acceso y reglas de conectividad.

### <a name="bkmk_addusers"></a> Agregar usuarios al grupo de seguridad inalámbrica a los usuarios

Puede usar este procedimiento para agregar un usuario, equipo o grupo al grupo de seguridad inalámbrica en los usuarios de Active Directory y los equipos de Microsoft Management Console \(MMC\) ajustar\-en.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para agregar usuarios al grupo de seguridad inalámbrica

1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Se abre MMC de Usuarios y equipos de Active Directory. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es ejemplo.com, haga clic en **ejemplo.com**.

2. En el panel de detalles, haga doble\-haga clic en la carpeta que contiene el grupo de seguridad inalámbrica.

3. En el panel de detalles a la derecha\-haga clic en el grupo de seguridad inalámbrica y, a continuación, haga clic en **propiedades**. El **propiedades** abre el cuadro de diálogo para el grupo de seguridad.

4. En el **miembros** , haga clic **agregar**y, a continuación, complete uno de los procedimientos siguientes para agregar un equipo o agregar un usuario o grupo.

##### <a name="to-add-a-user-or-group"></a>Para agregar un usuario o grupo

1. En **escriba los nombres de objeto para seleccionar**, escriba el nombre del usuario o grupo que desea agregar y, a continuación, haga clic en **Aceptar**.

2. Para asignar la pertenencia a grupos a otros usuarios o grupos, repita el paso 1 de este procedimiento.  

##### <a name="to-add-a-computer"></a>Para agregar un equipo

1. Haga clic en **Tipos de objeto**. El **tipos de objeto** abre el cuadro de diálogo.

2. En **tipos de objeto**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.

3. En **escriba los nombres de objeto para seleccionar**, escriba el nombre del equipo que desea agregar y, a continuación, haga clic en **Aceptar**.

4. Para asignar la pertenencia a grupos a otros equipos, repita los pasos 1\-3 de este procedimiento.

## <a name="bkmk_policies"></a>Configuración de red inalámbrica \(IEEE 802.11\) directivas

Siga estos pasos para configurar la red inalámbrica \(IEEE 802.11\) extensión de directiva de grupo de directivas:

- [Abrir o agregar y abrir un objeto de directiva de grupo](#bkmk_opengpme)

- [Activar la red inalámbrica predeterminada \(IEEE 802.11\) directivas](#bkmk_activate)

- [Configurar la nueva directiva de red inalámbrica](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir o agregar y abrir un objeto de directiva de grupo

De forma predeterminada, la característica de administración de directivas de grupo está instalada en equipos que ejecutan Windows Server 2016 cuando Active Directory Domain Services \(AD DS\) está instalado el rol de servidor y el servidor está configurado como un controlador de dominio. El siguiente procedimiento se describe cómo abrir la consola de administración de directivas de grupo \(GPMC\) en el controlador de dominio. El procedimiento explica cómo abrir un dominio existente\-objeto de directiva de grupo de nivel \(GPO\) para editarlo, o crear un GPO de dominio nuevo y abrirlo para su edición.

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto de directiva de grupo

1. En el controlador de dominio, haga clic en **iniciar**, haga clic en **herramientas administrativas de Windows**y, a continuación, haga clic en **Group Policy Management**. Se abre la consola de administración de directivas de grupo.  

2. En el panel izquierdo, haga doble\-haga clic en el bosque. Por ejemplo, haga doble\-haga clic en **bosque: ejemplo.com**.  

3. En el panel izquierdo, haga doble\-haga clic en **dominios**y, a continuación, haga doble\-haga clic en el dominio para el que desea administrar un objeto de directiva de grupo. Por ejemplo, haga doble\-haga clic en **ejemplo.com**.  

4. Realiza una de las siguientes acciones:

    -   **Para abrir un dominio existente\-el nivel de GPO para su edición**, haga doble clic en el dominio que contiene el objeto de directiva de grupo que desea administrar, derecha\-haga clic en la directiva de dominio que desea administrar, por ejemplo, la directiva predeterminada de dominio, y, a continuación, haga clic en **editar**. **Editor de administración de directivas de grupo** se abre.

    -   **Para crear un nuevo objeto de directiva de grupo y abrir para editar**, ¿no\-haga clic en el dominio para el que desea crear un nuevo objeto de directiva de grupo y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**.

        En **nuevo GPO**, en **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo y, a continuación, haga clic en **Aceptar**.

        Derecha\-haga clic en el nuevo objeto de directiva de grupo y, a continuación, haga clic en **editar**. **Editor de administración de directivas de grupo** se abre.

En la sección siguiente usará el Editor de administración de directivas de grupo para crear la directiva de red inalámbrica.

### <a name="bkmk_activate"></a>Activar la red inalámbrica predeterminada \(IEEE 802.11\) directivas

Este procedimiento describe cómo activar el valor predeterminado de redes inalámbricas \(IEEE 802.11\) directivas utilizando el Editor de administración de directivas de grupo \(GPME\).

>[!NOTE]
>Después de activar la **Windows Vista y versiones posteriores** versión de la red inalámbrica \(IEEE 802.11\) directivas o la **Windows XP** versión, la opción de versión es quita automáticamente de la lista de opciones cuando secundario\-haga clic en **redes inalámbricas \(IEEE 802.11\) directivas**. Esto ocurre porque después de seleccionar una versión de directiva, se agrega la directiva en el panel de detalles de GPME cuando se selecciona el **redes inalámbricas \(IEEE 802.11\) directivas** nodo. Este estado permanece a menos que elimine la directiva inalámbrica, momento en el que se devuelve la versión de directiva de red inalámbrica a la derecha\-haga clic en el menú de **red inalámbrica \(IEEE 802.11\) directivas** en GPME . Además, las directivas inalámbricas sólo aparecen en el panel de detalles GPME cuando la **red inalámbrica \(IEEE 802.11\) directivas** nodo está seleccionado.

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para activar de forma predeterminada, redes inalámbricas \(IEEE 802.11\) directivas  

1. Siga el procedimiento anterior, **para abrir o agregar y abrir un objeto de directiva de grupo** para abrir el GPME.

2. En GPME, en el panel izquierdo, haga doble\-haga clic en **configuración del equipo**, double\-haga clic en **directivas**, double\-haga clic en **configuración de Windows** y, a continuación, haga doble\-haga clic en **configuración de seguridad**.

![Directiva de grupo de 802.1X X inalámbrica](../../../media/Wireless-GP/Wireless-GP.jpg)

3. En **configuración de seguridad**, ¿no\-haga clic en **redes inalámbricas \(IEEE 802.11\) directivas**y, a continuación, haga clic en **crear una nueva directiva inalámbrica para Windows Vista y versiones posteriores**. 

![Directiva de red inalámbrica mediante 802.1X](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. El **propiedades de nueva directiva de red inalámbrica** abre el cuadro de diálogo. En **nombre de la directiva**, escriba un nombre nuevo para la directiva o mantenga el nombre predeterminado. Haga clic en **Aceptar** para guardar la directiva. La directiva predeterminada se activa y se muestran en el panel de detalles de GPME con el nuevo nombre que proporcionó o con el nombre predeterminado **nueva directiva de red inalámbrica**.

![Propiedades de nueva directiva de red inalámbrica](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. En el panel de detalles, haga doble\-haga clic en **nueva directiva de red inalámbrica** para abrirlo.

En la sección siguiente puede realizar la configuración de directiva, orden de preferencia de procesamiento de directivas y permisos de red.

### <a name="bkmk_policyconfig"></a>Configurar la nueva directiva de red inalámbrica

Puede usar los procedimientos de esta sección para configurar redes inalámbricas \(IEEE 802.11\) directiva. Esta directiva permite configurar opciones de seguridad y autenticación, administrar perfiles inalámbricos y especificar permisos para redes inalámbricas que no estén configuradas como redes preferidas.

- [Configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Establecer el orden de preferencia para los perfiles de conexión inalámbrica](#bkmk_preferenceorder)  

- [Definir permisos de red](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAP v2

Este procedimiento proporciona los pasos necesarios para configurar un PEAP\-MS\-perfil inalámbrico CHAP v2.  

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAP v2

1. En GPME, en el cuadro de diálogo de propiedades de red inalámbrica para la directiva que acaba de crear, en el **General** ficha y, en **descripción**, escriba una breve descripción de la directiva.

2. Para especificar que se usa la configuración automática de WLAN para configurar el adaptador de red inalámbrica, asegúrese de que **servicio configuración automática de WLAN de Windows de uso para los clientes** está seleccionada.  

3. En **conectarse a las redes disponibles según el orden de los siguientes perfiles**, haga clic en **agregar**y, a continuación, seleccione **infraestructura**. El **propiedades de perfil nuevo** abre el cuadro de diálogo.

4. En el**propiedades de perfil nuevo** cuadro de diálogo el **conexión** ficha la **nombre del perfil** , escriba un nombre nuevo para el perfil. Por ejemplo, escriba **ejemplo.com WLAN perfil para Windows 10**.

5. En **nombre de red\(s\) \(SSID\)**, escriba el SSID que se corresponde con el SSID configurado en tus AP inalámbricos y, a continuación, haga clic en **agregar**.

    Si tu implementación utiliza varios SSID y cada AP inalámbrico usa la misma configuración de seguridad inalámbrica, repite este paso para agregar el SSID a cada AP inalámbrico al que desees aplicar este perfil.

    Si tu implementación utiliza varios SSID y la configuración de seguridad de los SSID no coincide, configura un perfil separado para cada grupo de SSID que utilice la misma configuración de seguridad. Por ejemplo, si tiene un grupo de AP inalámbricos configurado para usar WPA2\-Enterprise y AES y otro grupo de AP inalámbricos para usar WPA\-Enterprise y TKIP, configura un perfil para cada grupo de AP inalámbricos.

6. Si el texto predeterminado **NEWSSID** está presente, selecciónelo y, a continuación, haga clic en **quitar**.

7. Si implementaste puntos de acceso inalámbrico que están configurados para suprimir la señal de difusión, selecciona **Conectarse aunque la red no sea de difusión**.

    > [!NOTE]
    > Al habilitar esta opción puede crear un riesgo de seguridad porque los clientes inalámbricos realizarán un sondeo e intenten conectarse a cualquier red inalámbrica. De manera predeterminada, esta configuración no está habilitada.  

8. Haz clic en la pestaña **Seguridad** , haz clic en **Avanzadas**y, luego, configura lo siguiente:

    1. Para configurar las opciones avanzadas de 802.1X, en **IEEE 802.1X**, activa la opción **Aplicar configuración 802.1X avanzada**.

        Cuando la avanzada de 802.1X se aplica la configuración, los valores predeterminados de **Max Eapol\-iniciar Msgs**, **período de retención**, **período de inicio**, y  **Período de autenticación** son suficientes para las implementaciones inalámbricas típicas. Por este motivo, no es necesario cambiar los valores predeterminados a menos que tenga una razón concreta para hacerlo.

    2. Para habilitar el inicio de sesión único, activa la opción **Habilitar inicio de sesión único en esta red**.

    3. Los demás valores predeterminados de **Inicio de sesión único** son suficientes para las implementaciones inalámbricas típicas.

    4. En **Movilidad rápida**, si su AP inalámbrico está configurado para pre\-autenticación, seleccione **esta red usa pre\-autenticación**.

9. Para especificar que las comunicaciones inalámbricas cumplen FIPS 140\-2 estándares, seleccionados **realizar criptografía en FIPS 140\-2 modo certificado**.

10. Haz clic en **Aceptar** para volver a la pestaña **Seguridad**. En **seleccionar los métodos de seguridad de esta red**, en **autenticación**, seleccione **WPA2\-Enterprise** si es compatible con tus inalámbrica y AP inalámbrico adaptadores de red del cliente. En caso contrario, seleccione **WPA\-Enterprise**.

11. En **cifrado**, si es compatible con tus adaptadores de red de cliente inalámbrico, selectos y AP inalámbrico **AES CCMP**. Si usa puntos de acceso y adaptadores de red inalámbrica compatibles con 802. 11ac, seleccione **AES GCMP**. Si no lo es, selecciona **TKIP**.

    > [!NOTE]  
    > La configuración de las opciones **autenticación** y **cifrado** debe coincidir con las opciones configuradas en tus AP inalámbricos. La configuración predeterminada para **modo de autenticación**, **Nº máximo de errores de autenticación**, y **almacenar en caché información de usuario para conexiones posteriores a esta red** son suficiente para las implementaciones inalámbricas típicas.  

12. En **seleccionar un método de autenticación de red**, seleccione **EAP protegido \(PEAP\)** y, a continuación, haga clic en **propiedades**. El **propiedades de EAP protegido** abre el cuadro de diálogo.

13. En **propiedades de EAP protegido**, confirme que **verificar la identidad del servidor validando el certificado** está seleccionada.

14. En **entidades de certificación raíz de confianza**, seleccione la entidad de certificación raíz de confianza \(CA\) que emitió el certificado de servidor para el NPS.

    > [!NOTE]  
    > Esta configuración limita las CA raíz en las que confían los clientes a las CA seleccionadas. Si no se selecciona ninguna CA raíz de confianza, a continuación, los clientes confiarán en que todas las CA que se muestran en su almacén de certificados de entidades de certificación raíz de confianza raíz.  

15. En el **Seleccionar método de autenticación** , seleccione **contraseña segura \(EAP\-MS\-CHAP v2\)**.

16. Haga clic en **configurar**. En el **propiedades de EAP MSCHAPv2** diálogo cuadro, compruebe **usar automáticamente mi nombre de inicio de sesión de Windows y contraseña \(y dominio, si existiera\)**  está seleccionada y haga clic en  **Aceptar**.

17. Para habilitar la reconexión rápida PEAP, asegúrese de que **Habilitar reconexión rápida** está seleccionada.

18. Para requerir TLV de cryptobinding de servidor en los intentos de conexión, seleccione **desconectar si servidor no presenta TLV de Cryptobinding**.

19. Para especificar que la identidad del usuario se enmascara en la primera fase de autenticación, seleccione **Habilitar privacidad de identidad**y en el cuadro de texto, escriba un nombre de identidad anónima o dejar en blanco el cuadro de texto.

    >[! NOTAS]
    >- Debe crearse la directiva NPS para conexiones inalámbricas 802.1X con NPS **directiva de solicitud de conexión**. Si se crea la directiva NPS mediante el uso de NPS **directiva de red**, privacidad de identidad no funcionará.
    >- Ciertos métodos EAP proporciona privacidad de identidad EAP donde vacía o una identidad anónima \(diferente de la identidad real\) se envía como respuesta a la solicitud de identidad EAP. PEAP envía la identidad dos veces durante la autenticación. En la primera fase, la identidad se envía en texto sin formato y se usa esta identidad para fines de enrutamiento, no para la autenticación de cliente. La identidad real, utilizado para la autenticación, se envía durante la segunda fase de la autenticación en el túnel seguro que se establece en la primera fase. Si **Habilitar privacidad de identidad** está activada la casilla, el nombre de usuario se sustituye por la entrada especificada en el cuadro de texto. Por ejemplo, suponga **Habilitar privacidad de identidad** está seleccionada y el alias de privacidad de identidad **anónimo** se especifica en el cuadro de texto. Para un usuario con un alias de la identidad real **jdoe@example.com**, se cambiará la identidad enviada en la primera fase de autenticación a **anonymous@example.com**. No se modifica la parte del dominio Kerberos de la identidad de la fase 1 ya que se utiliza para fines de enrutamiento.  

20. Haga clic en **Aceptar** para cerrar el **propiedades de EAP protegido** cuadro de diálogo.
21. Haga clic en **Aceptar** para cerrar el **seguridad** ficha.
22. Si desea crear perfiles adicionales, haga clic en **agregar**y, a continuación, repita los pasos anteriores, tomar decisiones diferentes para personalizar cada perfil para los clientes inalámbricos y la red al que desea aplicar el perfil de. Cuando haya terminado de agregar perfiles, haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de directiva de red inalámbrica.

En la sección siguiente puede solicitar los perfiles de directiva para obtener una seguridad óptima.

#### <a name="bkmk_preferenceorder"></a>Establecer el orden de preferencia para los perfiles de conexión inalámbrica
Puede usar este procedimiento si ha creado varios perfiles inalámbricos en la directiva de red inalámbrica y desea ordenar los perfiles de seguridad y eficacia óptima.

Para asegurarse de que los clientes inalámbricos conectan con el mayor nivel de seguridad que pueden admitir, coloque las directivas más restrictivas en la parte superior de la lista.

Por ejemplo, si tiene dos perfiles, uno para los clientes que son compatibles con WPA2 y otro para los clientes que son compatibles con WPA, coloque el perfil de WPA2 superior en la lista. Esto garantiza que los clientes que son compatibles con WPA2 usará ese método para la conexión en lugar de WPA menos seguro.

Este procedimiento proporciona los pasos para especificar el orden en que se usan los perfiles de conexión inalámbrica para conectar los clientes inalámbricos de miembros de dominio a las redes inalámbricas.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para establecer el orden de preferencia para los perfiles de conexión inalámbrica

1. En GPME, en el cuadro de diálogo de propiedades de red inalámbrica para la directiva que acaba de configurar, haga clic en el **General** ficha.

2. En el **General** ficha **conectarse a las redes disponibles según el orden de los siguientes perfiles**, seleccione el perfil que desea mover en la lista y, a continuación, haga clic en el "arriba" botón o "flecha abajo" botón para mover el perfil a la ubicación deseada en la lista.

3.  Repita el paso 2 para cada perfil que desea mover en la lista.  

4.  Haga clic en **Aceptar** para guardar los cambios.

En la sección siguiente, puede definir permisos de red para la directiva inalámbrica.

#### <a name="bkmk_permissions"></a>Definir permisos de red
Puede configurar opciones en el **permisos de red** pestaña para los miembros del dominio a qué red inalámbrica \(IEEE 802.11\) las directivas se aplican.

Solo se puede aplicar la siguiente configuración para redes inalámbricas que no estén configuradas en el **General** pestaña en el **propiedades de la directiva de red inalámbrica** página:

- Permitir o denegar las conexiones a redes inalámbricas específicas que especifican por tipo de red y el identificador \(SSID\)

- Permitir o denegar las conexiones a redes ad hoc

- Permitir o denegar las conexiones a redes de infraestructura

- Permitir o denegar a los usuarios ver los tipos de red \(ad hoc o infraestructura\) a la que se deniega el acceso

- Permitir o denegar a los usuarios para crear un perfil que se aplica a todos los usuarios

- Los usuarios solo pueden conectarse a redes permitidas mediante perfiles de directiva de grupo

Pertenencia a **Admins. del dominio**, o equivalente, es lo mínimo necesario para completar estos procedimientos.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir o denegar las conexiones a redes inalámbricas específicas

1. En GPME, en el cuadro de diálogo Propiedades de red inalámbrica, haga clic en el **permisos de red** ficha.

2. En el **permisos de red** , haga clic **agregar**. El **nueva entrada de permiso** abre el cuadro de diálogo.

3. En el **nueva entrada de permiso** cuadro de diálogo el **nombre de red \(SSID\)**  campo, escriba el SSID de la red de red que se desea definir permisos.

4.  En **tipo de red**, seleccione **infraestructura** o **Ad hoc**.

    > [!NOTE]  
    > Si no está seguro de si la red de difusión es una red ad hoc o infraestructura, puede configurar dos entradas de permisos de red, uno para cada tipo de red.

5. En **permiso**, seleccione **permitir** o **Deny**.

6. Haga clic en **Aceptar**, para volver a la **permisos de red** ficha.

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar los permisos de red adicional \(opcional\)

1.  En el **permisos de red** pestaña, configure cualquiera o todas las opciones siguientes:  

    -   Para denegar el acceso a redes ad hoc de los miembros del dominio, seleccione **impedir conexiones a ad\-redes hoc**.

    -   Para denegar el acceso a redes de infraestructura de los miembros del dominio, seleccione **impedir conexiones a redes de infraestructura**.  

    -   Para permitir que los miembros del dominio ver los tipos de red \(ad hoc o infraestructura\) a la que se deniega el acceso, seleccione **permitir al usuario ver las redes denegadas**.

    -   Para permitir a los usuarios crear perfiles que se aplican a todos los usuarios, seleccione **permitir a todos crear perfiles de todos los usuarios**.

    -   Para especificar que los usuarios solo pueden conectarse a redes permitidas con perfiles de directiva de grupo, seleccione **usar sólo perfiles de directiva de grupo para redes permitidas**.

## <a name="bkmk_nps"></a>Configurar sus NPSs
Siga estos pasos para configurar NPSs para llevar a cabo la autenticación 802.1X para acceso inalámbrico:

- [Registrar NPS en Active Directory Domain Services](#bkmk_npsreg)

- [Configurar un punto de acceso inalámbrico como cliente RADIUS NPS](#bkmk_radiusclient)

- [Crear directivas de NPS para conexiones inalámbricas 802.1X con un asistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar NPS en Active Directory Domain Services
Puede usar este procedimiento para registrar un servidor que ejecuta el servidor de directivas de red \(NPS\) en Active Directory Domain Services \(AD DS\) en el dominio donde el NPS es miembro. Para NPSs va a conceder el permiso para leer el dial\-en Propiedades de las cuentas de usuario durante el proceso de autorización, cada NPS debe estar registrado en AD DS. Registrar un NPS agrega el servidor a la **servidores RAS e IAS** grupo de seguridad en AD DS.

>[!NOTE]
>Puede instalar NPS en un controlador de dominio o en un servidor dedicado. Ejecute el siguiente comando de Windows PowerShell para instalar NPS si aún no lo ha hecho:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar un NPS en el dominio predeterminado

1. En NPS, en **administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Ajustar el NPS\-en abre.

2. Derecha\-haga clic en **NPS \(Local\)** y, a continuación, haga clic en **Registrar servidor en Active Directory**. Se abrirá el cuadro de diálogo **Servidor de directivas de redes**.

3. En **Servidor de directivas de redes**, haga clic en **Aceptar** y, a continuación, en **Aceptar** de nuevo.

### <a name="bkmk_radiusclient"></a>Configurar un punto de acceso inalámbrico como cliente RADIUS NPS
Puede usar este procedimiento para configurar un punto de acceso, también conocido como un *servidor de acceso de red \(NAS\)*, como un acceso telefónico de autenticación remota\-en el servicio de usuario \(RADIUS\) cliente mediante el complemento NPS\-en. 

>[!IMPORTANT]
>Los equipos cliente, como los equipos portátiles inalámbricos y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico 802.1X\-conmutadores compatibles con, red privada virtual \(VPN\) servidores y marcado\-servidores de seguridad, porque usan el protocolo RADIUS para comunicarse con servidores RADIUS como NPSs.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso de red como un cliente RADIUS en NPS

1. En NPS, en **administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Ajustar el NPS\-en abre.

2. En el NPS ajustar\-en, haga doble\-haga clic en **clientes y servidores RADIUS**. Derecha\-haga clic en **clientes RADIUS**y, a continuación, haga clic en **New**.

3. En **nuevo cliente RADIUS**, compruebe que la **habilitar este cliente RADIUS** casilla está activada.

4. En **nuevo cliente RADIUS**, en **Nombre_descriptivo**, escriba un nombre para mostrar para el punto de acceso inalámbrico.

    Por ejemplo, si desea agregar un punto de acceso inalámbrico \(AP\) denominado AP\-01, tipo **AP\-01**.

5. En **dirección \(IP o DNS\)**, escriba la dirección IP o nombre de dominio completo \(FQDN\) de NAS.

    Si especifica el FQDN, para comprobar que el nombre es correcto y se asigna a una dirección IP válida, haga clic en **compruebe**y, a continuación, en **Comprobar dirección**, en el **dirección** , a continuación, haga clic en  **Resolver**. Si el nombre de dominio completo que se asigna a una dirección IP válida, la dirección IP de ese servidor NAS aparecerán automáticamente en **dirección IP**. Si el FQDN no se resuelve en una dirección IP que recibirá un mensaje que indica que este host es desconocido. Si esto ocurre, compruebe que tiene el nombre correcto de Asia Pacífico y que el punto de acceso está encendido y conectado a la red.  

    Haga clic en **Aceptar** para cerrar **Comprobar dirección**.  

6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las siguientes acciones:  

    -   Para configurar manualmente un secreto compartido, seleccione **Manual**y, a continuación, en **secreto compartido**, escriba la contraseña segura que también se especifica en el servidor NAS. Vuelva a escribir el secreto compartido en **Confirmar secreto compartido**.  

    -   Para generar automáticamente un secreto compartido, seleccione el **generar** casilla de verificación y, a continuación, haga clic en el **generar** botón. Guarde el secreto compartido generado y, a continuación, usar ese valor para configurar el servidor NAS para que pueda comunicarse con NPS.  

        >[!IMPORTANT]
        >El secreto compartido que especifique para su Asia Pacífico virtual en NPS debe coincidir exactamente con el secreto compartido que está configurado en tu real AP inalámbrico Si usa la opción de NPS para generar un secreto compartido, debe configurar el AP inalámbrico real que coincida con el secreto compartido que se generó por NPS.

7. En **nuevo cliente RADIUS**, en el **avanzadas** ficha **nombre de proveedor**, especifique el nombre del fabricante NAS. Si no estás seguro de que el nombre del fabricante NAS, seleccione **estándar RADIUS**.

8. En **opciones adicionales**, si está utilizando los métodos de autenticación que no sean EAP y PEAP y si el servidor NAS admite el uso del atributo de autenticador de mensaje, seleccione **mensajes de solicitud de acceso deben contener el Mensaje\-atributo de autenticador**.

9. Haga clic en **Aceptar**. El servidor NAS aparece en la lista de clientes RADIUS configurado en el NPS.

### <a name="bkmk_npspolicy"></a>Crear directivas NPS para conexiones inalámbricas 802.1X con un asistente
Puede usar este procedimiento para crear directivas de solicitud de la conexión y directivas necesarias para implementar cualquier 802.1X de red\-puntos de acceso inalámbricos como acceso telefónico de autenticación remota\-en el servicio de usuario \(RADIUS\) los clientes al servidor RADIUS ejecuta el servidor de directivas de red \(NPS\).  
Después de ejecutar al asistente, se crean las siguientes directivas:

- Directiva de solicitud de una conexión

- Una directiva de red

>[!NOTE]
>Puede ejecutar al Asistente para nuevo IEEE 802.1X proteger conexiones cableadas e inalámbricas conexiones cada vez que necesita crear nuevas directivas para acceso autenticado mediante 802.1X.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Crear directivas para 802.1X inalámbrico autenticado mediante un asistente

1. Abra el NPS ajustar\-en. Si aún no está seleccionada, haga clic en **NPS \(Local\)**. Si está ejecutando el complemento MMC NPS\-en y desea crear directivas en un NPS remoto, seleccione el servidor.

2. En **Introducción**, en **configuración estándar**, seleccione **servidor RADIUS para conexiones cableadas o inalámbricas 802.1X**. El texto y los vínculos debajo del texto cambiarán para reflejar la selección.

3. Haga clic en **configurar 802.1X X**. Se abre el Asistente para configurar 802.1X X.

4.  En el **Seleccionar tipo de conexiones 802.1X** página del asistente, en **tipo de las 802.1X conexiones**, seleccione **conexiones inalámbricas seguras**y en **nombre**, escriba un nombre para la directiva o deje el nombre predeterminado **conexiones inalámbricas seguras**. Haz clic en **Siguiente**.

5.  En el **especificar conmutadores 802.1X** página del asistente, en **clientes RADIUS**, 802.1X todas de conmutadores y puntos de acceso inalámbrico que haya agregado como clientes RADIUS en el complemento NPS\-en que se muestran. Lleve a cabo cualquiera de las siguientes acciones:

    -   Para agregar servidores de acceso de red adicional \(NAS\), como puntos de acceso inalámbricos en **clientes RADIUS**, haga clic en **agregar**y, a continuación, en **nuevo cliente RADIUS**, escriba la información para: **Nombre descriptivo**, **dirección \(IP o DNS\)**, y **secreto compartido**.

    -   Para modificar la configuración de cualquier NAS, en **clientes RADIUS**, seleccione el punto de acceso para el que desea modificar la configuración y, a continuación, haga clic en **editar**. Modifique la configuración según sea necesario.

    -   Para quitar un servidor NAS de la lista, en **clientes RADIUS**, seleccione el servidor NAS y, a continuación, haga clic en **quitar**.

        >[!WARNING]
        >Eliminación de un cliente RADIUS desde el **configurar 802.1X** asistente elimina el cliente de la configuración de NPS. Todas las adiciones, modificaciones y eliminaciones que se realicen dentro de la **configurar 802.1X** asistente a los clientes RADIUS se reflejan en el NPS complemento\-en, en el **clientes RADIUS** nodo bajo  **NPS** \/ **clientes y servidores RADIUS**. Por ejemplo, si utiliza el Asistente para quitar un conmutador 802.1X, el conmutador se quita también de NPS complemento\-en.

6. Haz clic en **Siguiente**. En el **configurar un método de autenticación** página del asistente, en **tipo \(según el método de acceso y configuración de red\)**, seleccione **Microsoft: EAP protegido \(PEAP\)** y, a continuación, haga clic en **configurar**.

    >[!TIP]
    >Si recibe un mensaje de error que indica que no se encuentra un certificado para su uso con el método de autenticación, y ha configurado servicios de certificados de Active Directory para emitir automáticamente certificados a servidores RAS e IAS en la red, en primer lugar Asegúrese de que ha seguido los pasos para registrar NPS en Active Directory Domain Services, a continuación, siga estos pasos para actualizar la directiva de grupo: Haga clic en **iniciar**, haga clic en **Windows System**, haga clic en **ejecutar**y en **abierto**, tipo **gpupdate**y, a continuación, presione ENTRAR. Cuando el comando devuelve resultados que indican que los usuarios y directiva de grupo del equipo han actualizado correctamente, seleccione **Microsoft: EAP protegido \(PEAP\)**  nuevo y, a continuación, haga clic en **configurar**.
    >
    >Si después de actualizar la directiva de grupo sigue recibiendo el mensaje de error que indica que no se encuentra un certificado para su uso con el método de autenticación, el certificado no se muestra porque no cumple el certificado de servidor mínima requisitos como se documenta en el Core Network Companion Guide: [Implementar certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Si esto ocurre, debe dejar la configuración de NPS, revocar el certificado emitido para NPS\(s\)y, a continuación, siga las instrucciones para configurar un nuevo certificado con la Guía de implementación de certificados de servidor.

7.  En el **editar propiedades de EAP protegido** página del asistente, en **certificado emitido**, asegúrese de que el certificado correcto de NPS está seleccionado y, a continuación, haga lo siguiente:

    >[!NOTE]
    >Compruebe que el valor de **emisor** es correcto para el certificado seleccionado en **certificado emitido**. Por ejemplo, el emisor esperado para un certificado emitido por una CA que ejecuta Servicios de certificados de Active Directory \(AD CS\) denominado corp\DC1, en el dominio "contoso.com", es **corp\-DC1\-CA**.

    -   Para permitir a los usuarios con sus equipos inalámbricos se mueven entre los puntos de acceso sin necesidad de volver a autenticar cada vez que se asocian a un nuevo punto de acceso, seleccione **Habilitar reconexión rápida**.

    -   Para especificar que conecta los clientes inalámbricos finalizará el proceso de autenticación de red si el servidor RADIUS no presenta tipo-longitud tipo\-longitud\-valor \(TLV de Cryptobinding\), seleccione **desconectar Los clientes sin Cryptobinding**.  

    -   Para modificar la directiva de configuración para el EAP de tipo, en **tipos de EAP**, haga clic en **editar**, en **propiedades de EAP MSCHAPv2**, modifique la configuración según sea necesario y, a continuación, haga clic en **Aceptar**.  

8.  Haga clic en **Aceptar**. Cierra el cuadro de diálogo Editar propiedades de EAP protegido, lo que le lleva a la **configurar 802.1X** asistente. Haz clic en **Siguiente**.

9. En **especificar grupos de usuarios**, haga clic en **agregar**y, a continuación, escriba el nombre del grupo de seguridad que ha configurado para los clientes inalámbricos de los usuarios de Active Directory y ajustar los equipos\-en. Por ejemplo, si denomina al grupo de seguridad inalámbrica de grupo inalámbrica, escriba **grupo inalámbrica**. Haz clic en **Siguiente**.

10. Haga clic en **configurar** para configurar atributos RADIUS estándar y proveedor\-atributos específicos de LAN virtual \(VLAN\) según sea necesario y como especificado por la documentación proporcionada por el proveedor de hardware de AP inalámbrico. Haz clic en **Siguiente**.

11. Revise los detalles de resumen de configuración y, a continuación, haga clic en **finalizar**.

Ahora se crean las directivas NPS, y puede pasar a unir los equipos inalámbricos al dominio.

## <a name="bkmk_domain"></a>Unir equipos inalámbricos nuevo al dominio
El método más sencillo para unir equipos inalámbricos nuevo al dominio consiste en conectar físicamente el equipo a un segmento de la red LAN cableada \(un segmento no se controla mediante un conmutador 802.1X\) antes de unir el equipo al dominio. Esto es más fácil porque la configuración de directiva de grupo inalámbrica se aplican de forma automática e inmediata y, si ha implementado su propio PKI, el equipo recibe el certificado de CA y lo coloca en el almacén de certificados entidades emisoras raíz de confianza, permite que el cliente inalámbrico debe confiar en NPSs con certificados de servidor emitidos por una entidad de certificación.

Del mismo modo, después de un nuevo equipo inalámbrico está unido al dominio, es el método preferido para los usuarios inicien sesión en el dominio realizar el registro en mediante el uso de una conexión a la red con cable.

### <a name="other-domain-join-methods"></a>Otros métodos de unión a dominio
En casos donde no es práctico unir equipos al dominio mediante una conexión Ethernet con cable, o en casos donde el usuario no puede iniciar sesión en el dominio por primera vez mediante el uso de una conexión con cable, debe usar un método alternativo.

- **Configuración del equipo de personal de TI**. Un miembro del personal de TI une un equipo inalámbrico al dominio y configura un inicio de sesión único perfil inalámbrico de bootstrap. Con este método, el Administrador de TI conecta el equipo inalámbrico a la red Ethernet con cable y une el equipo al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario se inicia el equipo sin usar una conexión con cable, las credenciales de dominio especificadas de manera manual para el inicio de sesión de usuario se usan para ambos establecer una conexión a la red inalámbrica e iniciar sesión en el dominio.

Para obtener más información, consulte la sección [unirse al dominio y el inicio de sesión utilizando el método de configuración del equipo de personal de TI](#bkmk_itstaff)

-   **Configuración del perfil inalámbrico por los usuarios de arranque**. Manualmente, el usuario configura el equipo inalámbrico con un perfil inalámbrico de bootstrap y une al dominio, según las instrucciones que se adquirió de un administrador de TI. El perfil inalámbrico de bootstrap permite al usuario establecer una conexión inalámbrica y, a continuación, unirse al dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para obtener más información, consulte la sección [unirse al dominio y el inicio de sesión mediante configuración de perfil inalámbrico de Bootstrap por los usuarios](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Unirse al dominio y el inicio de sesión utilizando el método de configuración del equipo de personal de TI
Los usuarios miembros de dominio con el dominio\-los equipos cliente inalámbricos combinada pueden usar un perfil inalámbrico temporal para conectarse a un 802.1X\-autenticado a redes inalámbricas sin necesidad de conectarse primero a la LAN con cable. Se llama a este perfil inalámbrico temporal una *perfil inalámbrico de bootstrap*.

Un perfil inalámbrico de bootstrap obliga al usuario especificar manualmente sus credenciales de cuenta de usuario de dominio y no valida el certificado de Remote Authentication Dial\-en el servicio de usuario \(RADIUS\) server ejecuta el servidor de directivas de red \(NPS\).

Una vez establecida la conectividad inalámbrica, se aplica la directiva de grupo en el equipo cliente inalámbrico y emite automáticamente un nuevo perfil inalámbrico. La nueva directiva usa las credenciales de cuenta de equipo y usuario para la autenticación de cliente. 

Además, como parte de la PEAP\-MS\-CHAP v2 autenticación mutua con el nuevo perfil en lugar del perfil de bootstrap, el cliente valida las credenciales del servidor RADIUS.

Después de unir el equipo al dominio, utilice este procedimiento para configurar un inicio de sesión único perfil inalámbrico de bootstrap, antes de distribuir el equipo inalámbrico al dominio\-usuario miembro.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar un inicio de sesión único perfil inalámbrico de bootstrap

1. Crear un perfil de bootstrap mediante el procedimiento en esta guía denominada [configurar un perfil de conexión inalámbrica para PEAP\-MS\-CHAP v2](#bkmk_configureprofile)y utilice la siguiente configuración:

    - PEAP\-MS\-autenticación CHAP v2

    - Validar deshabilitado de certificado de servidor RADIUS

    - Inicio de sesión único habilitado

2. En las propiedades de la directiva de red inalámbrica dentro del que se crea el nuevo perfil de arranque, en el **General** , seleccione el perfil de bootstrap y, a continuación, haga clic en **exportar** para exportar el perfil a un recurso compartido de red, unidad flash USB u otra ubicación fácilmente accesible. El perfil se guarda como archivo *.xml a la ubicación que especifique.

3. Unir el nuevo equipo inalámbrico al dominio \(por ejemplo, mediante una conexión Ethernet que no requiere IEEE 802.1X autenticación\) y agregar el perfil inalámbrico de bootstrap al equipo mediante el **netsh de wlan Agregar perfil** comando.

    >[!NOTE]
    >Para obtener más información, vea comandos Netsh de LAN inalámbrica \(WLAN\) en [http:\/\/technet.microsoft.com\/biblioteca\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuir el nuevo equipo inalámbrico al usuario con el procedimiento para "Iniciar sesión en el dominio con equipos que ejecutan Windows 10."

Cuando el usuario arranca el equipo, Windows solicita al usuario que escriba su nombre de cuenta de usuario de dominio y la contraseña. Como inicio de sesión único está habilitado, el equipo usa las credenciales de cuenta de usuario de dominio para establecer primero una conexión con la red inalámbrica y, a continuación, inicie sesión en el dominio.

#### <a name="bkmk_w10"></a>Inicie sesión en el dominio con equipos que ejecutan Windows 10

1. Cierre la sesión del equipo o reinicie el equipo.

2. Presione cualquier tecla del teclado o haga clic en el escritorio. Aparece la pantalla de inicio de sesión con un nombre de cuenta de usuario local muestra y un campo de entrada de contraseña debajo del nombre. No iniciar sesión con la cuenta de usuario local.

3. En la esquina inferior izquierda de la pantalla, haga clic en **otro usuario**. El registro de otro usuario en la pantalla aparece con dos campos, uno para el nombre de usuario y una contraseña. A continuación la contraseña de campo es el texto **inicie sesión en:** y, a continuación, el nombre del dominio donde el equipo está unido. Por ejemplo, si el nombre de su dominio es ejemplo.com, el texto lee **inicie sesión en: EJEMPLO**.

4. En **nombre de usuario**, escriba el nombre de usuario de dominio.

5. En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

>[!NOTE]
>Si el **otro usuario** pantalla no incluye el texto **inicie sesión en:** y el nombre de dominio, debe escribir su nombre de usuario en el formato *dominio\\usuario*. Por ejemplo, para iniciar sesión en el dominio de ejemplo.com con una cuenta llamada **usuario\-01**, tipo **ejemplo\\usuario\-01**.

### <a name="bkmk_userbootstrap"></a>Unirse al dominio y el inicio de sesión mediante configuración de perfil inalámbrico de Bootstrap por los usuarios
Con este método, completa los pasos descritos en la sección pasos generales, a continuación, proporcione el dominio\-los usuarios miembros con las instrucciones sobre cómo configurar manualmente un equipo inalámbrico con un perfil inalámbrico de bootstrap. El perfil inalámbrico de bootstrap permite al usuario establecer una conexión inalámbrica y, a continuación, unirse al dominio. Después de que el equipo está unido al dominio y se reinicia, el usuario puede iniciar sesión en el dominio a través de una conexión inalámbrica.

#### <a name="general-steps"></a>Pasos generales

1. Configurar una cuenta de administrador del equipo local, en **Panel de Control**, para el usuario.

    >[!IMPORTANT]
    >Para unir un equipo a un dominio, el usuario debe ser iniciado sesión en el equipo con la cuenta de administrador local. Como alternativa, el usuario debe proporcionar las credenciales para la cuenta de administrador local durante el proceso de unión del equipo al dominio. Además, el usuario debe tener una cuenta de usuario en el dominio al que el usuario desea unir el equipo. Durante el proceso de unión del equipo al dominio, se pedirá al usuario las credenciales de cuenta de dominio \(nombre de usuario y contraseña\).

2. Proporcionar a los usuarios de dominio con las instrucciones para configurar un perfil inalámbrico de bootstrap, tal como se describe en el siguiente procedimiento **para configurar un perfil inalámbrico de bootstrap**.
3. Además, proporcionar a los usuarios tanto las credenciales del equipo local \(nombre de usuario y contraseña\)y las credenciales de dominio \(dominio nombre de cuenta de usuario y contraseña\) en el formulario *DomainName \\UserName*, así como los procedimientos "Unir el equipo al dominio," e "Iniciar sesión en el dominio," como se documentó en Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar un perfil inalámbrico de bootstrap

1. Use las credenciales proporcionadas por el Administrador de red o el soporte técnico de TI profesional para iniciar sesión en el equipo con la cuenta de administrador del equipo local.

2. Derecha\-haga clic en el icono de red en el escritorio y haga clic en **abrir Centro de redes y recursos compartidos**. Se abre **Centro de redes y recursos compartidos**. En **cambiar la configuración de red**, haga clic en **configurar una nueva conexión o red**. El **establecer una conexión o red** abre el cuadro de diálogo.

3. Haga clic en **conectarse manualmente a una red inalámbrica**y, a continuación, haga clic en **siguiente**.

4. En **conectarse manualmente a una red inalámbrica**, en **nombre de red**, escriba el nombre SSID del punto de acceso.  

5. En **tipo de seguridad**, seleccione la configuración proporcionada por el administrador.

6. En **el tipo de cifrado** y **clave de seguridad**, seleccione o escriba los valores proporcionados por el administrador.

7. Seleccione **iniciar automáticamente esta conexión**y, a continuación, haga clic en **siguiente**.

8. En **se agregó correctamente *** el SSID de red*, haga clic en **cambiar configuración de conexión**.

9. Haga clic en **cambiar configuración de conexión**. El *su SSID de red* abre el cuadro de diálogo de propiedades de red inalámbrica.

10. Haga clic en el **seguridad** ficha y, a continuación, en **elegir un método de autenticación de red**, seleccione **EAP protegido \(PEAP\)**.

11. Haga clic en **Configuración**. El **EAP protegido \(PEAP\) propiedades** abre la página.

12. En el **EAP protegido \(PEAP\) propiedades** página, asegúrese de que **Validar certificado de servidor** está desactivada, haga clic en **Aceptar** dos veces, y a continuación, haga clic en **cerrar**.

13. Windows, a continuación, intenta conectarse a la red inalámbrica. La configuración del perfil inalámbrico de bootstrap especifica que debe proporcionar sus credenciales de dominio. Cuando Windows le pide un nombre de cuenta y una contraseña, escriba sus credenciales de cuenta de dominio como sigue:  *Nombre de dominio\\nombre de usuario*, *contraseña de dominio*.

##### <a name="to-join-a-computer-to-the-domain"></a>Para unir un equipo al dominio

1. Inicie sesión en el equipo con la cuenta de administrador local.

2. En el cuadro de texto de búsqueda, escriba **PowerShell**. En los resultados de búsqueda, haga clic en **Windows PowerShell**y, a continuación, haga clic en **ejecutar como administrador**. Se abre Windows PowerShell con un símbolo del sistema con privilegios elevados.

3. En Windows PowerShell, escriba el siguiente comando y, a continuación, presione ENTRAR. Asegúrese de sustituir DomainName variable con el nombre del dominio que desea unir.
    
    Agregar equipo DomainName
    
4. Cuando se le solicite, escriba el nombre de usuario de dominio y la contraseña y haga clic en **Aceptar**.
5. Reinicie el equipo.
6. Siga las instrucciones en la sección anterior [inicie sesión en el dominio con equipos que ejecutan Windows 10](#bkmk_w10).