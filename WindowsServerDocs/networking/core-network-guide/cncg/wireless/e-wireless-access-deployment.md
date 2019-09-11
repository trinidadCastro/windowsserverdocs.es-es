---
title: Implementación de acceso inalámbrico
description: Este tema forma parte de la guía de redes de Windows Server 2016 "implementación de acceso inalámbrico autenticado mediante 802.1 X basado en contraseña".
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd951f221f1cf1c5715e26830b7da644d685634a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868975"
---
# <a name="wireless-access-deployment"></a>Implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para implementar el acceso inalámbrico:

- [Implementación y configuración de AP inalámbricos](#bkmk_aps)

- [Crear un grupo de seguridad de usuarios inalámbricos](#bkmk_groups)

- [Configuración de directivas \(IEEE 802,11\) de red inalámbrica](#bkmk_policies)

- [Configuración de NPSs](#bkmk_nps)

- [Unir nuevos equipos inalámbricos al dominio](#bkmk_domain)

## <a name="bkmk_aps"></a>Implementación y configuración de AP inalámbricos

Siga estos pasos para implementar y configurar los AP inalámbricos:

- [Especificar frecuencias de canal de AP inalámbrico](#bkmk_channel)

- [Configuración de AP inalámbricos](#bkmk_wirelessaps)

>[!NOTE]
>Los procedimientos de esta guía no incluyen instrucciones para los casos en que se abre el cuadro de diálogo **Control de cuentas de usuario** para solicitar permiso para continuar. Si aparece este cuadro de diálogo en respuesta a sus acciones mientras realiza los procedimientos de esta guía, haga clic en **Continuar**.

### <a name="bkmk_channel"></a>Especificar frecuencias de canal de AP inalámbrico

Cuando se implementan varios puntos de conexión inalámbricos en un único sitio geográfico, se deben configurar puntos de conexión inalámbricos que tengan señales superpuestas para usar frecuencias de canal únicas para reducir las interferencias entre los AP inalámbricos.

Puede usar las siguientes directrices para ayudarle a elegir las frecuencias de canal que no entran en conflicto con otras redes inalámbricas en la ubicación geográfica de la red inalámbrica.

- Si hay otras organizaciones que tengan oficinas en estrecha proximidad o en el mismo edificio que la organización, identifique si hay redes inalámbricas que pertenezcan a esas organizaciones. Averigüe la selección de ubicación y las frecuencias de canal asignadas de sus AP inalámbricos, ya que debe asignar diferentes frecuencias de canal a los de AP y debe determinar la mejor ubicación para instalar el punto de conexión.

- Identifique señales inalámbricas superpuestas en plantas adyacentes dentro de su propia organización. Después de identificar las áreas de cobertura superpuestas fuera y dentro de la organización, asigne frecuencias de canal para los AP inalámbricos, asegurándose de que se asignan frecuencias de canal diferentes a dos AP inalámbricos con cobertura superpuesta.

### <a name="bkmk_wirelessaps"></a>Configuración de AP inalámbricos

Use la siguiente información junto con la documentación del producto proporcionada por el fabricante de AP inalámbricos para configurar los AP inalámbricos.

En este procedimiento se enumeran los elementos que se configuran normalmente en un AP inalámbrico. Los nombres de elemento pueden variar según la marca y el modelo, y pueden ser diferentes de los de la lista siguiente. Para obtener información específica, consulte la documentación de AP inalámbrico.

#### <a name="to-configure-your-wireless-aps"></a>Para configurar los AP inalámbricos  

- **SSID**. Especifique el nombre\(de la red inalámbrica s\) \(, por ejemplo,\)ExampleWLAN. Este es el nombre que se anuncia a los clientes inalámbricos.

- **Cifrado**. Especifique WPA2\- \(\) Enterprise preferidoo\) WPA\-Enterprise, y el cifrado de cifrado de AES preferido o TKIP, en función de las versiones admitidas por su \( adaptadores de red de equipos cliente inalámbricos.

- **Dirección\(IP de AP inalámbrico\)estática**. En cada AP, configure una dirección IP estática única que se encuentre dentro del intervalo de exclusión del ámbito DHCP de la subred. El uso de una dirección que se excluye de la asignación por DHCP impide que el servidor DHCP asigne la misma dirección IP a un equipo o a otro dispositivo.

- **Máscara de subred**. Configúrelo para que coincida con la configuración de la máscara de subred de la LAN a la que ha conectado el AP inalámbrico.  

- **Nombre DNS**. Algunos AP inalámbricos se pueden configurar con un nombre DNS. El servicio DNS de la red puede resolver nombres DNS en una dirección IP. En cada AP inalámbrico que admita esta característica, escriba un nombre único para la resolución de DNS.  

- **Servicio DHCP**. Si el punto de conexión inalámbrico tiene\-un servicio DHCP integrado, deshabilítelo.  

- **Secreto compartido de RADIUS**. Use un secreto compartido de RADIUS único para cada punto de conexión inalámbrico a menos que tenga previsto configurar APs como clientes RADIUS en NPS por grupo. Si planea configurar APs por grupo en NPS, el secreto compartido debe ser el mismo para todos los miembros del grupo. Además, cada secreto compartido que use debe ser una secuencia aleatoria de al menos 22 caracteres que combine mayúsculas y minúsculas, números y signos de puntuación. Para garantizar la aleatoriedad, puede usar un generador de caracteres aleatorios, como el generador de caracteres aleatorios que se encuentra en el Asistente para **configurar 802.1 x** de NPS, con el fin de crear los secretos compartidos.

>[!TIP]
>Grabe el secreto compartido para cada punto de conexión inalámbrico y almacénelo en una ubicación segura, como una oficina segura. Debe conocer el secreto compartido para cada punto de conexión inalámbrico cuando configure clientes RADIUS en NPS.  

- **Dirección IP del servidor RADIUS**. Escriba la dirección IP del servidor que ejecuta NPS.

- **Puertos\(UDP.\)** De forma predeterminada, NPS usa los puertos UDP 1812 y 1645 para los mensajes de autenticación y los puertos UDP 1813 y 1646 para los mensajes de cuentas. Se recomienda que use estos mismos puertos UDP en los AP, pero si tiene una razón válida para usar puertos diferentes, asegúrese de que no solo configura los APs con los nuevos números de puerto, sino que también vuelve a configurar todos los NPSs para que usen los mismos números de puerto que el APs. Si el APs y NPSs no están configurados con los mismos puertos UDP, NPS no puede recibir ni procesar las solicitudes de conexión de los AP y se producirá un error en todos los intentos de conexión inalámbrica en la red.

- **VSA**. Algunos AP inalámbricos requieren\-los atributos \(específicos\) del proveedor VSA para proporcionar la funcionalidad de AP inalámbrico completa. Los VSA se agregan en la Directiva de red de NPS.

- **Filtrado de DHCP**. Configure los AP inalámbricos para impedir que los clientes inalámbricos envíen paquetes IP desde el puerto UDP 68 a la red, como se documenta en el fabricante de AP inalámbricos.

- **Filtrado DNS**. Configure los puntos de conexión inalámbricos para impedir que los clientes inalámbricos envíen paquetes IP desde el puerto TCP o UDP 53 a la red, como se documenta en el fabricante de AP inalámbricos.

## <a name="create-security-groups-for-wireless-users"></a>Crear grupos de seguridad para usuarios inalámbricos

Siga estos pasos para crear uno o más grupos de seguridad de usuarios inalámbricos y, a continuación, agregue usuarios al grupo de seguridad de usuarios inalámbricos adecuado:

- [Crear un grupo de seguridad de usuarios inalámbricos](#bkmk_groups)

- [Agregar usuarios al grupo de seguridad inalámbrica](#bkmk_addusers)

### <a name="bkmk_groups"></a>Crear un grupo de seguridad de usuarios inalámbricos

Puede usar este procedimiento para crear un grupo de seguridad inalámbrica en el complemento \(\-Microsoft Management Console MMC\) de Active Directory usuarios y equipos.  

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-create-a-wireless-users-security-group"></a>Para crear un grupo de seguridad de usuarios inalámbricos

1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Se abre el complemento\-Active Directory usuarios y equipos. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es ejemplo.com, haga clic en **ejemplo.com**.

2. En el panel de detalles,\-haga clic con el botón secundario en la carpeta en la que \(desea agregar un grupo\-nuevo, por ejemplo, haga clic con el botón secundario en **usuarios**\), seleccione **nuevo**y, a continuación, haga clic en **Grupo**.

3. En **Nuevo objeto: grupo**, en **Nombre de grupo**, escriba un nombre para el nuevo grupo. Por ejemplo, escriba **Grupo inalámbrico**.

4. En **Ámbito de grupo**, seleccione una de las opciones siguientes:

    - **Dominio local**

    - **Global**

    - **Mundo**

5. En **Tipo de grupo**, seleccione **Seguridad**.

6. Haga clic en **Aceptar**.

Si necesita más de un grupo de seguridad para los usuarios inalámbricos, repita estos pasos para crear grupos de usuarios inalámbricos adicionales. Más adelante, puede crear directivas de red individuales en NPS para aplicar diferentes condiciones y restricciones a cada grupo, proporcionándoles diferentes permisos de acceso y reglas de conectividad.

### <a name="bkmk_addusers"></a>Agregar usuarios al grupo de seguridad usuarios inalámbricos

Puede usar este procedimiento para agregar un usuario, equipo o grupo al grupo de seguridad inalámbrica en el complemento \(\-MMC\) de Microsoft Management Console de usuarios y equipos de Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Para agregar usuarios al grupo de seguridad inalámbrica

1. Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Se abre MMC de Usuarios y equipos de Active Directory. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es ejemplo.com, haga clic en **ejemplo.com**.

2. En el panel de detalles,\-haga doble clic en la carpeta que contiene el grupo de seguridad inalámbrica.

3. En el panel de detalles,\-haga clic con el botón secundario en el grupo de seguridad inalámbrica y, a continuación, haga clic en **propiedades**. Se abre el cuadro de diálogo **propiedades** del grupo de seguridad.

4. En la pestaña **miembros** , haga clic en **Agregar**y, a continuación, complete uno de los procedimientos siguientes para agregar un equipo o agregar un usuario o grupo.

##### <a name="to-add-a-user-or-group"></a>Para agregar un usuario o grupo

1. En **Escriba los nombres de objeto**que desea seleccionar, escriba el nombre del usuario o grupo que desea agregar y, a continuación, haga clic en **Aceptar**.

2. Para asignar la pertenencia a grupos a otros usuarios o grupos, repita el paso 1 de este procedimiento.  

##### <a name="to-add-a-computer"></a>Para agregar un equipo

1. Haga clic en **Tipos de objeto**. Se abre el cuadro de diálogo **tipos de objeto** .

2. En **tipos de objeto**, seleccione **equipos**y, a continuación, haga clic en **Aceptar**.

3. En **Escriba los nombres de objeto que desea seleccionar**, escriba el nombre del equipo que desea agregar y, a continuación, haga clic en **Aceptar**.

4. Para asignar la pertenencia a grupos a otros equipos,\-Repita los pasos del 1 al 3 de este procedimiento.

## <a name="bkmk_policies"></a>Configuración de directivas \(IEEE 802,11\) de red inalámbrica

Siga estos pasos para configurar las directivas \(de red\) inalámbrica IEEE 802,11 Directiva de grupo extensión:

- [Abrir o agregar y abrir un objeto directiva de grupo](#bkmk_opengpme)

- [Activar directivas IEEE 802,11 \(\) de red inalámbrica predeterminada](#bkmk_activate)

- [Configuración de la nueva Directiva de red inalámbrica](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Abrir o agregar y abrir un objeto directiva de grupo

De forma predeterminada, la característica de administración de directiva de grupo se instala en los equipos que ejecutan \(Windows\) Server 2016 cuando está instalado el rol de servidor de Active Directory Domain Services AD DS y el servidor está configurado como controlador de dominio. El siguiente procedimiento describe cómo abrir el consola de administración de directivas de grupo \(GPMC\) en el controlador de dominio. A continuación, el procedimiento describe cómo abrir un GPO\-\) de nivel de dominio \(existente Directiva de grupo objeto para su edición, o crear un nuevo GPO de dominio y abrirlo para su edición.

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto directiva de grupo

1. En el controlador de dominio, haga clic en **Inicio**, en **herramientas administrativas de Windows**y, a continuación, en **Administración de directiva de grupo**. Se abre el Consola de administración de directivas de grupo.  

2. En el panel izquierdo, haga\-doble clic en el bosque. Por ejemplo, haga\-doble clic en **bosque: example.com**.  

3. En el panel izquierdo, haga\-doble clic en **dominios**y,\-a continuación, haga doble clic en el dominio para el que desea administrar un objeto de directiva de grupo. Por ejemplo, haga\-doble clic en **example.com**.  

4. Realiza una de las siguientes acciones:

    -   **Para abrir un GPO de\-nivel de dominio existente para editarlo**, haga doble clic en el dominio que contiene el Directiva de grupo objeto que desea\-administrar, haga clic con el botón secundario en la Directiva de dominio que desea administrar, como la directiva predeterminada de dominio y, a continuación, Haga clic en **Editar**. Se abre **Editor de administración de directivas de grupo** .

    -   **Para crear un objeto de directiva de grupo nuevo y abrirlo para su edición**, haga clic con el botón secundario\-en el dominio para el que desea crear un nuevo objeto Directiva de grupo y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**.

        En **nuevo GPO**, en **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo y, a continuación, haga clic en **Aceptar**.

        Haga\-clic con el botón secundario en el nuevo objeto Directiva de grupo y, a continuación, haga clic en **Editar**. Se abre **Editor de administración de directivas de grupo** .

En la siguiente sección usará Editor de administración de directivas de grupo para crear la Directiva inalámbrica.

### <a name="bkmk_activate"></a>Activar directivas IEEE 802,11 \(\) de red inalámbrica predeterminada

En este procedimiento se describe cómo activar las directivas IEEE \(802,11\) de red inalámbrica predeterminada mediante el \(editor de administración de directivas de grupo\)GPME.

>[!NOTE]
>Después de activar la versión de **Windows Vista y versiones posteriores** de las directivas \(IEEE 802,11\) de red inalámbrica o de la versión de **Windows XP** , la opción versión se quita automáticamente de la lista de opciones cuando se haga\-clic con el botón derecho en **directivas IEEE 802,11 \(\) de red inalámbrica**. Esto se debe a que, después de seleccionar una versión de Directiva, la Directiva se agrega en el panel de detalles de GPME cuando se selecciona el nodo de **directivas IEEE 802,11 \(\) de red inalámbrica** . Este estado permanece a menos que elimine la Directiva inalámbrica, momento en el que la versión de la directiva\-inalámbrica vuelve al menú contextual de **las directivas de red \(\) inalámbrica IEEE 802,11** en GPME. Además, las directivas inalámbricas solo se muestran en el panel de detalles de GPME cuando se selecciona el nodo de **directivas IEEE 802,11 \(\) de red inalámbrica** .

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Para activar las directivas IEEE \(802,11\) de red inalámbrica predeterminada  

1. Siga el procedimiento anterior **para abrir o agregar y abrir un objeto Directiva de grupo** para abrir el GPME.

2. En el panel izquierdo del GPME, haga\-doble clic en **configuración del equipo**,\-haga doble clic en\- **directivas**, haga doble clic en configuración\-de **Windows**y, a continuación, haga doble clic en **seguridad. Configuración**.

![directiva de grupo inalámbrica 802.1 x](../../../media/Wireless-GP/Wireless-GP.jpg)

3. En **configuración de seguridad**,\-haga clic con el botón secundario en **directivas de \(red inalámbrica IEEE 802,11\)** y, a continuación, haga clic en **crear una nueva Directiva inalámbrica para Windows Vista y versiones posteriores**. 

![Directiva inalámbrica de 802.1 x](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. Se abre el cuadro de diálogo **propiedades de nueva Directiva de red inalámbrica** . En **nombre**de la Directiva, escriba un nombre nuevo para la Directiva o mantenga el nombre predeterminado. Haga clic en **Aceptar** para guardar la Directiva. La directiva predeterminada se activa y se muestra en el panel de detalles del GPME con el nuevo nombre que proporcionó o con el nombre predeterminado **nueva Directiva de red inalámbrica**.

![Propiedades de nueva Directiva de red inalámbrica](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. En el panel de detalles,\-haga doble clic en **nueva Directiva de red inalámbrica** para abrirla.

En la siguiente sección puede realizar la configuración de directivas, el orden de preferencia de procesamiento de directivas y los permisos de red.

### <a name="bkmk_policyconfig"></a>Configuración de la nueva Directiva de red inalámbrica

Puede usar los procedimientos de esta sección para configurar la directiva IEEE \(802,11\) de red inalámbrica. Esta directiva le permite configurar las opciones de seguridad y autenticación, administrar perfiles inalámbricos y especificar permisos para redes inalámbricas que no estén configuradas como redes preferidas.

- [Configurar un perfil de conexión inalámbrica para\-PEAP\-MS CHAP V2](#bkmk_configureprofile)  

- [Establecer el orden de preferencia para los perfiles de conexión inalámbrica](#bkmk_preferenceorder)  

- [Definir permisos de red](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurar un perfil de conexión inalámbrica para\-PEAP\-MS CHAP V2

Este procedimiento proporciona los pasos necesarios para configurar un perfil\-inalámbrico\-PEAP MS CHAP v2.  

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Para configurar un perfil de conexión inalámbrica para\-PEAP\-MS CHAP V2

1. En GPME, en el cuadro de diálogo Propiedades de red inalámbrica de la Directiva que acaba de crear, en la pestaña **General** y en **Descripción**, escriba una breve descripción de la Directiva.

2. Para especificar que se use la configuración automática de WLAN para configurar el adaptador de red inalámbrica, asegúrese de que esté seleccionada la opción **usar el servicio de configuración automática de WLAN de Windows para clientes** .  

3. En **conectarse a las redes disponibles en el orden de los siguientes perfiles**, haga clic en **Agregar**y, a continuación, seleccione **infraestructura**. Se abrirá el cuadro de diálogo **propiedades del nuevo perfil** .

4. En el cuadro de diálogo**propiedades de nuevo perfil** , en la pestaña **conexión** , en el campo **nombre del perfil** , escriba un nuevo nombre para el perfil. Por ejemplo, escriba **example.com WLAN Profile para Windows 10**.

5. En **nombre\(de\)red SSID\),escriba el SSID correspondiente al SSID configurado en los AP inalámbricos y, a continuación, haga clic en Agregar. \(**

    Si tu implementación utiliza varios SSID y cada AP inalámbrico usa la misma configuración de seguridad inalámbrica, repite este paso para agregar el SSID a cada AP inalámbrico al que desees aplicar este perfil.

    Si tu implementación utiliza varios SSID y la configuración de seguridad de los SSID no coincide, configura un perfil separado para cada grupo de SSID que utilice la misma configuración de seguridad. Por ejemplo, si tiene un grupo de AP inalámbricos configurado para usar WPA2\-Enterprise y AES, y otro grupo de AP inalámbricos para usar\-WPA Enterprise y TKIP, configure un perfil para cada grupo de AP inalámbricos.

6. Si el **NEWSSID** de texto predeterminado está presente, selecciónelo y, a continuación, haga clic en **quitar**.

7. Si implementaste puntos de acceso inalámbrico que están configurados para suprimir la señal de difusión, selecciona **Conectarse aunque la red no sea de difusión**.

    > [!NOTE]
    > La habilitación de esta opción puede suponer un riesgo para la seguridad porque los clientes inalámbricos sondearán e intentarán conexiones con cualquier red inalámbrica. De manera predeterminada, esta configuración no está habilitada.  

8. Haz clic en la pestaña **Seguridad** , haz clic en **Avanzadas**y, luego, configura lo siguiente:

    1. Para configurar las opciones avanzadas de 802.1X, en **IEEE 802.1X**, activa la opción **Aplicar configuración 802.1X avanzada**.

        Cuando se aplica la configuración avanzada de 802.1 x, los valores predeterminados de **mensajes\-de inicio de EAPOL máx**., **período de retención**, período de **Inicio**y **período de autenticación** son suficientes para las implementaciones inalámbricas típicas. Por este motivo, no es necesario cambiar los valores predeterminados a menos que tenga una razón concreta para hacerlo.

    2. Para habilitar el inicio de sesión único, activa la opción **Habilitar inicio de sesión único en esta red**.

    3. Los demás valores predeterminados de **Inicio de sesión único** son suficientes para las implementaciones inalámbricas típicas.

    4. En **itinerancia rápida**, si el punto de conexión inalámbrico está configurado para\-la autenticación previa, seleccione **esta red\-usa autenticación previa**.

9. Para especificar que las comunicaciones inalámbricas cumplan los estándares de FIPS 140\-2, seleccione **realizar\-criptografía en el modo certificado de FIPS 140 2**.

10. Haz clic en **Aceptar** para volver a la pestaña **Seguridad**. En **seleccionar los métodos de seguridad de esta red**, en **autenticación**, **Seleccione\-WPA2 Enterprise** si es compatible con los adaptadores de red de AP inalámbrico y cliente inalámbrico. En caso contrario, seleccione **WPA\-Enterprise**.

11. En **cifrado**, si los adaptadores de red de AP inalámbrico y cliente inalámbrico lo admiten, seleccione **AES-CCMP**. Si usa puntos de acceso y adaptadores de red inalámbrica compatibles con 802.11 AC, seleccione **AES-GCMP**. Si no lo es, selecciona **TKIP**.

    > [!NOTE]  
    > La configuración de **autenticación** y **cifrado** debe coincidir con la configuración establecida en los AP inalámbricos. La configuración predeterminada para el **modo de autenticación**, máximo de errores de **autenticación**y **almacenar en caché información de usuario para conexiones posteriores a esta red** es suficiente para las implementaciones inalámbricas típicas.  

12. En **Seleccione un método de autenticación de red**, seleccione  **\(EAP\)PEAP protegido**y, a continuación, haga clic en **propiedades**. Se abre el cuadro de diálogo **propiedades de EAP protegido** .

13. En **propiedades de EAP protegido**, confirme que está seleccionada **la opción comprobar la identidad del servidor validando el certificado** .

14. En **entidades de certificación raíz de confianza**, selecciona la \(entidad\) de certificación raíz de confianza que emitió el certificado de servidor a tu NPS.

    > [!NOTE]  
    > Esta configuración limita las CA raíz en las que confían los clientes a las CA seleccionadas. Si no se selecciona ninguna CA raíz de confianza, los clientes confiarán en todas las CA raíz que se enumeran en su almacén de certificados de entidades de certificación raíz de confianza.  

15. En la lista **Seleccionar método de autenticación** , selecciona **EAP \(\-de contraseña\-segura MS\)CHAP V2**.

16. Haga clic en **configurar**. En el cuadro de diálogo **propiedades de EAP MSCHAPv2** , compruebe que la opción **usar automáticamente mi \(nombre de inicio de\) sesión y contraseña y dominio de Windows si** está seleccionada y haga clic en **Aceptar**.

17. Para habilitar la reconexión rápida de PEAP, asegúrese de que está seleccionada la opción **Habilitar reconexión rápida** .

18. Para requerir el TLV de cryptobinding de servidor en los intentos de conexión, seleccione **desconectar si el servidor no presenta TLV de cryptobinding**.

19. Para especificar que la identidad del usuario se enmascara en la fase uno de la autenticación, seleccione **Habilitar privacidad de identidad**y, en el cuadro de texto, escriba un nombre de identidad anónima o deje el cuadro de texto en blanco.

    > [! APUNTE
    > - La Directiva NPS para la red inalámbrica 802.1 X debe crearse mediante la **Directiva de solicitud de conexión**NPS. Si la Directiva NPS se crea con la **Directiva de red**NPS, la privacidad de la identidad no funcionará.
    > - Determinados métodos EAP proporcionan la privacidad de identidad EAP, donde se envía una identidad \(vacía o anónima distinta de la identidad\) real en respuesta a la solicitud de identidad EAP. PEAP envía la identidad dos veces durante la autenticación. En la primera fase, la identidad se envía en texto sin formato y esta identidad se usa para fines de enrutamiento, no para la autenticación de cliente. La identidad real, que se usa para la autenticación, se envía durante la segunda fase de la autenticación, dentro del túnel seguro que se establece en la primera fase. Si la casilla **Habilitar la privacidad de identidad** está activada, el nombre de usuario se reemplaza por la entrada especificada en el cuadro de texto. Por ejemplo, supongamos que está seleccionada la opción **Habilitar privacidad de identidad** y que el alias de privacidad de identidad **anónimo** se especifica en el cuadro de texto. Para un usuario con un alias <strong>jdoe@example.com</strong>de identidad real, la identidad enviada en la primera fase de autenticación se cambiará a. <strong>anonymous@example.com</strong> La parte del dominio Kerberos de la identidad de la primera fase no se modifica, ya que se usa para el enrutamiento.  

20. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **propiedades de EAP protegido** .
21. Haga clic en **Aceptar** para cerrar la pestaña **seguridad** .
22. Si desea crear perfiles adicionales, haga clic en **Agregar**y, a continuación, repita los pasos anteriores, con distintas opciones para personalizar cada perfil de la red y los clientes inalámbricos a los que desea aplicar el perfil. Cuando haya terminado de agregar perfiles, haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de directiva de red inalámbrica.

En la sección siguiente, puede ordenar los perfiles de directiva para obtener una seguridad óptima.

#### <a name="bkmk_preferenceorder"></a>Establecer el orden de preferencia para los perfiles de conexión inalámbrica
Puede usar este procedimiento si ha creado varios perfiles inalámbricos en la Directiva de red inalámbrica y desea ordenar los perfiles para lograr una eficacia y una seguridad óptimas.

Para asegurarse de que los clientes inalámbricos se conectan con el nivel de seguridad más alto que pueden admitir, coloque las directivas más restrictivas en la parte superior de la lista.

Por ejemplo, si tiene dos perfiles, uno para los clientes que admiten WPA2 y otro para los clientes que admiten WPA, coloque el perfil de WPA2 más arriba en la lista. Esto garantiza que los clientes que admiten WPA2 usarán ese método para la conexión en lugar de la WPA menos segura.

Este procedimiento proporciona los pasos para especificar el orden en que se usan los perfiles de conexión inalámbrica para conectar los clientes inalámbricos de miembros de dominio a las redes inalámbricas.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Para establecer el orden de preferencia para los perfiles de conexión inalámbrica

1. En GPME, en el cuadro de diálogo Propiedades de red inalámbrica de la Directiva que acaba de configurar, haga clic en la pestaña **General** .

2. En la pestaña **General** , en **conectarse a las redes disponibles según el orden de los siguientes perfiles**, seleccione el perfil que desea desplace en la lista y, a continuación, haga clic en el botón "flecha arriba" o "flecha abajo" para pasar el perfil al Ubicación de la lista.

3.  Repita el paso 2 para cada perfil que desee desplazar en la lista.  

4.  Haga clic en **Aceptar** para guardar todos los cambios.

En la sección siguiente, puede definir los permisos de red para la Directiva inalámbrica.

#### <a name="bkmk_permissions"></a>Definir permisos de red
Puede configurar las opciones de la pestaña **permisos de red** para los miembros del dominio a los \(que se aplican las directivas de red inalámbrica IEEE 802,11.\)

Solo puede aplicar la configuración siguiente para redes inalámbricas que no estén configuradas en la ficha **General** de la página de propiedades de la **Directiva de red inalámbrica** :

- Permitir o denegar conexiones a redes inalámbricas específicas que se especifiquen por tipo de red y \(SSID de identificador de conjunto de servicios\)

- Permitir o denegar conexiones a redes ad hoc

- Permitir o denegar conexiones a redes de infraestructura

- Permitir o denegar a los usuarios ver \(los tipos de red\) ad hoc o la infraestructura a la que se les deniega el acceso

- Permitir o denegar a los usuarios la creación de un perfil que se aplique a todos los usuarios

- Los usuarios solo pueden conectarse a redes permitidas mediante el uso de perfiles de directiva de grupo

El requisito mínimo para completar estos procedimientos es la pertenencia al grupo **Admins**. del dominio o un grupo equivalente.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Para permitir o denegar conexiones a redes inalámbricas específicas

1. En GPME, en el cuadro de diálogo Propiedades de red inalámbrica, haga clic en la pestaña **permisos de red** .

2. En la pestaña **permisos de red** , haga clic en **Agregar**. Se abrirá el cuadro de diálogo **nueva entrada de permisos** .

3. En el cuadro de diálogo **nueva entrada de permiso** , en el campo **\(nombre de red SSID\)** , escriba el SSID de la red para el que desea definir los permisos.

4.  En **tipo de red**, seleccione **infraestructura** o **ad hoc**.

    > [!NOTE]  
    > Si no está seguro de si la red de difusión es una red de infraestructura o ad hoc, puede configurar dos entradas de permiso de red, una para cada tipo de red.

5. En **permiso**, seleccione **permitir** o **denegar**.

6. Haga clic en **Aceptar**para volver a la pestaña **permisos de red** .

##### <a name="to-specify-additional-network-permissions-optional"></a>Para especificar los permisos \(de red adicionales opcionales\)

1.  En la pestaña **permisos de red** , configure una o todas las opciones siguientes:  

    -   Para denegar el acceso de los miembros de dominio a las redes ad hoc, seleccione **impedir conexiones a redes\-ad hoc**.

    -   Para denegar el acceso de los miembros de dominio a las redes de infraestructura, seleccione **impedir conexiones a redes de infraestructura**.  

    -   Para permitir a los miembros del dominio ver los \(tipos de red ad\) hoc o la infraestructura a la que se les deniega el acceso, seleccione **permitir al usuario ver las redes denegadas**.

    -   Para permitir que los usuarios creen perfiles que se aplican a todos los usuarios, seleccione **permitir a todos crear perfiles de todos**los usuarios.

    -   Para especificar que los usuarios solo pueden conectarse a redes permitidas mediante el uso de perfiles de directiva de grupo, seleccione **usar solo perfiles de directiva de grupo para redes permitidas**.

## <a name="bkmk_nps"></a>Configuración de NPSs
Siga estos pasos para configurar NPSs para realizar la autenticación de 802.1 X para el acceso inalámbrico:

- [Registrar NPS en Active Directory Domain Services](#bkmk_npsreg)

- [Configuración de un punto de conexión inalámbrico como un cliente RADIUS NPS](#bkmk_radiusclient)

- [Creación de directivas NPS para la red inalámbrica 802.1 X mediante un asistente](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrar NPS en Active Directory Domain Services
Puede usar este procedimiento para registrar un \(servidor que ejecuta NPS\) de servidor de directivas de redes\) en Active Directory Domain Services \(AD DS en el dominio al que pertenece el NPS. Para que NPSs tenga permiso para leer las propiedades de\-acceso telefónico de las cuentas de usuario durante el proceso de autorización, cada NPS debe estar registrado en AD DS. El registro de un NPS agrega el servidor al grupo de seguridad **servidores RAS e IAS** en AD DS.

>[!NOTE]
>Puede instalar NPS en un controlador de dominio o en un servidor dedicado. Ejecute el siguiente comando de Windows PowerShell para instalar NPS si todavía no lo ha hecho:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar un NPS en su dominio predeterminado

1. En el NPS, en **Administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre el\-complemento NPS.

2. Haga\-clic con el botón secundario en  **\(NPS local\)** y, a continuación, haga clic en **registrar servidor en Active Directory**. Se abrirá el cuadro de diálogo **Servidor de directivas de redes**.

3. En **Servidor de directivas de redes**, haga clic en **Aceptar** y, a continuación, en **Aceptar** de nuevo.

### <a name="bkmk_radiusclient"></a>Configuración de un punto de conexión inalámbrico como un cliente RADIUS NPS
Puede usar este procedimiento para configurar un punto de conexión, también conocido como  *\(servidor NAS\)de acceso*a la red, como un cliente RADIUS\) de \(servicio de usuario de acceso telefónico\-de autenticación remota mediante el complemento NPS. \-en. 

>[!IMPORTANT]
>Los equipos cliente, como los equipos portátiles inalámbricos y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso\-inalámbricos, conmutadores compatibles con\) 802.1 x, servidores\-VPN de red \(privada virtual y servidores de acceso telefónico, porque usan el protocolo RADIUS para comunicarse con servidores RADIUS como NPSs.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Para agregar un servidor de acceso a la red como cliente RADIUS en NPS

1. En el NPS, en **Administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes**. Se abre el\-complemento NPS.

2. En el complemento\-NPS, haga doble\-clic en **clientes y servidores RADIUS**. Haga\-clic con el botón secundario en **clientes RADIUS**y, a continuación, haga clic en **nuevo**.

3. En **nuevo cliente RADIUS**, compruebe que la casilla **habilitar este cliente RADIUS** está activada.

4. En **nuevo cliente RADIUS**, en **nombre descriptivo**, escriba un nombre para mostrar para el punto de acceso inalámbrico.

    Por ejemplo, si desea agregar \(un AP\) de punto de acceso inalámbrico denominado AP\-01, escriba **AP\-01**.

5. En **dirección \(IP o DNS\)de dirección**, escriba la dirección IP o el FQDN \(\) del nombre de dominio completo para el NAS.

    Si escribe el FQDN, para comprobar que el nombre es correcto y se asigna a una dirección IP válida, haga clic en **comprobar**y, a continuación, en **Comprobar dirección**, en el campo **Dirección** , haga clic en **resolver**. Si el nombre de FQDN se asigna a una dirección IP válida, la dirección IP de ese NAS aparecerá automáticamente en **dirección IP**. Si el FQDN no se resuelve en una dirección IP, recibirá un mensaje que indica que no se conoce dicho host. Si esto ocurre, compruebe que tiene el nombre de AP correcto y que el AP está encendido y conectado a la red.  

    Haga clic en **Aceptar** para cerrar **Comprobar dirección**.  

6. En **nuevo cliente RADIUS**, en **secreto compartido**, realice una de las acciones siguientes:  

    -   Para configurar manualmente un secreto compartido de RADIUS, seleccione **manual**y, a continuación, en **secreto compartido**, escriba la contraseña segura que se ha escrito también en el servidor NAS. Vuelva a escribir el secreto compartido en **confirmar secreto compartido**.  

    -   Para generar automáticamente un secreto compartido, active la casilla **generar** y, a continuación, haga clic en el botón **generar** . Guarde el secreto compartido generado y, a continuación, use ese valor para configurar el servidor NAS para que pueda comunicarse con el NPS.  

        >[!IMPORTANT]
        >El secreto compartido de RADIUS que especifique para el punto de conexión virtual en NPS debe coincidir exactamente con el secreto compartido de RADIUS configurado en el punto de conexión de acceso inalámbrico real. Si usa la opción NPS para generar un secreto compartido de RADIUS, debe configurar el punto de conexión inalámbrico real coincidente con el secreto compartido de RADIUS generado por NPS.

7. En **nuevo cliente RADIUS**, en la pestaña **Opciones avanzadas** , en **nombre del proveedor**, especifique el nombre del fabricante de NAS. Si no está seguro del nombre del fabricante de NAS, seleccione **RADIUS estándar**.

8. En **opciones adicionales**, si usa métodos de autenticación que no sean EAP y PEAP, y si el NAS admite el uso del atributo de autenticador de mensaje, seleccione **los mensajes de solicitud de acceso deben\- contener el mensaje. Atributo Authenticator**.

9. Haga clic en **Aceptar**. El NAS aparece en la lista de clientes RADIUS configurados en el NPS.

### <a name="bkmk_npspolicy"></a>Creación de directivas NPS para la red inalámbrica 802.1 X mediante un asistente
Puede usar este procedimiento para crear las directivas de solicitud de conexión y las directivas de red necesarias para implementar\-puntos de acceso inalámbricos compatibles con 802.1\-x como RADIUS \(delserviciodeusuariodeaccesotelefónicodeautenticaciónremota.\) clientes del servidor RADIUS que ejecuta NPS\)de servidor \(de directivas de redes.  
Después de ejecutar el asistente, se crean las siguientes directivas:

- Una directiva de solicitud de conexión

- Una directiva de red

>[!NOTE]
>Puede ejecutar el Asistente para nuevas conexiones cableadas e inalámbricas seguras de IEEE 802.1 X cada vez que necesite crear nuevas directivas para el acceso autenticado a 802.1 X.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Crear directivas para la red inalámbrica autenticada mediante 802.1 X con un asistente

1. Abra el complemento\-NPS. Si aún no está seleccionada, haga clic **en \(NPS\)local**. Si está ejecutando el complemento\-MMC de NPS y desea crear directivas en un NPS remoto, seleccione el servidor.

2. En **Introducción**, en **Configuración estándar**, seleccione **servidor RADIUS para conexiones cableadas o inalámbricas 802.1 x**. El texto y los vínculos situados debajo del texto cambian para reflejar la selección.

3. Haga clic en **configurar 802.1 x**. Se abre el Asistente para configurar 802.1 X.

4.  En la página del asistente **Seleccionar tipo de conexión de 802.1 x** , en **tipo de conexiones de 802.1 x**, seleccione **conexiones inalámbricas seguras**y, en **nombre**, escriba un nombre para la Directiva o deje el nombre predeterminado **conexiones inalámbricas seguras** . . Haga clic en **Next**.

5.  En la página **especificar modificadores de 802.1 x** del asistente, en **clientes RADIUS**, se muestran todos los conmutadores 802.1 x y los puntos de acceso inalámbrico que ha\-agregado como clientes RADIUS en el complemento NPS. Lleve a cabo cualquiera de las siguientes acciones:

    -   Para \(agregar servidores de acceso a la\)red (NAS) adicionales, como AP inalámbricos, en **clientes RADIUS**, haga clic en **Agregar**y, a continuación, en **nuevo cliente RADIUS**, escriba la información de: **Nombre descriptivo**, **dirección \(IP o DNS\)de dirección**y **secreto compartido**.

    -   Para modificar la configuración de cualquier NAS, en **clientes RADIUS**, seleccione el AP para el que desea modificar la configuración y, a continuación, haga clic en **Editar**. Modifique la configuración según sea necesario.

    -   Para quitar un NAS de la lista, en **clientes RADIUS**, seleccione el NAS y, a continuación, haga clic en **quitar**.

        >[!WARNING]
        >Al quitar un cliente RADIUS en el Asistente para **configurar 802.1 x** , se elimina el cliente de la configuración de NPS. Todas las adiciones, modificaciones y eliminaciones que realice en el asistente **para configurar 802.1 x** en clientes RADIUS se reflejan en el\-complemento NPS, en el nodo **clientes RADIUS** en clientes \/ RADIUS NPS.  **Servidores de y**. Por ejemplo, si usa el Asistente para quitar un conmutador 802.1 x, el conmutador también se quita del complemento\-NPS.

6. Haga clic en **Next**. En la página del asistente **configurar un método de autenticación** , en  **\(tipo según el método de acceso y\)la configuración de red**, seleccione **Microsoft: EAP \(protegido PEAP\)y,a continuación, haga clic en **configurar.****

    >[!TIP]
    >Si recibe un mensaje de error que indica que no se puede encontrar un certificado para su uso con el método de autenticación y ha configurado Active Directory servicios de Certificate Server para emitir automáticamente certificados a servidores RAS e IAS de la red, primero Asegúrese de que ha seguido los pasos para registrar NPS en Active Directory Domain Services y, a continuación, siga estos pasos para actualizar directiva de grupo: Haga clic en **Inicio**, en **sistema de Windows**, en **Ejecutar**y, en **abrir**, escriba **gpupdate**y, a continuación, presione Entrar. Cuando el comando devuelve resultados que indican que el usuario y el equipo Directiva de grupo han actualizado correctamente **, seleccione Microsoft: Proteja EAP \(PEAP\) de** nuevo y, a continuación, haga clic en **configurar**.
    >
    >Si después de actualizar directiva de grupo sigue recibiendo el mensaje de error que indica que no se puede encontrar un certificado para su uso con el método de autenticación, el certificado no se muestra porque no cumple el certificado de servidor mínimo. requisitos que se documentan en la guía complementaria de red principal: [Implemente certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Si esto ocurre, debe interrumpir la configuración de NPS, revocar el certificado emitido para\(los\)NPS y, a continuación, seguir las instrucciones para configurar un certificado nuevo mediante la guía de implementación de certificados de servidor.

7.  En la página del asistente **Editar propiedades de EAP protegido** , en **certificado emitido**, asegúrese de que está seleccionado el certificado NPS correcto y, a continuación, haga lo siguiente:

    >[!NOTE]
    >Compruebe que el valor del **emisor** sea correcto para el certificado seleccionado en **certificado emitido**. Por ejemplo, el emisor esperado para un certificado emitido por una CA que ejecuta Active Directory servicios de \(Certificate\) Server AD CS denominado corp\DC1, en el dominio contoso.com, es **\-Corp DC1\-CA**.

    -   Para que los usuarios puedan desplazarse con sus equipos inalámbricos entre los puntos de acceso sin que sea necesario volver a autenticarse cada vez que se asocian con un nuevo AP, seleccione **Habilitar reconexión rápida**.

    -   Para especificar que los clientes inalámbricos que se conectan finalizarán el proceso de autenticación de red si el\-servidor RADIUS \(no\)presenta el valor TLV de longitud\-de tipo de cryptobinding, seleccione **desconectar clientes. sin cryptobinding**.  

    -   Para modificar la configuración de directiva para el tipo de EAP, en **tipos de EAP**, haga clic en **Editar**, en **propiedades de EAP MSCHAPv2**, modifique la configuración según sea necesario y, a continuación, haga clic en **Aceptar**.  

8.  Haga clic en **Aceptar**. El cuadro de diálogo Editar propiedades de EAP protegido se cierra y vuelve al Asistente para **configurar 802.1 x** . Haga clic en **Next**.

9. En **especificar grupos de usuarios**, haga clic en **Agregar**y, a continuación, escriba el nombre del grupo de seguridad que configuró para los clientes inalámbricos en\-el complemento Active Directory usuarios y equipos. Por ejemplo, si se denomina Grupo inalámbrico de grupo de seguridad inalámbrica, escriba **Grupo inalámbrico**. Haga clic en **Next**.

10. Haga clic en **configurar** para configurar los atributos estándar\-de RADIUS y los atributos \(específicos\) del proveedor para la VLAN de LAN virtual según sea necesario, y según se especifica en la documentación proporcionada por el proveedor de hardware de AP inalámbrico. Haga clic en **Next**.

11. Revise los detalles del Resumen de configuración y, a continuación, haga clic en **Finalizar**.

Ahora se crean las directivas de NPS y puede pasar a la Unión de equipos inalámbricos al dominio.

## <a name="bkmk_domain"></a>Unir nuevos equipos inalámbricos al dominio
El método más sencillo para unir nuevos equipos inalámbricos al dominio es conectar físicamente el equipo a un segmento de la LAN \(cableada un segmento no controlado por un conmutador\) de 802.1 x antes de unir el equipo al dominio. Esto es más sencillo porque la configuración de la Directiva de grupo inalámbrica se aplica de forma automática y inmediata y, si ha implementado su propia PKI, el equipo recibe el certificado de CA y lo coloca en el almacén de certificados de entidades de certificación raíz de confianza. permitir que el cliente inalámbrico confíe en NPSs con certificados de servidor emitidos por la CA.

Del mismo modo, después de que un nuevo equipo inalámbrico se une al dominio, el método preferido para que los usuarios inicien sesión en el dominio es realizar el inicio de sesión mediante una conexión cableada a la red.

### <a name="other-domain-join-methods"></a>Otros métodos de unión a un dominio
En los casos en los que no es práctico unir equipos al dominio mediante una conexión Ethernet cableada, o en los casos en los que el usuario no puede iniciar sesión en el dominio por primera vez mediante una conexión cableada, debe utilizar un método alternativo.

- **Configuración del equipo del personal de ti**. Un miembro del personal de ti une un equipo inalámbrico al dominio y configura un perfil inalámbrico de arranque de inicio de sesión único. Con este método, el administrador de ti conecta el equipo inalámbrico a la red Ethernet cableada y une el equipo al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario inicia el equipo sin utilizar una conexión cableada, las credenciales de dominio que especifican manualmente para el inicio de sesión de usuario se utilizan para establecer una conexión a la red inalámbrica e iniciar sesión en el dominio.

Para obtener más información, consulte la sección [Unión del dominio e inicio de sesión mediante el método de configuración del equipo del personal de ti](#bkmk_itstaff) .

-   **Configuración del perfil inalámbrico de bootstrap por parte de los usuarios**. El usuario configura manualmente el equipo inalámbrico con un perfil inalámbrico de arranque y se une al dominio, en función de las instrucciones adquiridas de un administrador de ti. El perfil inalámbrico de Bootstrap permite al usuario establecer una conexión inalámbrica y luego unirse al dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para obtener más información, consulte la sección [Unión del dominio e inicio de sesión mediante la configuración del perfil inalámbrico de bootstrap por parte de los usuarios](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Unirse al dominio e iniciar sesión mediante el método de configuración de equipo del personal de ti
Los usuarios miembros del dominio\-con equipos cliente inalámbricos Unidos a un dominio pueden usar un perfil inalámbrico temporal para\-conectarse a una red inalámbrica autenticada mediante 802.1 x sin necesidad de conectarse primero a la LAN cableada. Este perfil inalámbrico temporal se denomina *perfil inalámbrico de bootstrap*.

Un perfil inalámbrico de bootstrap requiere que el usuario especifique manualmente las credenciales de su cuenta de usuario de dominio y no valida el certificado del\-servidor RADIUS\) de \(servicio de usuario de acceso telefónico de autenticación remota. ejecutando NPS \(\)del servidor de directivas de redes.

Una vez establecida la conectividad inalámbrica, se aplica directiva de grupo en el equipo cliente inalámbrico y se emite automáticamente un nuevo perfil inalámbrico. La nueva directiva usa las credenciales de cuenta de usuario y equipo para la autenticación del cliente. 

Además, como parte de la autenticación\-mutua\-de PEAP MS CHAP V2 mediante el nuevo perfil en lugar del perfil de arranque, el cliente valida las credenciales del servidor RADIUS.

Después de unir el equipo al dominio, use este procedimiento para configurar un perfil inalámbrico de arranque de inicio de sesión único antes de distribuir el equipo inalámbrico al\-usuario miembro del dominio.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Para configurar un perfil inalámbrico de arranque de inicio de sesión único

1. Cree un perfil de arranque mediante el procedimiento de esta guía denominado [configurar un perfil de conexión inalámbrica para\-PEAP\-MS CHAP V2](#bkmk_configureprofile)y use la configuración siguiente:

    - Autenticación\-PEAP\-MS CHAP V2

    - Validar certificado de servidor RADIUS deshabilitado

    - Inicio de sesión único habilitado

2. En las propiedades de la Directiva de red inalámbrica en la que se creó el nuevo perfil de arranque, en la ficha **General** , seleccione el perfil de arranque y, a continuación, haga clic en **exportar** para exportar el perfil a un recurso compartido de red, una unidad flash USB u otro fácilmente. ubicación accesible. El perfil se guarda como un archivo *. XML en la ubicación que especifique.

3. Unir el nuevo equipo inalámbrico al dominio \(, por ejemplo, a través de una conexión Ethernet que no requiere la autenticación\) IEEE 802.1 x y agregar el perfil inalámbrico de bootstrap al equipo mediante el uso del **perfil netsh wlan Add** comando.

    >[!NOTE]
    >Para obtener más información, consulte comandos Netsh \(para red de área local inalámbrica WLAN\) en [http:\/\/\/technet.Microsoft.com\/Library dd744890. aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuya el nuevo equipo inalámbrico al usuario con el procedimiento "iniciar sesión en el dominio con equipos que ejecutan Windows 10".

Cuando el usuario inicia el equipo, Windows solicita al usuario que escriba su nombre de cuenta de usuario de dominio y contraseña. Dado que el inicio de sesión único está habilitado, el equipo usa las credenciales de la cuenta de usuario de dominio para establecer primero una conexión con la red inalámbrica y, a continuación, iniciar sesión en el dominio.

#### <a name="bkmk_w10"></a>Inicio de sesión en el dominio con equipos que ejecutan Windows 10

1. Cierre la sesión del equipo o reinicie el equipo.

2. Presione cualquier tecla del teclado o haga clic en el escritorio. Aparece la pantalla de inicio de sesión con un nombre de cuenta de usuario local y un campo de entrada de contraseña debajo del nombre. No inicie sesión con la cuenta de usuario local.

3. En la esquina inferior izquierda de la pantalla, haga clic en **otro usuario**. La pantalla de inicio de sesión de otro usuario aparece con dos campos, uno para nombre de usuario y otro para contraseña. Debajo del campo contraseña está el texto **Inicio de sesión en:** y, a continuación, el nombre del dominio al que está unido el equipo. Por ejemplo, si el dominio se denomina example.com, el texto lee **el inicio de sesión en: EJEMPLO**.

4. En **nombre de usuario**, escriba el nombre de usuario del dominio.

5. En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

>[!NOTE]
>Si la pantalla de **otro usuario** no incluye el **Inicio de sesión** de texto en: y el nombre de dominio, debe escribir el nombre de usuario en el formato *usuario del dominio\\* . Por ejemplo, para iniciar sesión en el dominio example.com con una cuenta denominada **usuario\-01**, escriba **example\\User\-01**.

### <a name="bkmk_userbootstrap"></a>Unir el dominio e iniciar sesión mediante la configuración de perfil inalámbrico de bootstrap por parte de los usuarios
Con este método, complete los pasos de la sección de pasos generales y, a continuación, proporcione\-a los usuarios miembros del dominio instrucciones sobre cómo configurar manualmente un equipo inalámbrico con un perfil inalámbrico de bootstrap. El perfil inalámbrico de Bootstrap permite al usuario establecer una conexión inalámbrica y luego unirse al dominio. Una vez que el equipo se ha unido al dominio y se ha reiniciado, el usuario puede iniciar sesión en el dominio a través de una conexión inalámbrica.

#### <a name="general-steps"></a>Pasos generales

1. Configure una cuenta de administrador de equipo local, en el **Panel de control**, para el usuario.

    >[!IMPORTANT]
    >Para unir un equipo a un dominio, el usuario debe haber iniciado sesión en el equipo con la cuenta de administrador local. Como alternativa, el usuario debe proporcionar las credenciales para la cuenta de administrador local durante el proceso de unión del equipo al dominio. Además, el usuario debe tener una cuenta de usuario en el dominio en el que el usuario desea unir el equipo. Durante el proceso de unión del equipo al dominio, se le pedirá al usuario el nombre de usuario y \(la contraseña\)de las credenciales de la cuenta de dominio.

2. Proporcione a los usuarios del dominio las instrucciones para configurar un perfil inalámbrico de bootstrap, como se documenta en el procedimiento siguiente **para configurar un perfil inalámbrico de bootstrap**.
3. Además, proporcione a los usuarios el nombre de usuario \(y la contraseña\)de las credenciales del \(equipo local, y el nombre\) y la contraseña de la cuenta de usuario de dominio de credenciales de dominio con el formato *nombreDeDominio\\El nombre de usuario*, así como los procedimientos para "unir el equipo al dominio" y "iniciar sesión en el dominio", tal como se documenta en la guía de [red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)de Windows Server 2016.

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Para configurar un perfil inalámbrico de bootstrap

1. Use las credenciales proporcionadas por el administrador de red o el profesional de soporte técnico de TI para iniciar sesión en el equipo con la cuenta de administrador del equipo local.

2. Haga\-clic con el botón secundario en el icono de red en el escritorio y haga clic en **abrir centro de redes y recursos compartidos**. Se abre **Centro de redes y recursos compartidos**. En **cambiar la configuración de red**, haga clic en **configurar una nueva conexión o red**. Se abre el cuadro de diálogo **configurar una conexión o red** .

3. Haga clic en **conectarse manualmente a una red inalámbrica**y, a continuación, haga clic en **siguiente**.

4. En **conectarse manualmente a una red inalámbrica**, en **nombre de red**, escriba el nombre de SSID del punto de conexión.  

5. En **tipo de seguridad**, seleccione el valor proporcionado por el administrador.

6. En **tipo de cifrado** y **clave de seguridad**, seleccione o escriba la configuración proporcionada por el administrador.

7. Seleccione **iniciar esta conexión automáticamente**y, a continuación, haga clic en **siguiente**.

8. En * se*agregó correctamente * * * el SSID de la red*, haga clic en **Cambiar configuración de conexión**.

9. Haga clic en **Cambiar configuración de conexión**. Se abre el cuadro de diálogo propiedad de red inalámbrica *de SSID de red* .

10. Haga clic en la pestaña **seguridad** y, a continuación, en **elegir un método de autenticación de red**, seleccione  **\(EAP protegido PEAP.\)**

11. Haga clic en **Configuración**. Se abre la página de **propiedades de \(EAP PEAP\) protegido** .

12. En la página de **propiedades\) de EAP \(protegido PEAP** , asegúrese de que la opción **validar certificado de servidor** no está seleccionada, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.

13. A continuación, Windows intenta conectarse a la red inalámbrica. La configuración del perfil inalámbrico de bootstrap especifica que debe proporcionar sus credenciales de dominio. Cuando Windows le solicite un nombre de cuenta y una contraseña, escriba las credenciales de la cuenta de dominio como se indica a continuación:  *Nombre de dominio nombre deusuario,contraseñadedominio.\\*

##### <a name="to-join-a-computer-to-the-domain"></a>Para unir un equipo al dominio

1. Inicie sesión en el equipo con la cuenta de administrador local.

2. En el cuadro de texto buscar, escriba **PowerShell**. En resultados de la búsqueda, haga clic con el botón derecho en **Windows PowerShell**y, a continuación, haga clic en **Ejecutar como administrador**. Windows PowerShell se abre con un símbolo del sistema con privilegios elevados.

3. En Windows PowerShell, escriba el siguiente comando y, a continuación, presione Entrar. Asegúrese de reemplazar la variable DomainName por el nombre del dominio al que desea unirse.
    
    Add-Computer DomainName
    
4. Cuando se le solicite, escriba su nombre de usuario y contraseña de dominio y haga clic en **Aceptar**.
5. Reinicie el equipo.
6. Siga las instrucciones de la sección anterior [inicie sesión en el dominio con equipos que ejecutan Windows 10](#bkmk_w10).