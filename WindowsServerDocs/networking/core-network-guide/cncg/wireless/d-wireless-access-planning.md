---
title: Planificación de la implementación de acceso inalámbrico
description: Este tema forma parte de la Guía de redes de Windows Server 2016 "Implementar mediante 802.1X basado en contraseña X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2571f509fbbca8384e626ad3c8c13f1c0a50400
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855466"
---
# <a name="wireless-access-deployment-planning"></a>Planificación de la implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de implementar acceso inalámbrico, debe planear los siguientes elementos:

- Instalación de puntos de acceso inalámbrico \(APs\) en la red

- Configuración del cliente inalámbrico y acceso

Las secciones siguientes proporcionan detalles sobre estos pasos de planeación.

## <a name="planning-wireless-ap-installations"></a>Planear instalaciones de AP inalámbricas
Al diseñar la solución de acceso de red inalámbrica, debe hacer lo siguiente:

1. Determinar qué estándares debe ser compatible con tus AP inalámbricos
2. Determinar las áreas de cobertura donde desea proporcionar servicios inalámbricos
3. Determine dónde desea ubicar AP inalámbricos

Además, debe planear un esquema de direcciones IP de tu AP inalámbrico y los clientes inalámbricos. Consulte la sección **planificar la configuración de AP inalámbrico en NPS** a continuación para obtener información relacionada.

### <a name="verify-wireless-ap-support-for-standards"></a>Comprobar la compatibilidad de AP inalámbrico para estándares
Para los fines de coherencia y la facilidad de implementación y administración de AP, se recomienda implementar AP inalámbricos de la misma marca y modelo.

Los AP inalámbricos que implemente deben admitir lo siguiente:

- **IEEE 802.1X**

- **Autenticación RADIUS**

- **La autenticación inalámbrica y cifrado.** Se muestran en orden de mayor a menor preferencia:

    1.  WPA2\-Enterprise con AES

    2.  WPA2\-Enterprise con TKIP

    3.  WPA\-Enterprise con AES

    4.  WPA\-Enterprise con TKIP

>[!NOTE]
>Para implementar WPA2, debe usar adaptadores de red inalámbrica y AP inalámbricos compatibles con WPA2. En caso contrario, usar WPA\-Enterprise.

Además, para proporcionar mayor seguridad para la red, la AP inalámbricos deben admitir las siguientes opciones de seguridad:

- **Filtros DHCP.** El AP inalámbrico debe filtrar en puertos IP para evitar la transmisión de mensajes de difusión DHCP en esos casos en que el cliente inalámbrico está configurado como un servidor DHCP. El AP inalámbrico debe bloquear al cliente en el envío de paquetes IP del puerto UDP 68 a la red.

- **Filtros DNS.** El AP inalámbrico debe filtrar en puertos IP para evitar que a un cliente actúe como un servidor DNS. El AP inalámbrico debe bloquear al cliente en el envío de paquetes IP de TCP o UDP puerto 53 a la red.

- **Aislamiento de clientes** si su punto de acceso inalámbrico proporciona capacidades de aislamiento de cliente, debe habilitar la característica evitar posibles direcciones de protocolo de resolución \(ARP\) ataques de suplantación de identidad.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar las áreas de cobertura para usuarios inalámbricos
Utilice los planos de cada planta para cada edificio para identificar las áreas donde desee proporcionar cobertura inalámbrica. Por ejemplo, identificar las oficinas adecuadas, salas de conferencias, vestíbulos, cafeterías o jardines.

En los dibujos, indica a los dispositivos que interfieran con las señales inalámbricas, como equipos médicos, cámaras de vídeo inalámbricas, teléfonos inalámbricos que operan en el 2.4 a 2,5 GHz Industrial, científico y médico \(ISM\) intervalo y Bluetooth\-dispositivos habilitados.

En el dibujo, marcar los aspectos de la compilación que podrían interferir con las señales inalámbricas; objetos metálicos usados en la construcción de un edificio pueden afectar a la señal inalámbrica. Por ejemplo, los siguientes objetos comunes pueden interferir con la propagación de señal: Ascensores, calefacción y aire\-acondicionamiento conductos y vigas de soporte técnico concreto.

Consulte el fabricante del punto de acceso para obtener información acerca de los orígenes que pueden provocar la atenuación de radiofrecuencia AP inalámbrico. La mayoría de APs proporcionan software de prueba que puede usar para comprobar la intensidad de señal, tasa de errores y rendimiento de los datos.

### <a name="determine-where-to-install-wireless-aps"></a>Determinar dónde instalar AP inalámbricos
En los planos, busque tus AP inalámbricos cierre suficientemente juntos para proporcionar una distancia suficiente cobertura inalámbrica, pero lo que sea necesario que no interfieren entre sí.

La distancia entre puntos de acceso necesaria depende del tipo de punto de acceso y antena de Asia Pacífico, aspectos de la compilación que bloquean inalámbricas señales y otras fuentes de interferencias. Puede marcar las selecciones de ubicación de AP inalámbricos para que cada AP inalámbrico no es de más de 300 pies desde cualquier punto de acceso inalámbrico adyacente. Consulte la documentación del fabricante de AP inalámbricos para las especificaciones de Asia Pacífico y directrices para la selección de ubicación.

Instalar temporalmente AP inalámbricos en las ubicaciones especificadas en los planos de arquitectura. A continuación, usa un equipo portátil equipado con un adaptador inalámbrico 802.11 y software del sitio de encuesta que habitualmente se suministra con los adaptadores inalámbricos, determine la intensidad de señal dentro de cada área de cobertura.

En las áreas de cobertura que la intensidad de la señal es baja, coloque el punto de acceso para mejorar la intensidad de señal para el área de cobertura, instale AP inalámbricos adicionales para proporcionar la cobertura es necesaria, reubicar o eliminar las fuentes de interferencias de señales.  

Actualice los planos de arquitectura para indicar la posición final de todos los puntos de acceso inalámbricos. Tener una asignación de selección de ubicación de AP precisa ayudará posteriormente durante la solución de problemas de operaciones o cuando desea actualizar o reemplazar los puntos de acceso.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planear la configuración de cliente de RADIUS NPS y AP inalámbrica
Puede usar NPS para configurar puntos de acceso inalámbricos individualmente o en grupos.

Si va a implementar una red inalámbrica de gran tamaño que incluye muchos puntos de acceso, es mucho más fácil configurar puntos de acceso en grupos. Para agregar los puntos de acceso como grupos de clientes RADIUS en NPS, debe configurar los puntos de acceso con estas propiedades.

- Los puntos de acceso inalámbricos están configurados con direcciones IP desde el mismo intervalo de direcciones IP.

- Los AP inalámbricos se configuran con el mismo secreto compartido.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planear el uso de la reconexión rápida PEAP
En una infraestructura de 802.1X, los puntos de acceso inalámbrico están configurados como clientes RADIUS a servidores RADIUS. Se implementa cuando reconexión rápida de PEAP, un cliente inalámbrico que se desplaza entre dos o más puntos de acceso no es necesario para autenticarse con cada nueva asociación.

Reconexión rápida de PEAP reduce el tiempo de respuesta para la autenticación entre cliente y autenticador porque la solicitud de autenticación se reenvía desde el nuevo punto de acceso a NPS que originalmente realizó la autenticación y autorización para el cliente solicitud de conexión.

Dado que tanto el cliente PEAP y NPS usan previamente en caché Transport Layer Security \(TLS\) las propiedades de conexión \(la recopilación de los cuales se denomina identificador de TLS\), NPS puede determinar rápidamente que el cliente está autorizado para una reconexión.

>[!IMPORTANT]
>Para obtener rápida volver a conectarse para poder funcionar correctamente, los puntos de acceso deben estar configurados como clientes RADIUS de la mismo NPS.

Si el NPS original no está disponible, o si el cliente se mueve a un punto de acceso que está configurado como un cliente RADIUS a otro servidor RADIUS, debe producirse la autenticación completa entre el cliente y el nuevo autenticador.

### <a name="wireless-ap-configuration"></a>Configuración de AP inalámbricos
En la lista siguiente resume los elementos que se define comúnmente en 802.1X\-AP inalámbricos compatibles con:

> [!NOTE]
> Los nombres de elementos pueden variar según la marca y el modelo y pueden diferir de las de la lista siguiente. Consulte la documentación de AP inalámbrica para la configuración\-detalles específicos.

- **Identificador de red \(SSID\)**. Éste es el nombre de la red inalámbrica \(por ejemplo, ExampleWlan\)y el nombre que se anuncia a los clientes inalámbricos. Para reducir la confusión, el SSID que elija para anunciar no debería coincidir con el SSID que se difunde por las redes inalámbricas que están dentro del alcance de su red inalámbrica.

    En los casos en que se implementan varios puntos de acceso inalámbricos como parte de la misma red inalámbrica, configure cada AP inalámbrico con el mismo SSID. En los casos en que se implementan varios puntos de acceso inalámbricos como parte de la misma red inalámbrica, configure cada AP inalámbrico con el mismo SSID.  

    En casos donde haya una necesidad de implementar distintas redes inalámbricas para satisfacer las necesidades empresariales específicas, su en una red la AP inalámbrico debe difundir un SSID diferente que el SSID sus otras redes\(s\). Por ejemplo, si tiene una red inalámbrica independiente para los empleados y los invitados, puede configurar tus AP inalámbricos para red de la empresa con el SSID establecido para difundir **ExampleWLAN**. Para la red de invitado, entonces podría establecer SSID de cada AP inalámbrico para difundir **GuestWLAN**. De esta manera los empleados y los invitados pueden conectarse a la red deseada sin confusión innecesario.  

    > [!TIP]  
    > Algunos AP inalámbrico tiene la posibilidad de difundir varios SSID para dar cabida a varios\-las implementaciones de red. AP inalámbrico que se puede difundir de varios SSID puede reducir costos de implementación y mantenimiento.  

- **Autenticación y cifrado inalámbrico**.

    Autenticación inalámbrica es la autenticación de seguridad que se usa cuando el cliente inalámbrico lo asocia con un punto de acceso inalámbrico.  

    Cifrado inalámbrico es el cifrado de seguridad que se usa con la autenticación inalámbrica para proteger las comunicaciones que se envían entre el cliente inalámbrico y el AP inalámbrico.  

- **Inalámbrica de la dirección IP PA \(estático\)**. En cada AP inalámbrico, configure una única dirección IP estática. Si la subred es atendida por un servidor DHCP, asegúrese de que todas las direcciones IP PA se encuentran dentro de un intervalo de exclusión de DHCP para que el servidor DHCP no vuelva a emitir la misma dirección IP a otro equipo o dispositivo. Los intervalos de exclusión se documentan en el procedimiento "para crear y activar un nuevo ámbito DHCP" en el [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si va a configurar puntos de acceso como clientes RADIUS por grupo en NPS, cada punto de acceso en el grupo debe tener una dirección IP desde el mismo intervalo de direcciones IP.

- **Nombre DNS**. Algunos puntos de acceso inalámbricos pueden configurarse con un nombre DNS. Configure cada AP inalámbrico con un nombre único. Por ejemplo, si tiene un AP inalámbricos implementados en un multi\-historia edificio, podría denominar los tres primeros AP inalámbricos que se implementan en el tercer piso AP3\-01, AP3\-02 y AP3\-03.

- **Máscara de subred de AP inalámbrica**. Configurar la máscara para designar qué parte de la dirección IP dirección es el identificador de red y qué parte de la dirección IP es el host.

- **Servicio DHCP AP**. Si tu AP inalámbrico tiene integradas\-en el servicio DHCP, deshabilitarla.

- **Secreto compartido**. Usar un radio único compartido secreto cada AP inalámbrico, a menos que se va a configurar a clientes RADIUS NPS en grupos - en qué circunstancias deben configurar todos los puntos de acceso en el grupo con el mismo secreto compartido. Secretos compartidos deben ser una secuencia aleatoria de al menos de 22 caracteres, con las mayúsculas y minúsculas, números y signos de puntuación. Para asegurarse de aleatoriedad, puede usar un programa de generación de caracteres aleatorios para crear los secretos compartidos. Se recomienda que registre el secreto compartido para cada AP inalámbrico y almacenarlo en una ubicación segura, como una oficina segura. Al configurar clientes RADIUS en la consola de NPS creará una versión virtual de cada punto de acceso. El secreto compartido que se configura en cada punto de acceso virtual en NPS debe coincidir con el secreto compartido en el punto de acceso real, físico.

- **Dirección IP del servidor RADIUS**. Escriba la dirección IP de NPS que desea usar para autenticar y autorizar las solicitudes de conexión a este punto de acceso.

- **El puerto UDP\(s\)**. De forma predeterminada, NPS usa los puertos UDP 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos UDP 1813 y 1646 para los mensajes de cuentas RADIUS. Se recomienda que no cambie la configuración de puertos UDP de RADIUS de forma predeterminada.

- **Los VSA**. Algunos puntos de acceso inalámbricos requieren proveedor\-atributos específicos \(VSA\) para proporcionar funcionalidad de AP inalámbrica completa.

- **Filtros DHCP**. Configurar puntos de acceso inalámbricos para bloquear a clientes inalámbricos en el envío de paquetes IP del puerto UDP 68 a la red. Consulte la documentación de tu AP inalámbrico configurar el filtrado de DHCP.

- **Filtrado de DNS**. Configurar puntos de acceso inalámbricos para bloquear a clientes inalámbricos en el envío de paquetes IP del puerto TCP o UDP 53 a la red. Consulte la documentación de tu AP inalámbrico configurar el filtrado de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planeación de acceso y la configuración de cliente inalámbrico

Al planear la implementación de 802.1X\-autentica el acceso inalámbrico, debe tener en cuenta varios cliente\-determinados factores:

- **Planear la compatibilidad con varios estándares**.

    Determinar si los equipos inalámbricos todos usan la misma versión de Windows o si son una combinación de los equipos que ejecutan sistemas operativos diferentes. Si son diferentes, asegúrese de que comprende las diferencias en los estándares compatibles con los sistemas operativos.

    Determine si todos los adaptadores de red inalámbrica en todos los equipos cliente inalámbricos compatibles con los mismos estándares inalámbricos, o si necesita admitir diversos estándares. Por ejemplo, determinar si algunos controladores de hardware del adaptador de red compatibles con WPA2\-Enterprise y AES, mientras que otros admiten solo WPA\-Enterprise y TKIP.

- **Modo de autenticación de cliente planeamiento**. Modos de autenticación definen cómo los clientes de Windows procesan las credenciales de dominio. Puede seleccionar desde los siguientes modos de autenticación de tres red en las directivas de red inalámbrica.  

    1. **Usuario re\-autenticación**. Este modo, se especifica que la autenticación siempre se realiza mediante las credenciales de seguridad según el estado actual del equipo. Cuando no los usuarios inician sesión el equipo, la autenticación se realiza utilizando las credenciales del equipo. Cuando un usuario haya iniciado sesión en el equipo, la autenticación se realiza siempre con las credenciales de usuario.  

    2. **Solo equipo**. Equipo solo modo especifica que la autenticación se realiza siempre utilizando sólo las credenciales del equipo.  

    3.  **Autenticación de usuario**. Modo de autenticación de usuario especifica que la autenticación se realiza solo cuando el usuario haya iniciado sesión en el equipo. Cuando no hay ningún usuario ha iniciado sesión el equipo, no se realizan los intentos de autenticación.  

- **Planeación restricciones inalámbricas**. Determine si desea proporcionar a todos los usuarios inalámbricos con el mismo nivel de acceso a la red inalámbrica, o si desea restringir el acceso para algunos de los usuarios inalámbricos. Puede aplicar las restricciones en NPS en grupos específicos de usuarios de telefonía móvil.  Por ejemplo, puede definir específico días y horas que ciertos grupos tienen permiso de acceso a la red inalámbrica.  

- **Planeación de métodos para agregar nuevos equipos inalámbricos**. Para conexiones inalámbricas\-compatibles con equipos que están unidos al dominio antes de implementar una red inalámbrica, si el equipo está conectado a un segmento de la red con cable que no está protegido con 802.1X, los valores de configuración inalámbrica son se aplica automáticamente después de configurar la red inalámbrica \(IEEE 802.11\) directivas en el controlador de dominio y después se actualiza la directiva de grupo en el cliente inalámbrico.  

    Para los equipos que ya no están combinados a su dominio, sin embargo, debe planear un método para aplicar la configuración que son necesaria para 802.1X\-acceso autenticado. Por ejemplo, determine si desea unir el equipo al dominio mediante uno de los métodos siguientes.

    1.  Conectar el equipo a un segmento de la red con cable que no está protegido con 802.1X y, a continuación, unir el equipo al dominio.

    2.  Proporcionar a los usuarios inalámbricos con los pasos y la configuración que necesitan para agregar su propio perfil inalámbrico de bootstrap, lo que permite unir el equipo al dominio.

    3.  Asignar personal de TI para los clientes inalámbricos de unir al dominio.

### <a name="planning-support-for-multiple-standards"></a>Planear la compatibilidad con varios estándares

La red inalámbrica \(IEEE 802.11\) extensión de directivas en la directiva de grupo proporciona una amplia gama de opciones de configuración para admitir una variedad de opciones de implementación.

Puede implementar AP inalámbricos que se configura con los estándares que desea admitir y, a continuación, configure varios perfiles inalámbricos en la red inalámbrica \(IEEE 802.11\) directivas, con cada perfil especifique un conjunto de estándares que requieren.

Por ejemplo, si la red tiene equipos inalámbricos compatibles con WPA2\-Enterprise y AES, otros equipos que son compatibles con WPA\-Enterprise y AES y otros equipos que son compatibles con WPA solo\-Enterprise y TKIP, debe Determine si desea:

- Configurar un perfil único para admitir todos los equipos inalámbricos mediante el método de cifrado más débil que todos los equipos de soporte técnico: en este caso, WPA\-Enterprise y TKIP.  
- Configurar dos perfiles para ofrecer la máxima seguridad posible que sea compatible con todos los equipos inalámbricos. En este caso podría configurar un perfil que especifica el cifrado más seguro \(WPA2\-Enterprise y AES\)y un perfil que usa la más débil WPA\-Enterprise y TKIP cifrado. En este ejemplo, es esencial que colocar el perfil que use WPA2\-Enterprise y AES más alto en el orden de preferencia. Los equipos que no son capaces de usar WPA2\-Enterprise y AES se omita al siguiente perfil en el orden de preferencia y procesar automáticamente el perfil que especifica WPA\-Enterprise y TKIP.

> [!IMPORTANT]
> Debe colocar el perfil con los estándares más seguros superior en la lista ordenada de perfiles, como conexión de equipos utiliza el primer perfil que son capaces de usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planeación de acceso restringido a la red inalámbrica

En muchos casos, puede proporcionar a los usuarios inalámbricos con diferentes niveles de acceso a la red inalámbrica. Por ejemplo, es posible que desee permitir el acceso sin restricciones a los usuarios, las horas del día, cada día de la semana. Para otros usuarios, es posible que solo desea permitir el acceso durante las principales horas, de lunes al viernes y denegar el acceso en el sábado y el domingo.

Esta guía proporciona instrucciones para crear un entorno de acceso que coloca todos los usuarios inalámbricos en un grupo de acceso común a los recursos para servicios inalámbricos. Crear un grupo de seguridad de usuarios de tecnología inalámbrica en los usuarios de Active Directory y ajustar los equipos\-en y, a continuación, realizar todos los usuarios para los que desea conceder acceso inalámbrico a un miembro de ese grupo.

Al configurar las directivas de red NPS, especifique el grupo de seguridad usuarios inalámbricos como el objeto que NPS procesa la hora de determinar la autorización.

Sin embargo, si su implementación requiere compatibilidad con distintos niveles de acceso sólo necesita lo siguiente:  

1. Crear más de un grupo de seguridad inalámbrica de usuarios para crear grupos de seguridad inalámbrica adicionales en equipos y usuarios de Active Directory. Por ejemplo, puede crear un grupo que contenga los usuarios que tienen acceso completo, un grupo para los usuarios que solo tengan acceso durante el horario de trabajo y otros grupos que se ajusten a los otros criterios que se ajuste a sus requisitos.

2. Agregar usuarios a los grupos de seguridad adecuado que ha creado.

3. Configurar directivas de red NPS adicionales para cada grupo de seguridad inalámbrica adicional y configurar las directivas para aplicar las condiciones y restricciones que requieren para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Métodos de planeamiento para agregar nuevos equipos inalámbricos

Es el método preferido para unir equipos inalámbricos nuevo para el dominio y, a continuación, inicie sesión en el dominio mediante el uso de una conexión a un segmento de la LAN que tiene acceso a los controladores de dominio y no está protegido por un conmutador Ethernet de autenticación 802.1X con cable.

En algunos casos, sin embargo, no sería práctico utilizar una conexión con cable para unir equipos al dominio, o bien, para un usuario usar una conexión con cable para su primer inicio de sesión mediante el uso de los equipos que ya se ha unido al dominio.

Para unir un equipo al dominio mediante el uso de una conexión inalámbrica o para los usuarios iniciar sesión en el momento de la primera de dominio mediante el uso de un dominio\-equipo unido a un y una conexión inalámbrica, los clientes inalámbricos en primer lugar deben establecer una conexión a la red inalámbrica en un segmento que se tiene acceso a los controladores de dominio de red mediante uno de los métodos siguientes.

1. **Un miembro del personal de TI une un equipo inalámbrico al dominio y, a continuación, configura un inicio de sesión único perfil inalámbrico de bootstrap.** Con este método, un administrador de TI conecta el equipo inalámbrico a la red Ethernet con cable y, a continuación, une el equipo al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario arranca el equipo, se usan las credenciales de dominio especificadas de manera manual para el proceso de inicio de sesión de usuario a iniciar sesión en el dominio y establecer una conexión a la red inalámbrica.  

2. **El usuario configura de forma manual el equipo inalámbrico con el perfil inalámbrico de bootstrap y, a continuación, une al dominio.** Con este método, los usuarios configurar manualmente sus equipos inalámbricos con un perfil inalámbrico de bootstrap según las instrucciones de un administrador de TI. El perfil inalámbrico de bootstrap permite a los usuarios establecer una conexión inalámbrica y, a continuación, unir el equipo al dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para implementar acceso inalámbrico, consulte [implementación de acceso inalámbrico](e-wireless-access-deployment.md).
