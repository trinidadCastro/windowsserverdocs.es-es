---
title: Planificación de la implementación de acceso inalámbrico
description: Este tema forma parte de la guía de redes de Windows Server 2016 "implementación de acceso inalámbrico autenticado mediante 802.1 X basado en contraseña".
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dc01e28429d4de7ff8f68a867d17963a08465adf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318078"
---
# <a name="wireless-access-deployment-planning"></a>Planificación de la implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de implementar el acceso inalámbrico, debe planear los siguientes elementos:

- Instalación de puntos de acceso inalámbricos \(APs\) en la red

- Acceso y configuración de cliente inalámbrico

En las secciones siguientes se proporcionan detalles sobre estos pasos de planeación.

## <a name="planning-wireless-ap-installations"></a>Planeación de instalaciones AP inalámbricas
Al diseñar la solución de acceso a redes inalámbricas, debe hacer lo siguiente:

1. Determinar qué estándares deben admitir los AP inalámbricos
2. Determinar las áreas de cobertura donde desea proporcionar el servicio inalámbrico
3. Determinar dónde desea ubicar los AP inalámbricos

Además, debe planear un esquema de direcciones IP para los clientes inalámbricos y de AP inalámbrico. Consulte la sección **planeación de la configuración de AP inalámbricos en NPS** a continuación para obtener información relacionada.

### <a name="verify-wireless-ap-support-for-standards"></a>Comprobar la compatibilidad de AP inalámbricos con los estándares
A efectos de coherencia y facilidad de implementación y administración de AP, se recomienda implementar AP inalámbricos de la misma marca y modelo.

Los AP inalámbricos que implemente deben admitir lo siguiente:

- **IEEE 802.1 X**

- **Autenticación RADIUS**

- **Autenticación y cifrado inalámbricos.** Enumerados por orden de mayor a menor preferencia:

    1.  WPA2\-Enterprise con AES

    2.  WPA2\-Enterprise con TKIP

    3.  WPA\-Enterprise con AES

    4.  WPA\-Enterprise con TKIP

>[!NOTE]
>Para implementar WPA2, debe usar adaptadores de red inalámbrica y AP inalámbricos que también admitan WPA2. De lo contrario, use WPA\-Enterprise.

Además, para proporcionar seguridad mejorada para la red, los AP inalámbricos deben admitir las siguientes opciones de seguridad:

- **Filtrado de DHCP.** El punto de conexión inalámbrico debe filtrar por los puertos IP para evitar la transmisión de mensajes de difusión DHCP en los casos en los que el cliente inalámbrico está configurado como un servidor DHCP. El punto de conexión inalámbrico debe impedir que el cliente envíe paquetes IP desde el puerto UDP 68 a la red.

- **Filtrado DNS.** El punto de conexión inalámbrico debe filtrar por los puertos IP para evitar que un cliente realice la ejecución como un servidor DNS. El punto de conexión inalámbrico debe impedir que el cliente envíe paquetes IP desde el puerto TCP o UDP 53 a la red.

- **Aislamiento de cliente** Si el punto de acceso inalámbrico proporciona funciones de aislamiento de cliente, debe habilitar la característica para evitar posibles ataques de protocolo de resolución de direcciones \(ARP\) la suplantación de identidad.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar áreas de cobertura para usuarios inalámbricos
Use los dibujos arquitectónicos de cada piso para cada edificio con el fin de identificar las áreas en las que desea proporcionar cobertura inalámbrica. Por ejemplo, identifique las oficinas adecuadas, las salas de conferencias, hoteles, cafeterías o Courtyards.

En los dibujos, indique los dispositivos que interfieren con las señales inalámbricas, como el equipo médico, las cámaras de vídeo inalámbricas, los teléfonos inalámbricos que operan en 2,4 a 2,5 GHz industrial, científico y médico \(el intervalo de\) ISM y los dispositivos habilitados para\-Bluetooth.

En el dibujo, marque aspectos del edificio que podrían interferir con las señales inalámbricas; los objetos de metal utilizados en la construcción de un edificio pueden afectar a la señal inalámbrica. Por ejemplo, los siguientes objetos comunes pueden interferir con la propagación de señales: ascensores, calefacción y aire\-conductos de acondicionamiento y soporte concreto Girders.

Consulte al fabricante de AP para obtener información acerca de los orígenes que podrían provocar la atenuación de la frecuencia de radio de AP inalámbrico. La mayoría de los APs proporcionan software de prueba que puede usar para comprobar la intensidad de la señal, la tasa de errores y el rendimiento de los datos.

### <a name="determine-where-to-install-wireless-aps"></a>Determinar dónde instalar AP inalámbricos
En los dibujos arquitectónicos, busque los AP inalámbricos lo suficientemente juntos para proporcionar una cobertura inalámbrica amplia, pero lo suficientemente alejado que no interfieren entre sí.

La distancia necesaria entre los puntos de conexión depende del tipo de punto de conexión de AP y AP, aspectos del edificio que bloquean las señales inalámbricas y otros orígenes de interferencias. Puede marcar ubicaciones de AP inalámbricos para que cada punto de conexión inalámbrico no tenga más de 300 pies desde cualquier AP inalámbrico adyacente. Consulte la documentación del fabricante de AP inalámbrico para conocer las especificaciones de AP y las directrices para la selección de ubicación.

Instale temporalmente AP inalámbricos en las ubicaciones especificadas en los dibujos arquitectónicos. A continuación, con un portátil equipado con un adaptador inalámbrico 802,11 y el software de encuesta de sitios que se suministra normalmente con los adaptadores inalámbricos, determine la intensidad de la señal dentro de cada área de cobertura.

En las áreas de cobertura en las que la intensidad de señal es baja, coloque el punto de conexión para mejorar la intensidad de la señal en el área de cobertura, instale AP inalámbricos adicionales para proporcionar la cobertura necesaria, reubique o quite orígenes de interferencias de señales.  

Actualice los dibujos arquitectónicos para indicar la ubicación final de todos los AP inalámbricos. Tener un mapa de ubicación de AP preciso le ayudará más adelante durante las operaciones de solución de problemas o cuando desee actualizar o reemplazar APs.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planear la configuración de cliente RADIUS de AP inalámbrico y NPS
Puede usar NPS para configurar AP inalámbricos individualmente o en grupos.

Si está implementando una red inalámbrica grande que incluye muchos puntos de conexión, es mucho más fácil configurar APs en grupos. Para agregar APs como grupos de clientes RADIUS en NPS, debe configurar APs con estas propiedades.

- Los AP inalámbricos se configuran con direcciones IP del mismo intervalo de direcciones IP.

- Todos los puntos de conexión inalámbricos están configurados con el mismo secreto compartido.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planear el uso de la reconexión rápida de PEAP
En una infraestructura de 802.1 X, los puntos de acceso inalámbrico se configuran como clientes RADIUS en los servidores RADIUS. Cuando se implementa la reconexión rápida de PEAP, no es necesario que un cliente inalámbrico que se desplace entre dos o más puntos de acceso se autentique con cada nueva asociación.

La reconexión rápida de PEAP reduce el tiempo de respuesta para la autenticación entre el cliente y el autenticador porque la solicitud de autenticación se reenvía desde el nuevo punto de acceso al NPS que originalmente realizó la autenticación y autorización para el cliente. solicitud de conexión.

Dado que tanto el cliente PEAP como NPS usan la seguridad de la capa de transporte previamente almacenada en la memoria caché \(TLS\) propiedades de conexión \(la colección de la que se denomina el identificador de TLS\), el NPS puede determinar rápidamente que el cliente está autorizado para una reconexión.

>[!IMPORTANT]
>Para que la reconexión rápida funcione correctamente, el APs debe estar configurado como clientes RADIUS del mismo NPS.

Si el NPS original deja de estar disponible, o si el cliente se mueve a un punto de acceso que está configurado como cliente RADIUS en otro servidor RADIUS, se debe realizar la autenticación completa entre el cliente y el nuevo autenticador.

### <a name="wireless-ap-configuration"></a>Configuración de AP inalámbrico
En la lista siguiente se resumen los elementos que se configuran normalmente en 802.1 X\-los AP inalámbricos compatibles:

> [!NOTE]
> Los nombres de elemento pueden variar según la marca y el modelo, y pueden ser diferentes de los de la lista siguiente. Consulte la documentación de AP inalámbricos para obtener información detallada sobre la configuración\-.

- **Identificador del conjunto de servicios \(SSID\)** . Este es el nombre de la red inalámbrica \(por ejemplo, ExampleWlan\)y el nombre que se anuncia a los clientes inalámbricos. Para reducir la confusión, el SSID que decida anunciar no debe coincidir con el SSID que emiten las redes inalámbricas que se encuentran dentro del intervalo de recepción de la red inalámbrica.

    En los casos en que se implementan varios AP inalámbricos como parte de la misma red inalámbrica, configure cada AP inalámbrico con el mismo SSID. En los casos en que se implementan varios AP inalámbricos como parte de la misma red inalámbrica, configure cada AP inalámbrico con el mismo SSID.  

    En los casos en los que tenga la necesidad de implementar diferentes redes inalámbricas para satisfacer las necesidades empresariales específicas, el punto de conexión de AP inalámbrico en una red debe difundir un SSID diferente que el SSID que el otro\(de red\). Por ejemplo, si necesita una red inalámbrica independiente para los empleados y los invitados, puede configurar los AP inalámbricos para la red de empresa con el SSID establecido en difundir **ExampleWLAN**. En el caso de la red de invitado, puede establecer el SSID de cada AP inalámbrico para difundir **GuestWLAN**. De este modo, los empleados y los invitados pueden conectarse a la red deseada sin confusiones innecesarias.  

    > [!TIP]  
    > Algunos AP inalámbricos tienen la capacidad de difundir varios SSID para acomodar implementaciones de red de varios\-. Los AP inalámbricos que pueden difundir varios SSID pueden reducir los costos de implementación y mantenimiento operativo.  

- **Autenticación y cifrado inalámbricos**.

    La autenticación inalámbrica es la autenticación de seguridad que se usa cuando el cliente inalámbrico se asocia con un punto de acceso inalámbrico.  

    El cifrado inalámbrico es el cifrado de cifrado de seguridad que se usa con la autenticación inalámbrica para proteger las comunicaciones que se envían entre el punto de conexión inalámbrico y el cliente inalámbrico.  

- **Dirección IP de AP inalámbrico \(\)estática** . En cada punto de conexión inalámbrico, configure una dirección IP estática única. Si un servidor DHCP presta servicio a la subred, asegúrese de que todas las direcciones IP de AP estén dentro de un intervalo de exclusión de DHCP para que el servidor DHCP no intente emitir la misma dirección IP en otro equipo o dispositivo. Los intervalos de exclusión se documentan en el procedimiento "para crear y activar un nuevo ámbito DHCP" en la [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si tiene previsto configurar APs como clientes RADIUS por grupo en NPS, cada punto de configuración del grupo debe tener una dirección IP del mismo intervalo de direcciones IP.

- **Nombre DNS**. Algunos AP inalámbricos se pueden configurar con un nombre DNS. Configure cada AP inalámbrico con un nombre único. Por ejemplo, si tiene un AP inalámbrico implementado en un proceso de creación de casos de varios\-, puede asignar un nombre a los tres primeros puntos de conexión inalámbricos que se implementan en el tercer piso AP3\-01, AP3\-02 y AP3\-03.

- **Máscara de subred de AP inalámbrico**. Configure la máscara para designar qué parte de la dirección IP es el identificador de red y qué parte de la dirección IP es el host.

- **Servicio DHCP de AP**. Si el punto de conexión inalámbrico tiene un\-integrado en el servicio DHCP, deshabilítelo.

- **Secreto compartido de RADIUS**. Use un secreto compartido de RADIUS único para cada punto de conexión inalámbrico a menos que vaya a configurar clientes RADIUS NPS en grupos, en los que debe configurar todos los puntos de conexión del grupo con el mismo secreto compartido. Los secretos compartidos deben ser una secuencia aleatoria de al menos 22 caracteres, con letras mayúsculas y minúsculas, números y signos de puntuación. Para garantizar la aleatoriedad, puede usar un programa de generación de caracteres aleatorios para crear los secretos compartidos. Se recomienda grabar el secreto compartido para cada punto de conexión inalámbrico y almacenarlo en una ubicación segura, como una oficina segura. Al configurar clientes RADIUS en la consola de NPS, creará una versión virtual de cada AP. El secreto compartido que configure en cada AP virtual en NPS debe coincidir con el secreto compartido en el punto de configuración físico real.

- **Dirección IP del servidor RADIUS**. Escriba la dirección IP del NPS que desea usar para autenticar y autorizar las solicitudes de conexión a este punto de acceso.

- **Puerto UDP\(s\)** . De forma predeterminada, NPS usa los puertos UDP 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos UDP 1813 y 1646 para los mensajes de cuentas RADIUS. Se recomienda que no cambie la configuración predeterminada de los puertos UDP de RADIUS.

- **VSA**. Algunos AP inalámbricos requieren atributos específicos del proveedor\-\(los VSA\) para proporcionar una funcionalidad de AP inalámbrico completa.

- **Filtrado de DHCP**. Configure los AP inalámbricos para impedir que los clientes inalámbricos envíen paquetes IP desde el puerto UDP 68 a la red. Consulte la documentación de su AP inalámbrico para configurar el filtrado de DHCP.

- **Filtrado DNS**. Configure los AP inalámbricos para impedir que los clientes inalámbricos envíen paquetes IP desde el puerto TCP o UDP 53 a la red. Consulte la documentación de su AP inalámbrico para configurar el filtrado de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planear el acceso y la configuración de clientes inalámbricos

Al planear la implementación de 802.1 X\-el acceso inalámbrico autenticado, debe tener en cuenta varios factores específicos de\-cliente:

- **Planeación de la compatibilidad con varios estándares**.

    Determine si todos los equipos inalámbricos usan la misma versión de Windows o si son una combinación de equipos que ejecutan sistemas operativos diferentes. Si son diferentes, asegúrese de que comprende cualquier diferencia en los estándares admitidos por los sistemas operativos.

    Determine si todos los adaptadores de red inalámbrica de todos los equipos cliente inalámbricos admiten los mismos estándares inalámbricos o si necesita admitir distintos estándares. Por ejemplo, determine si algunos controladores de hardware de adaptador de red admiten WPA2\-Enterprise y AES, mientras que otros solo admiten WPA\-Enterprise y TKIP.

- **Planeamiento del modo de autenticación del cliente**. Los modos de autenticación definen cómo los clientes de Windows procesan las credenciales de dominio. Puede seleccionar entre los siguientes tres modos de autenticación de red en las directivas de red inalámbrica.  

    1. El **usuario\-la autenticación**. Este modo especifica que la autenticación siempre se realiza mediante el uso de credenciales de seguridad basadas en el estado actual del equipo. Cuando ningún usuario ha iniciado sesión en el equipo, la autenticación se realiza mediante las credenciales del equipo. Cuando un usuario inicia sesión en el equipo, la autenticación siempre se realiza mediante las credenciales de usuario.  

    2. **Solo equipo**. Modo de solo equipo especifica que la autenticación siempre se realiza utilizando las credenciales del equipo.  

    3.  **Autenticación de usuario**. El modo de autenticación de usuario especifica que la autenticación solo se realiza cuando el usuario inicia sesión en el equipo. Cuando no hay usuarios con sesión iniciada en el equipo, no se realizan intentos de autenticación.  

- **Planeación de restricciones inalámbricas**. Determine si desea proporcionar a todos los usuarios inalámbricos el mismo nivel de acceso a la red inalámbrica o si desea restringir el acceso a algunos de los usuarios inalámbricos. Puede aplicar restricciones en NPS contra grupos específicos de usuarios inalámbricos.  Por ejemplo, puede definir días y horas específicos a los que determinados grupos tienen permiso de acceso a la red inalámbrica.  

- **Métodos de planeación para agregar nuevos equipos inalámbricos**. En el caso de los equipos con capacidad de\-inalámbrica que están Unidos al dominio antes de implementar la red inalámbrica, si el equipo está conectado a un segmento de la red cableada que no está protegida por 802.1 X, las opciones de configuración inalámbrica se aplican automáticamente después de configurar las directivas de red inalámbrica \(IEEE 802,11\) en el controlador de dominio y después de que se actualice directiva de grupo en el cliente  

    Sin embargo, para los equipos que aún no están Unidos a su dominio, debe planear un método para aplicar la configuración necesaria para el acceso autenticado de 802.1 X\-. Por ejemplo, determine si desea unir el equipo al dominio utilizando uno de los métodos siguientes.

    1.  Conecte el equipo a un segmento de la red cableada que no esté protegida por 802.1 X y, a continuación, una el equipo al dominio.

    2.  Proporcione a los usuarios inalámbricos los pasos y la configuración que necesitan para agregar su propio perfil de arranque inalámbrico, lo que les permite unir el equipo al dominio.

    3.  Asigne al personal de ti que se una a los clientes inalámbricos al dominio.

### <a name="planning-support-for-multiple-standards"></a>Planeación de la compatibilidad con varios estándares

La red inalámbrica \(extensión de directivas de\) IEEE 802,11 en directiva de grupo proporciona una amplia gama de opciones de configuración para admitir diversas opciones de implementación.

Puede implementar AP inalámbricos configurados con los estándares que desea admitir y, a continuación, configurar varios perfiles inalámbricos en las directivas de red inalámbrica \(IEEE 802,11\), donde cada perfil especifica un conjunto de estándares que requiere.

Por ejemplo, si la red tiene equipos inalámbricos que admiten WPA2\-Enterprise y AES, otros equipos que admiten WPA\-Enterprise y AES, y otros equipos que solo admiten WPA\-Enterprise y TKIP, debe determinar si desea:

- Configure un único perfil para admitir todos los equipos inalámbricos mediante el método de cifrado más débil que admitan todos los equipos; en este caso, WPA\-Enterprise y TKIP.  
- Configure dos perfiles para proporcionar la mejor seguridad posible que sea compatible con cada equipo inalámbrico. En esta instancia, configuraría un perfil que especifica el cifrado más fuerte \(WPA2\-Enterprise y AES\)y otro perfil que usa el cifrado WPA\-Enterprise y TKIP más débil. En este ejemplo, es esencial que coloque el perfil que usa WPA2\-Enterprise y AES más alto en el orden de preferencia. Los equipos que no pueden usar WPA2\-Enterprise y AES pasarán automáticamente al siguiente perfil en el orden de preferencia y procesarán el perfil que especifica WPA\-Enterprise y TKIP.

> [!IMPORTANT]
> Debe colocar el perfil con los estándares más seguros más arriba en la lista ordenada de perfiles, ya que los equipos que se conectan usan el primer perfil que pueden usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planeación del acceso restringido a la red inalámbrica

En muchos casos, es posible que desee proporcionar a los usuarios inalámbricos distintos niveles de acceso a la red inalámbrica. Por ejemplo, puede que desee permitir que algunos usuarios tengan acceso sin restricciones, cada hora del día, cada día de la semana. Para otros usuarios, es posible que solo desee permitir el acceso durante las horas principales, de lunes a viernes, y denegar el acceso en sábado y domingo.

En esta guía se proporcionan instrucciones para crear un entorno de acceso que coloca a todos los usuarios inalámbricos en un grupo con acceso común a los recursos inalámbricos. Cree un grupo de seguridad de usuarios inalámbricos en el\-de Active Directory usuarios y equipos en y, a continuación, haga que todos los usuarios a los que desea conceder acceso inalámbrico pertenezcan a un miembro de ese grupo.

Al configurar las directivas de red de NPS, se especifica el grupo de seguridad usuarios inalámbricos como el objeto que NPS procesa al determinar la autorización.

Sin embargo, si la implementación requiere compatibilidad con distintos niveles de acceso solo necesitará lo siguiente:  

1. Cree más de un grupo de seguridad de usuarios inalámbricos para crear grupos de seguridad inalámbrica adicionales en Active Directory usuarios y equipos. Por ejemplo, puede crear un grupo que contenga usuarios que tengan acceso completo, un grupo para los usuarios que solo tengan acceso durante las horas de trabajo normales y otros grupos que se ajusten a otros criterios que satisfagan sus necesidades.

2. Agregue usuarios a los grupos de seguridad adecuados que creó.

3. Configure directivas de red NPS adicionales para cada grupo de seguridad inalámbrica adicional y configure las directivas para aplicar las condiciones y restricciones que necesita para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Métodos de planeación para agregar nuevos equipos inalámbricos

El método preferido para unir nuevos equipos inalámbricos al dominio y, a continuación, iniciar sesión en el dominio es mediante una conexión cableada a un segmento de la LAN que tiene acceso a los controladores de dominio y no está protegido por un conmutador Ethernet de autenticación de 802.1 X.

En algunos casos, sin embargo, puede que no sea práctico usar una conexión cableada para unir equipos al dominio o, para que un usuario Use una conexión cableada para su primer inicio de sesión mediante equipos que ya están Unidos al dominio.

Para unir un equipo al dominio mediante una conexión inalámbrica o para que los usuarios inicien sesión en el dominio la primera vez mediante un equipo unido al dominio\-y una conexión inalámbrica, los clientes inalámbricos deben establecer primero una conexión a la red inalámbrica en un segmento que tenga acceso a los controladores de dominio de la red mediante uno de los métodos siguientes.

1. **Un miembro del personal de ti une un equipo inalámbrico al dominio y, a continuación, configura un perfil inalámbrico de arranque de inicio de sesión único.** Con este método, un administrador de ti conecta el equipo inalámbrico a la red Ethernet cableada y, a continuación, une el equipo al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario inicia el equipo, las credenciales de dominio que especifican manualmente para el proceso de inicio de sesión de usuario se utilizan para establecer una conexión a la red inalámbrica e iniciar sesión en el dominio.  

2. **El usuario configura manualmente el equipo inalámbrico con el perfil inalámbrico de arranque y, a continuación, se une al dominio.** Con este método, los usuarios configuran manualmente sus equipos inalámbricos con un perfil inalámbrico de bootstrap basado en las instrucciones de un administrador de ti. El perfil inalámbrico de Bootstrap permite a los usuarios establecer una conexión inalámbrica y, a continuación, unir el equipo al dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para implementar el acceso inalámbrico, consulte [implementación de acceso inalámbrico](e-wireless-access-deployment.md).
