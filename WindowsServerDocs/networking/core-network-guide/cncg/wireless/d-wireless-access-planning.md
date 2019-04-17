---
title: Acceso inalámbrico planeación de implementación
description: Este tema es parte de la Guía de redes de Windows Server 2016 "Implementar basada en contraseña 802.1X X autenticados Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1aa7d9fa66c480988ec7e3a97447157bd3eab9c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-planning"></a>Acceso inalámbrico planeación de implementación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Antes de implementar acceso inalámbrico, debes planear los siguientes elementos:

- Instalación de acceso inalámbrico puntos \(APs\) en la red

- Acceso y configuración de cliente inalámbrico

Las siguientes secciones proporcionan detalles sobre estos pasos.

## <a name="planning-wireless-ap-installations"></a>Planeación de las instalaciones de punto de acceso inalámbricas
Al diseñar la solución de acceso de red inalámbrica, deben hacer lo siguiente:

1. Determinar qué estándares deben ser compatible con los puntos de acceso inalámbricos
2. Determinar las áreas de cobertura donde desea proporcionar servicios inalámbricos
3. Determinar que quieras buscar puntos de acceso inalámbricos

Además, debes planear un esquema de dirección IP de su punto de acceso inalámbrico e inalámbricas los clientes. Consulta la sección **planear la configuración del punto de acceso inalámbrico en NPS** debajo de información para relacionados.

### <a name="verify-wireless-ap-support-for-standards"></a>Compruebe la compatibilidad de punto de acceso inalámbrico estándares
Para los fines de coherencia y facilidad de implementación y administración de punto de acceso, se recomienda implementar puntos de acceso inalámbricos de la misma marca y modelo.

Los puntos de acceso inalámbricos que implementas deben admitir lo siguiente:

- **IEEE 802.1X X**

- **Autenticación de radio**

- **Autenticación inalámbrica y cifrado.** Se enumeran en orden de mayor a menor preferencia:

    1.  Empresa WPA2\ con AES

    2.  Empresa WPA2\ con TKIP

    3.  Empresa WPA\ con AES

    4.  Empresa WPA\ con TKIP

>[!NOTE]
>Para implementar WPA2, debes usar adaptadores de red inalámbrica y puntos de acceso inalámbricos que sean compatibles con WPA2. De lo contrario, usa WPA\ Enterprise.

Además, para proporcionar seguridad mejorada para la red, los puntos de acceso inalámbricos deben admitir las siguientes opciones de seguridad:

- **Filtrado de DHCP.** El punto de acceso inalámbrico debe filtrar en puertos IP para evitar la transmisión de mensajes de difusión DHCP en aquellos casos en que el cliente inalámbrico está configurado como un servidor DHCP. El punto de acceso inalámbrico debe bloquear al cliente envíe paquetes IP de puerto UDP 68a la red.

- **Filtrado de DNS.** El punto de acceso inalámbrico debe filtrar en puertos IP para evitar que a un cliente realizar como un servidor DNS. El punto de acceso inalámbrico debe bloquear al cliente envíe paquetes IP de TCP o UDP puerto 53a la red.

- **El aislamiento de cliente** si el punto de acceso inalámbrico proporciona funcionalidades de aislamiento de cliente, debe habilitar la característica evitar posibles protocolo de resolución de direcciones \(ARP\) ataques de suplantación de identidad.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar las áreas de la cobertura de los usuarios inalámbricos
Usa los dibujos arquitectónicos de cada planta de cada edificio para identificar las áreas donde desea proporcionar cobertura inalámbrica. Por ejemplo, identifica las oficinas apropiadas, salas de conferencias, vestíbulos, cafeterías o courtyards.

En los dibujos, se indican los dispositivos que interfieran con las señales inalámbricas, como equipos médicos, videocámaras inalámbricas, teléfonos inalámbricos que funcionan en la 2.4a 2,5 GHz Industrial, científica y medicina \(ISM\) gama y los dispositivos habilitados para Bluetooth\.

En el plano, marcar los aspectos de la compilación que podrían interferir con señales inalámbricas; objetos de metal usados en la construcción de un edificio pueden afectar a la señal inalámbrica. Por ejemplo, los siguientes objetos comunes pueden interferir con la propagación de señal: ascensores, lleven calentamiento y configuración air\ y vigas concretos de soporte técnico.

Consulta al fabricante de su punto de acceso para obtener información sobre las fuentes que podría provocar la atenuación de radiofrecuencia de punto de acceso inalámbrico. La mayoría de puntos de acceso proporcionan software de prueba que puedes usar para comprobar la intensidad de señal, la tasa de errores y el rendimiento de datos.

### <a name="determine-where-to-install-wireless-aps"></a>Determina dónde quieres instalar los puntos de acceso inalámbricos
En los dibujos arquitectónicos, busque los puntos de acceso inalámbricos cerca lo suficientemente juntos para proporcionar un amplio cobertura inalámbrica pero lo que sea necesario intervalos que no interfieran con entre sí.

La distancia entre puntos de acceso necesaria depende del tipo de punto de acceso y antena PA, aspectos de la compilación que bloquear inalámbrica señales, así como otros orígenes de interferencia. Puedes marcar posiciones de punto de acceso inalámbricos para que cada punto de acceso inalámbrico no más de 300 pies desde cualquier punto de acceso inalámbrico adyacentes. Consulta la documentación del fabricante de punto de acceso inalámbrico para especificaciones de punto de acceso y las directrices para la colocación.

Instale temporalmente los puntos de acceso inalámbricos en las ubicaciones especificadas en tus dibujos arquitectónicos. A continuación, usa un equipo portátil equipado con un adaptador inalámbrico 802.11 y el software de la encuesta de sitio que normalmente se suministra con adaptadores inalámbricos, determinar la intensidad de señal dentro de cada área de cobertura.

En las áreas de cobertura donde la intensidad de señal es baja, coloca el punto de acceso para mejorar la intensidad de señal de la zona de cobertura, instalar los puntos de acceso inalámbricos adicionales para proporcionar la cobertura necesaria, reubicar o quitar orígenes de interferencia de señal.  

Actualizar los dibujos arquitectónicos para indicar la ubicación final de todos los puntos de acceso inalámbricos. Tener un mapa de colocación AP precisa ayudará más tarde durante las operaciones de solución de problemas o cuando quieres actualizar o reemplazar los puntos de acceso.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planear la configuración inalámbrica de punto de acceso y el cliente de RADIUS NPS
Puedes usar NPS para configurar puntos de acceso inalámbricos individualmente o en grupos.

Si estás implementando una red inalámbrica grande que incluye muchos puntos de acceso, es mucho más fácil de configurar puntos de acceso en grupos. Para agregar los puntos de acceso como grupos de clientes RADIUS en NPS, debes configurar los puntos de acceso con estas propiedades.

- Los puntos de acceso inalámbricos están configurados con direcciones IP desde el mismo intervalo de direcciones IP.

- Todos los puntos de acceso inalámbricos están configurados con el mismo secreto compartido.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planear el uso de PEAP reconexión rápida
En una infraestructura de 802.1X, puntos de acceso inalámbrico están configurados como clientes RADIUS de servidores RADIUS. Se implementa cuando reconexión rápida de PEAP, no es necesario para autenticarse con cada nueva asociación de un cliente inalámbrico que se transfieren entre dos o más puntos de acceso.

Reconexión rápida de PEAP reduce el tiempo de respuesta para la autenticación entre el cliente y el autenticador porque la solicitud de autenticación se habrá reenviada desde los puntos de acceso al servidor NPS que originalmente se realizó la autenticación y autorización para la solicitud de conexión de cliente.

Dado que el cliente PEAP y el servidor NPS ambos usan anteriormente en caché las propiedades de conexión de seguridad de la capa de transporte \(TLS\) \ (la colección de los cuales se denomina la handle\ TLS), el servidor NPS puede determinar rápidamente lo que el cliente está autorizado para el restablecimiento de la conexión.

>[!IMPORTANT]
>Para obtener rápida a conectar para que funcione correctamente, los puntos de acceso deben estar configurados como los clientes RADIUS del mismo servidor NPS.

Si el servidor NPS original no está disponible, o si el cliente se mueve a un punto de acceso que esté configurado como un cliente RADIUS a un servidor RADIUS diferentes, se debe producir autenticación completa entre el cliente y el autenticador de nuevo.

### <a name="wireless-ap-configuration"></a>Configuración de punto de acceso inalámbrica
La siguiente lista resume los elementos que normalmente se configuran en 802.1X\-puntos de acceso inalámbricos compatible con:

> [!NOTE]
> Los nombres de elementos pueden variar según la marca y modelo y pueden ser diferentes de las que están en la siguiente lista. Consulte la documentación de punto de acceso inalámbrica para detalles específicos del configuration\.

- **Identificador de \(SSID\)**. Este es el nombre de la red inalámbrica \ (por ejemplo, ExampleWlan\) y el nombre que se anuncia a los clientes inalámbricos. Para reducir la confusión, el SSID que elijas para anunciar no debe coincidir con el SSID difundida por las redes inalámbricas que están dentro del alcance de su red inalámbrica.

    En casos en que se implementan varios puntos de acceso inalámbricos como parte de la misma red inalámbrica, configurar cada punto de acceso inalámbrico con el mismo SSID. En casos en que se implementan varios puntos de acceso inalámbricos como parte de la misma red inalámbrica, configurar cada punto de acceso inalámbrico con el mismo SSID.  

    En casos donde haya una necesidad para implementar diferentes redes inalámbricas para satisfacer las necesidades de negocios específico, el punto de acceso inalámbrico en una red debería difusión otro SSID que el SSID tu otros network\(s\). Por ejemplo, si necesitas una red inalámbrica independiente para los empleados e invitados, podría configurar los puntos de acceso inalámbricos para la red de empresa con la difusión de SSID **ExampleWLAN**. Para la red de invitado, a continuación, puede establecer SSID de cada punto de acceso inalámbrico para difundir **GuestWLAN**. De este modo los empleados e invitados pueden conectarse a la red prevista sin confusiones innecesarios.  

    > [!TIP]  
    > Algunos punto de acceso inalámbrico tiene la capacidad de difusión de SSID varios para dar cabida a las implementaciones de multi\ red. Punto de acceso inalámbrico que puede difusión de SSID varios puede reducir costos de implementación y mantenimiento operativa.  

- **Autenticación y cifrado inalámbrico**.

    Autenticación inalámbrica es la autenticación de seguridad que se usa cuando el cliente inalámbrico se asocia con un punto de acceso inalámbrico.  

    Cifrado inalámbrico es el cifrado de seguridad que se usa con autenticación inalámbrica para proteger las comunicaciones que se envían entre el punto de acceso inalámbrico y el cliente inalámbrico.  

- **Inalámbrica AP IP dirección \(static\)**. En cada punto de acceso inalámbrico, configurar una dirección IP estática. Si la subred es mantenida por un servidor DHCP, asegúrate de que todas las direcciones IP AP entran dentro de un intervalo de exclusión de DHCP para que el servidor DHCP no intenta emitir la misma dirección IP en otro equipo o dispositivo. Los intervalos de exclusión se documentan en el procedimiento "para crear y activar un nuevo ámbito DHCP" en la [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si vas a configurar puntos de acceso como clientes RADIUS por grupo en NPS, cada punto de acceso en el grupo debe tener una dirección IP desde el mismo intervalo de direcciones IP.

- **El nombre DNS**. Algunos puntos de acceso inalámbricos pueden configurarse con un nombre DNS. Configurar cada punto de acceso inalámbrico con un nombre único. Por ejemplo, si tienes un puntos de acceso inalámbricos implementados en un edificio multi\ historia, podría denominar primero tres puntos de acceso inalámbricos se implementan en el tercer suelo AP3\-01, AP3\-02 y AP3\-03.

- **Máscara de subred de punto de acceso inalámbrica**. Configurar la máscara para designar qué parte de la dirección IP dirección es el identificador de red y qué parte de la dirección IP del host.

- **Servicio DHCP AP**. Si el punto de acceso inalámbrico tiene un servicio DHCP built\, deshabilitarlo.

- **Secreto compartido**. Usa un único radio la clave compartida para cada punto de acceso inalámbrico, a menos que vas a configurar a los clientes de RADIUS NPS en grupos - en qué circunstancia debe configurar todos los puntos de acceso en el grupo con el mismo secreto compartido. Secretos compartidos deben ser una secuencia aleatoria de al menos 22 caracteres, con las mayúsculas y minúsculas, números y signos de puntuación. Para garantizar la selección aleatoria, puedes usar un programa de generación de carácter aleatorias para crear tus secretos compartidos. Se recomienda que registre el secreto compartido para cada punto de acceso inalámbrico y guardarla en una ubicación segura, como una oficina segura. Cuando se configura clientes RADIUS en la consola NPS creará una versión virtual de cada punto de acceso. El secreto compartido que se configura en cada punto de acceso virtual en NPS debe coincidir con la clave secreta compartida en el punto de acceso real, físico.

- **Dirección IP del servidor RADIUS**. Escribe la dirección IP del servidor NPS que quieres usar para autenticar y autorizar solicitudes de conexión a este punto de acceso.

- **UDP port\(s\)**. De manera predeterminada, NPS usa los puertos UDP 1812 y 1645 para mensajes de autenticación RADIUS y puertos UDP 1813 y 1646 para los mensajes RADIUS. Se recomienda que no cambia la configuración predeterminada de puertos UDP de RADIUS.

- **VSA**. Algunos puntos de acceso inalámbricos requieren atributos específicos vendor\ \(VSAs\) para proporcionar la funcionalidad de punto de acceso inalámbrica completo.

- **Filtrado de DHCP**. Configurar puntos de acceso inalámbricos para bloquear a los clientes inalámbricos envíe paquetes IP de puerto UDP 68a la red. Consulta la documentación de su punto de acceso inalámbrico configurar el filtrado de DHCP.

- **Filtrado de DNS**. Configurar puntos de acceso inalámbricos para bloquear a los clientes inalámbricos envíe paquetes IP de puerto TCP o UDP 53a la red. Consulta la documentación de su punto de acceso inalámbrico configurar el filtrado de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planeación de acceso y la configuración del cliente inalámbrico

Al planear la implementación de 802.1X\-inalámbrica acceso autenticado, debes tener en cuenta varios factores client\ específicos:

- **Planeación de soporte para estándares de varios**.

    Determinar si los equipos inalámbricos todos utilizan la misma versión de Windows o si son una mezcla de equipos que ejecutan distintos sistemas operativos. Si son diferentes, asegúrate de que comprenden las diferencias en los estándares compatibles con los sistemas operativos.

    Determinar si todos los adaptadores de red inalámbrica en todos los equipos cliente inalámbrico compatible con los mismos estándares inalámbricos, o si necesitas admitir distintos estándares. Por ejemplo, determinar si algunos controladores de hardware de adaptador de red compatibles con WPA2\ Enterprise y AES, mientras que otros admiten solo TKIP y WPA\ de empresa.

- **Modo de autenticación de cliente planeación**. Modos de autenticación definen cómo los clientes de Windows procesan las credenciales de dominio. Puedes seleccionar entre los modos de autenticación de tres red siguientes en las directivas de red inalámbrica.  

    1. **Autenticación del usuario re\**. Este modo se especifica que la autenticación siempre se realiza mediante el uso de credenciales de seguridad basadas en el estado actual del equipo. Cuando no los usuarios inician sesión el equipo, la autenticación se realiza mediante las credenciales del equipo. Cuando un usuario haya iniciado sesión el equipo, la autenticación se realiza siempre con las credenciales de usuario.  

    2. **Equipo solo**. Equipo solo modo especifica que la autenticación se realiza siempre usando solamente las credenciales del equipo.  

    3.  **Autenticación de usuario**. Modo de autenticación de usuario especifica que la autenticación solo se realiza cuando el usuario haya iniciado sesión en el equipo. Cuando no hay ningún usuario que inició sesión en el equipo, no se realizan intentos de autenticación.  

- **Planeación restricciones inalámbricas**. Determinar si desea proporcionar todos los usuarios inalámbricos con el mismo nivel de acceso a la red inalámbrica, o si quieres restringir el acceso de algunos de los usuarios inalámbricos. Puedes aplicar restricciones en NPS frente a determinados grupos de usuarios inalámbricos.  Por ejemplo, puedes definir específico días y horas que determinados grupos se permiten el acceso a la red inalámbrica.  

- **Planeación de métodos para agregar nuevos equipos inalámbricos**. Para equipos compatibles con wireless\ que están unidos al dominio antes de implementar la red inalámbrica, si el equipo está conectado a un segmento de la red por cable que no está protegido con 802.1X, las opciones de configuración inalámbrica se aplican automáticamente después de configurar la red inalámbrica \ (IEEE 802.11\) directivas en el controlador de dominio y después se actualiza la directiva de grupo en el cliente inalámbrico.  

    Para equipos que ya no unidos a tu dominio, sin embargo, debes planear un método para aplicar la configuración que sea necesaria para 802.1X\-acceso autenticado. Por ejemplo, determinar si va a unir el equipo al dominio mediante uno de los siguientes métodos.

    1.  Conecta el equipo a un segmento de la red por cable que no está protegido con 802.1X, a continuación, unir el equipo al dominio.

    2.  Proporcionar a los usuarios inalámbricos con los pasos y la configuración que necesitan para agregar su propio perfil inalámbrico de arranque que les permite unir el equipo al dominio.

    3.  Asignar personal de TI a unirse a los clientes inalámbricos al dominio.

### <a name="planning-support-for-multiple-standards"></a>Planeación de soporte para estándares de varios

La red inalámbrica \ (IEEE 802.11\) extensión de directivas de la directiva de grupo proporciona una amplia gama de opciones de configuración para admitir una variedad de opciones de implementación.

Puedes implementar los puntos de acceso inalámbricos que están configurados con los estándares que quieres admitir y, a continuación, configurar varios perfiles inalámbricos en la red inalámbrica \ (IEEE 802.11\) las directivas, con cada perfil de especificar un conjunto de estándares que necesitas.

Por ejemplo, si dispone de la red inalámbricos equipos que admiten WPA2\ Enterprise y AES, otros equipos que admiten WPA\ Enterprise y AES y otros equipos que admitan solo WPA\ Enterprise y TKIP, debes determinar si quieres:

- Configurar un único perfil para admitir todos los equipos inalámbricos mediante el método de cifrado más débil que todos los equipos admiten - en este caso, WPA\ Enterprise TKIP.  
- Configurar dos perfiles para proporcionar la mayor seguridad posible que es compatible con todos los equipos inalámbrico. En este caso debe configurar un perfil que especifica el cifrado más seguro \ (WPA2\ Enterprise y AES\) y un perfil que usa el cifrado WPA\ Enterprise y TKIP más débil. En este ejemplo, es esencial que realices el perfil que usa AES más altos en el orden de preferencia y WPA2\ de empresa. Los equipos que no son capaces de utilizar WPA2\ Enterprise y AES omitir en el perfil siguiente en el orden de preferencia y procesar el perfil que especifica WPA\ Enterprise y TKIP automáticamente.

> [!IMPORTANT]
> Debes colocar el perfil con los estándares más seguros superior en la lista ordenada de perfiles, porque la conexión de los equipos usa el primer perfil que son capaces de usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planeación de acceso restringido a la red inalámbrica

En muchos casos, es posible que quieres proporcionar a los usuarios inalámbricos con distintos niveles de acceso a la red inalámbrica. Por ejemplo, es posible que desee permitir el acceso sin restricciones a los usuarios, las horas del día, todos los días de la semana. Para otros usuarios, es posible que solo quieres permitir el acceso durante las horas normales, lunes al viernes y denegar el acceso en el sábado y el domingo.

Esta guía brinda instrucciones para crear un entorno de acceso que coloca todos los usuarios inalámbricos en un grupo comunes acceso a recursos inalámbricos. Puedes crear un grupo de seguridad de los usuarios inalámbricos en los equipos y usuarios de Active Directory en snap\ y, a continuación, hacer que todos los usuarios que quieres conceder acceso inalámbrico un miembro de ese grupo.

Al configurar las directivas de red NPS, especificar el grupo de seguridad de los usuarios inalámbricos como el objeto que NPS procesa al determinar la autorización.

Sin embargo, si la implementación requiere compatibilidad con varios niveles de acceso, necesitas solo hacer lo siguiente:  

1. Crea más de un grupo de seguridad de los usuarios inalámbrica para crear grupos de seguridad adicional de inalámbrica en equipos y usuarios de Active Directory. Por ejemplo, puedes crear un grupo que contiene los usuarios que tienen acceso completo, un grupo para aquellos que solo tengan acceso durante el horario de trabajo y otros grupos que cumplan con otros criterios que coincidan con los requisitos.

2. Agregar usuarios a los grupos de seguridad apropiados que creaste.

3. Configurar las directivas de red NPS adicionales para cada grupo de seguridad inalámbrica adicional y configurar las directivas para aplicar las condiciones y restricciones que requieren para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Planeación de métodos para agregar nuevos equipos inalámbricos

El método preferido para unirte a equipos inalámbricos nuevos al dominio y, a continuación, inicie sesión en el dominio es mediante una conexión a un segmento de la red LAN que tiene acceso a controladores de dominio y no está protegido por una autenticación 802.1X conmutador Ethernet por cable.

En algunos casos, sin embargo, no sería práctico usar una conexión por cable para unirte a equipos al dominio, o bien, un usuario puede usar una conexión por cable para su primer inicio de sesión mediante el uso de equipos que ya están unidos al dominio.

Para unir un equipo al dominio mediante una conexión inalámbrica o para los usuarios iniciar sesión en el dominio de la primera vez mediante un equipo unido a domain\ y una conexión inalámbrica, los clientes inalámbricos primero deben establecer una conexión a la red inalámbrica en un segmento que tiene acceso a los controladores de dominio de red mediante uno de los siguientes métodos.

1. **Un miembro del personal de TI se une a un equipo inalámbrico al dominio y, a continuación, configura un inicio de sesión único arrancar perfil inalámbrico.** Con este método, un administrador de TI conecta el equipo inalámbrico a la red Ethernet por cable y después une el equipo al dominio. A continuación, el administrador distribuye el equipo al usuario. Cuando el usuario inicie el equipo, las credenciales de dominio especifican manualmente para el proceso de inicio de sesión de usuario se usan para establecer una conexión a la red inalámbrica tanto iniciar sesión en el dominio.  

2. **El usuario configura manualmente equipo inalámbrico con perfil inalámbrico de inicio y, a continuación, une al dominio.** Con este método, los usuarios configurar manualmente sus equipos inalámbricas con un perfil inalámbrico arranque siguiendo las instrucciones del Administrador de TI. El perfil inalámbrico arrancar permite a los usuarios establecer una conexión inalámbrica y, a continuación, unir el equipo al dominio. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede iniciar sesión en el dominio mediante una conexión inalámbrica y sus credenciales de cuenta de dominio.

Para implementar acceso inalámbrico, consulta [implementación de acceso inalámbrico](e-wireless-access-deployment.md).
