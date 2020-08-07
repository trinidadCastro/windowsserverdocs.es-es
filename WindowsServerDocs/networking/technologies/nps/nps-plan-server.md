---
title: Planear NPS como servidor RADIUS
description: En este tema se proporciona información acerca de la planeación de la implementación de servidores RADIUS del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 73e85d9582e447466115ce30047a3968a8f457d8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952010"
---
# <a name="plan-nps-as-a-radius-server"></a>Planear NPS como servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando se implementa NPS del servidor de directivas \( \) de redes como un servidor servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación, la autorización y la administración de cuentas para las solicitudes de conexión para el dominio local y para los dominios que confían en el dominio local. Puede utilizar estas directrices de planeación para simplificar la implementación de RADIUS.

Estas directrices de planeación no incluyen las circunstancias en las que desea implementar NPS como un proxy RADIUS. Cuando se implementa NPS como un proxy RADIUS, NPS reenvía las solicitudes de conexión a un servidor que ejecuta NPS u otros servidores RADIUS en dominios remotos, dominios que no son de confianza o ambos.

Antes de implementar NPS como un servidor RADIUS en la red, use las siguientes directrices para planear la implementación.

- Planear la configuración de NPS.

- Planear clientes RADIUS.

- Planear el uso de métodos de autenticación.

- Planear directivas de red.

- Planear cuentas NPS.

## <a name="plan-nps-configuration"></a>Planear la configuración de NPS

Debe decidir en qué dominio es miembro el NPS. En entornos de varios dominios, un NPS puede autenticar las credenciales para las cuentas de usuario en el dominio del que es miembro y para todos los dominios que confían en el dominio local del NPS. Para permitir que NPS Lea las propiedades de acceso telefónico de las cuentas de usuario durante el proceso de autorización, debe agregar la cuenta de equipo del NPS al grupo RAS y NPSs para cada dominio.

Después de determinar la pertenencia al dominio del NPS, el servidor debe estar configurado para comunicarse con clientes RADIUS, también denominados servidores de acceso a la red, mediante el protocolo RADIUS. Además, puede configurar los tipos de eventos que NPS registra en el registro de eventos y puede escribir una descripción para el servidor.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de la configuración de NPS, puede seguir estos pasos.

- Determine los puertos RADIUS que NPS usa para recibir mensajes RADIUS de clientes RADIUS. Los puertos predeterminados son los puertos UDP 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos 1813 y 1646 para los mensajes de cuentas RADIUS.

- Si el NPS está configurado con varios adaptadores de red, determine los adaptadores en los que desea que se permita el tráfico RADIUS.

- Determine los tipos de eventos que desea que NPS registre en el registro de eventos. Puede registrar solicitudes de autenticación rechazadas, solicitudes de autenticación correctas o ambos tipos de solicitudes.

- Determine si va a implementar más de un NPS. Para proporcionar tolerancia a errores para la autenticación y las cuentas basadas en RADIUS, use al menos dos NPSs. Un NPS se usa como el servidor RADIUS principal y el otro se usa como copia de seguridad. Después, cada cliente RADIUS se configura en ambos NPSs. Si el NPS principal deja de estar disponible, los clientes RADIUS enviarán mensajes de solicitud de acceso al NPS alternativo.

- Planee el script usado para copiar una configuración de NPS en otros NPSs para ahorrar en la sobrecarga administrativa y para evitar el configuración incorrecto de un servidor. NPS proporciona los comandos Netsh que le permiten copiar toda o parte de una configuración de NPS para la importación en otro NPS. Puede ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guarda la secuencia de comandos como un script, puede ejecutar el script en una fecha posterior si decide cambiar las configuraciones de servidor.

## <a name="plan-radius-clients"></a>Planeación de clientes RADIUS

Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso inalámbricos, servidores de red privada virtual (VPN), conmutadores compatibles con 802.1 X y servidores de acceso telefónico. Los proxies RADIUS, que reenvían los mensajes de solicitud de conexión a los servidores RADIUS, también son clientes RADIUS. NPS es compatible con todos los servidores de acceso a la red y los proxies RADIUS que cumplen con el protocolo RADIUS, tal como se describe en RFC 2865, "servicio de autenticación remota telefónica de usuario (RADIUS)" y RFC 2866, "cuentas RADIUS".

>[!IMPORTANT]
>Los clientes de acceso, como los equipos cliente, no son clientes RADIUS. Solo los servidores de acceso a la red y los servidores proxy que admiten el protocolo RADIUS son clientes RADIUS.

Además, los puntos de acceso inalámbrico y los conmutadores deben ser capaces de la autenticación de 802.1 X. Si desea implementar el protocolo de autenticación extensible \( EAP o el \) Protocolo de autenticación extensible protegido \( PEAP \) , los puntos de acceso y los conmutadores deben admitir el uso de EAP.

Para probar la interoperabilidad básica de las conexiones PPP para puntos de acceso inalámbricos, configure el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Use protocolos de autenticación basados en PPP adicionales, como PEAP, hasta que haya probado los que piensa usar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de clientes RADIUS, puede seguir estos pasos.

- Documente los atributos específicos del proveedor (VSA) que debe configurar en NPS. Si los servidores de acceso a la red requieren VSA, registre la información de VSA para su uso posterior al configurar las directivas de red en NPS.

- Documente las direcciones IP de los clientes RADIUS y NPS para simplificar la configuración de todos los dispositivos. Cuando implemente los clientes RADIUS, debe configurarlos para que usen el protocolo RADIUS, con la dirección IP de NPS especificada como servidor de autenticación. Y al configurar NPS para que se comunique con los clientes RADIUS, debe especificar las direcciones IP del cliente RADIUS en el complemento NPS.

- Cree secretos compartidos para la configuración en los clientes RADIUS y en el complemento NPS. Debe configurar los clientes RADIUS con un secreto compartido, o una contraseña, que también escribirá en el complemento NPS mientras configura los clientes RADIUS en NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planear el uso de métodos de autenticación

NPS admite métodos de autenticación basados en contraseña y basados en certificados. Sin embargo, no todos los servidores de acceso a la red admiten los mismos métodos de autenticación. En algunos casos, puede que desee implementar un método de autenticación diferente en función del tipo de acceso a la red.

Por ejemplo, puede que desee implementar el acceso inalámbrico y VPN para su organización, pero usar un método de autenticación diferente para cada tipo de acceso: EAP-TLS para conexiones VPN, debido a la fuerte seguridad que proporciona EAP con seguridad de la capa de transporte (EAP-TLS), y PEAP-MS-CHAP v2 para conexiones inalámbricas 802.1 X.

PEAP con el protocolo de autenticación por desafío mutuo de Microsoft versión 2 (PEAP-MS-CHAP v2) proporciona una característica denominada reconexión rápida que está diseñada específicamente para su uso con equipos portátiles y otros dispositivos inalámbricos. La reconexión rápida permite a los clientes inalámbricos moverse entre puntos de acceso inalámbrico de la misma red sin que se vuelvan a autenticar cada vez que se asocien a un nuevo punto de acceso. Esto proporciona una mejor experiencia para los usuarios inalámbricos y les permite moverse entre los puntos de acceso sin tener que volver a escribir sus credenciales.
Debido a la reconexión rápida y la seguridad que proporciona PEAP-MS-CHAP V2, PEAP-MS-CHAP V2 es una opción lógica como método de autenticación para las conexiones inalámbricas.

En el caso de las conexiones VPN, EAP-TLS es un método de autenticación basado en certificados que proporciona una seguridad segura que protege el tráfico de red incluso cuando se transmite a través de Internet desde equipos domésticos o móviles a los servidores VPN de la organización.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticación basados en certificados.

Los métodos de autenticación basados en certificados tienen la ventaja de proporcionar una seguridad sólida. y tienen la desventaja de ser más difíciles de implementar que los métodos de autenticación basados en contraseñas.

Tanto PEAP-MS-CHAP V2 como EAP-TLS son métodos de autenticación basados en certificados, pero hay muchas diferencias entre ellos y el modo en que se implementan.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa certificados para la autenticación de cliente y de servidor, y requiere la implementación de una infraestructura de clave pública (PKI) en la organización. La implementación de una PKI puede ser compleja y requiere una fase de planeación independiente de la planeación del uso de NPS como un servidor RADIUS.

Con EAP-TLS, el NPS inscribe un certificado de servidor de una CA de entidad de certificación \( \) y el certificado se guarda en el equipo local en el almacén de certificados. Durante el proceso de autenticación, la autenticación de servidor se produce cuando NPS envía su certificado de servidor al cliente de acceso para demostrar su identidad al cliente de acceso. El cliente de acceso examina varias propiedades de certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos mínimos de certificado de servidor y lo emite una CA en la que confía el cliente de acceso, el cliente autentica el NPS correctamente.

Del mismo modo, la autenticación de cliente se produce durante el proceso de autenticación cuando el cliente envía su certificado de cliente al NPS para demostrar su identidad al NPS. NPS examina el certificado y, si el certificado de cliente cumple los requisitos mínimos de certificado de cliente y lo emite una entidad de certificación en la que el NPS confía, el NPS autentica el cliente de acceso correctamente.

Aunque es necesario que el certificado de servidor se almacene en el almacén de certificados en el NPS, el certificado de cliente o de usuario puede almacenarse en el almacén de certificados del cliente o en una tarjeta inteligente.

Para que este proceso de autenticación se realice correctamente, es necesario que todos los equipos tengan el certificado de CA de la organización en el almacén de certificados de entidades de certificación raíz de confianza para el equipo local y el usuario actual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP V2 usa un certificado para la autenticación de servidor y las credenciales basadas en contraseña para la autenticación de usuarios. Dado que los certificados se usan solo para la autenticación de servidor, no es necesario implementar una PKI para poder usar PEAP-MS-CHAP v2. Cuando se implementa PEAP-MS-CHAP V2, puede obtener un certificado de servidor para NPS de una de las dos maneras siguientes:

- Puede instalar Active Directory servicios de Certificate Server (AD CS) y, a continuación, realizar la inscripción automática de certificados en NPSs. Si usa este método, también debe inscribir el certificado de CA en los equipos cliente que se conectan a la red para que confíen en el certificado emitido para el NPS.

- Puede adquirir un certificado de servidor de una entidad de certificación pública como VeriSign. Si usa este método, asegúrese de seleccionar una entidad de certificación que ya sea de confianza para los equipos cliente. Para determinar si los equipos cliente confían en una CA, abra el complemento Microsoft Management Console (MMC) de certificados en un equipo cliente y, a continuación, vea el almacén de entidades de certificación raíz de confianza para el equipo local y para el usuario actual. Si hay un certificado de la CA en estos almacenes de certificados, el equipo cliente confía en la CA y, por tanto, confiará en cualquier certificado emitido por la CA.

Durante el proceso de autenticación con PEAP-MS-CHAP V2, la autenticación de servidor se produce cuando NPS envía su certificado de servidor al equipo cliente. El cliente de acceso examina varias propiedades de certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos mínimos de certificado de servidor y lo emite una CA en la que confía el cliente de acceso, el cliente autentica el NPS correctamente.

La autenticación de usuario se produce cuando un usuario que intenta conectarse a la red escribe credenciales basadas en contraseña e intenta iniciar sesión. NPS recibe las credenciales y realiza la autenticación y autorización. Si el usuario está autenticado y autorizado correctamente, y si el equipo cliente autenticó correctamente el NPS, se concede la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante la planeación del uso de métodos de autenticación, puede seguir estos pasos.

- Identifique los tipos de acceso a la red que planea ofrecer, como la conexión inalámbrica, VPN, el conmutador compatible con 802.1 X y el acceso telefónico.

- Determine el método de autenticación o los métodos que desea usar para cada tipo de acceso. Se recomienda usar los métodos de autenticación basados en certificados que proporcionan una seguridad sólida. sin embargo, puede que no resulte práctico implementar una PKI, por lo que otros métodos de autenticación podrían proporcionar un equilibrio mejor de lo que necesita para la red.

- Si va a implementar EAP-TLS, planee la implementación de la PKI. Esto incluye la planeación de las plantillas de certificado que se van a usar para certificados de servidor y certificados de equipo cliente. También incluye la determinación de cómo inscribir certificados en equipos miembro de dominio y no pertenecientes a un dominio, y cómo determinar si desea utilizar tarjetas inteligentes.

- Si está implementando PEAP-MS-CHAP V2, determine si desea instalar AD CS para emitir certificados de servidor a su NPSs o si desea adquirir certificados de servidor de una entidad de certificación pública, como VeriSign.

### <a name="plan-network-policies"></a>Planear directivas de red

NPS usa las directivas de red para determinar si las solicitudes de conexión recibidas de los clientes RADIUS están autorizadas. NPS también usa las propiedades de acceso telefónico de la cuenta de usuario para realizar una determinación de la autorización.

Dado que las directivas de red se procesan en el orden en que aparecen en el complemento NPS, Planee colocar las directivas más restrictivas en primer lugar en la lista de directivas. Para cada solicitud de conexión, NPS intenta hacer coincidir las condiciones de la Directiva con las propiedades de la solicitud de conexión. NPS examina cada directiva de red en orden hasta que encuentra una coincidencia. Si no encuentra ninguna coincidencia, se rechaza la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de las directivas de red, puede usar los pasos siguientes.

- Determine el orden de procesamiento de NPS preferido de las directivas de red, de más restrictivas a menos restrictivas.

- Determine el estado de la Directiva. El estado de la Directiva puede tener el valor de habilitado o deshabilitado. Si la Directiva está habilitada, NPS evalúa la Directiva mientras realiza la autorización. Si la Directiva no está habilitada, no se evalúa.

- Determine el tipo de directiva. Debe determinar si la Directiva está diseñada para conceder acceso cuando la solicitud de conexión coincida con las condiciones de la Directiva o si la Directiva está diseñada para denegar el acceso cuando la solicitud de conexión coincida con las condiciones de la Directiva. Por ejemplo, si desea denegar explícitamente el acceso inalámbrico a los miembros de un grupo de Windows, puede crear una directiva de red que especifique el grupo, el método de conexión inalámbrica y un valor de tipo de directiva de denegar acceso.

- Determine si desea que NPS omita las propiedades de acceso telefónico de las cuentas de usuario que son miembros del grupo en el que se basa la Directiva. Cuando esta opción no está habilitada, las propiedades de acceso telefónico de las cuentas de usuario invalidan las opciones configuradas en las directivas de red. Por ejemplo, si se configura una directiva de red que concede acceso a un usuario pero las propiedades de acceso telefónico de la cuenta de usuario para ese usuario están configuradas para denegar el acceso, se deniega el acceso al usuario. Pero si habilita la configuración de tipo de directiva omitir las propiedades de acceso telefónico de la cuenta de usuario, se concede al mismo usuario acceso a la red.

- Determine si la directiva utiliza la configuración de origen de la Directiva. Esta configuración permite especificar fácilmente un origen para todas las solicitudes de acceso. Los orígenes posibles son una puerta de enlace de Terminal Services (puerta de enlace de TS), un servidor de acceso remoto (VPN o de acceso telefónico), un servidor DHCP, un punto de acceso inalámbrico y un servidor de autoridad de registro de mantenimiento. También puede especificar un origen específico del proveedor.

- Determine las condiciones que deben coincidir para que se aplique la Directiva de red.

- Determine la configuración que se aplica si la solicitud de conexión coincide con las condiciones de la Directiva de red.

- Determine si desea usar, modificar o eliminar las directivas de red predeterminadas.

## <a name="plan-nps-accounting"></a>Planear cuentas NPS

NPS proporciona la capacidad de registrar datos de cuentas de RADIUS, como las solicitudes de autenticación y cuentas de usuario, en tres formatos: formato IAS, formato compatible con la base de datos y registro de Microsoft SQL Server.

Formato IAS y formato compatible con la base de datos cree archivos de registro en el NPS local en formato de archivo de texto.

El registro de SQL Server proporciona la capacidad de iniciar sesión en una base de datos de SQL Server 2000 o SQL Server 2005 compatible con XML, extendiendo cuentas de RADIUS para aprovechar las ventajas del registro en una base de datos relacional.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de cuentas NPS, puede seguir estos pasos.

- Determine si desea almacenar los datos de cuentas NPS en archivos de registro o en una base de datos de SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Cuentas NPS mediante archivos de registro locales

La grabación de las solicitudes de autenticación y cuentas de usuario en los archivos de registro se utiliza principalmente para el análisis de la conexión y la facturación, y también es útil como herramienta de investigación de seguridad, lo que le proporciona un método para realizar el seguimiento de la actividad de un usuario malintencionado tras un ataque.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de cuentas NPS mediante archivos de registro locales, puede usar los pasos siguientes.

- Determine el formato del archivo de texto que desea utilizar para los archivos de registro de NPS.

- Elija el tipo de información que desea registrar. Puede registrar solicitudes de cuentas, solicitudes de autenticación y estado periódico.

- Determine la ubicación del disco duro donde desea almacenar los archivos de registro.

- Diseñe la solución de copia de seguridad del archivo de registro. La ubicación del disco duro donde se almacenan los archivos de registro debe ser una ubicación que le permita realizar una copia de seguridad de los datos fácilmente. Además, la ubicación del disco duro debe protegerse configurando la lista de control de acceso (ACL) de la carpeta donde se almacenan los archivos de registro.

- Determine la frecuencia con la que desea crear los nuevos archivos de registro. Si desea que los archivos de registro se creen en función del tamaño del archivo, determine el tamaño máximo de archivo permitido antes de que NPS cree un nuevo archivo de registro.

- Determine si desea que NPS elimine los archivos de registro más antiguos si el disco duro se queda sin espacio de almacenamiento.

- Determinar la aplicación o aplicaciones que desea usar para ver los datos de cuentas y generar informes.

### <a name="nps-sql-server-logging"></a>Registro de SQL Server de NPS

El registro de SQL Server de NPS se usa cuando se necesita información de estado de sesión, para la creación de informes y análisis de datos, y para centralizar y simplificar la administración de los datos de cuentas.

NPS proporciona la capacidad de usar el registro de SQL Server para registrar solicitudes de autenticación y cuentas de usuario recibidas de uno o más servidores de acceso a la red en un origen de datos de un equipo que ejecute Microsoft SQL Server Desktop Engine \( MSDE 2000 \) , o cualquier versión de SQL Server posterior a SQL Server 2000.

Los datos de cuentas se pasan desde NPS en formato XML a un procedimiento almacenado en la base de datos, que admite tanto SQL XML como lenguaje de consulta estructurado \( SQL \) \( SQLXML \) . La grabación de solicitudes de autenticación y cuentas de usuario en una base de datos de SQL Server compatible con XML permite que varios NPSs tengan un origen de datos.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de cuentas NPS mediante el registro de SQL Server de NPS, puede seguir estos pasos.

- Determine si usted u otro miembro de su organización tiene SQL Server experiencia de desarrollo de base de datos relacional 2000 o SQL Server 2005 y sabe cómo usar estos productos para crear, modificar, administrar y administrar bases de datos de SQL Server.

- Determine si SQL Server está instalado en el NPS o en un equipo remoto.

- Diseñe el procedimiento almacenado que utilizará en la base de datos de SQL Server para procesar los archivos XML entrantes que contienen datos de cuentas NPS.

- Diseñar la estructura y el flujo de replicación de la base de datos SQL Server.

- Determinar la aplicación o aplicaciones que desea usar para ver los datos de cuentas y generar informes.

- Planear el uso de servidores de acceso a la red que envían el atributo de clase en todas las solicitudes de cuentas. El atributo de clase se envía al cliente RADIUS en un mensaje de aceptación de acceso y es útil para correlacionar mensajes de solicitud de cuentas con sesiones de autenticación. Si el servidor de acceso a la red envía el atributo de clase en los mensajes de solicitud de cuentas, se puede usar para hacer coincidir los registros de cuentas y de autenticación. La combinación de los atributos Unique-serial-number, Service-reboot-Time y Server-Address debe ser una identificación única para cada autenticación que el servidor acepte.

- Planear el uso de servidores de acceso a la red que admiten cuentas provisionales.

- Planear el uso de servidores de acceso a la red que envían mensajes de contabilidad y de contabilidad.

- Planear el uso de servidores de acceso a la red que admiten el almacenamiento y el reenvío de datos de cuentas. Los servidores de acceso a la red que admiten esta característica pueden almacenar datos de cuentas cuando el servidor de acceso a la red no se puede comunicar con el NPS. Cuando el NPS está disponible, el servidor de acceso a la red reenvía los registros almacenados al NPS, lo que proporciona una mayor confiabilidad en la administración de los servidores de acceso a la red que no proporcionan esta característica.

- Planee siempre configurar el atributo Acct-Interim-Interval en las directivas de red. El atributo Acct-Interim-Interval establece el intervalo (en segundos) entre cada actualización provisional que envía el servidor de acceso a la red. Según RFC 2869, el valor del atributo Acct-Interim-Interval no debe ser inferior a 60 segundos, o un minuto, y no debe ser inferior a 600 segundos o 10 minutos. Para obtener más información, vea RFC 2869, "extensiones de RADIUS".

- Asegúrese de que el registro de estado periódico está habilitado en su NPSs.

