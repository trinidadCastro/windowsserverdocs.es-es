---
title: Planear NPS como un servidor de radio
description: Este tema proporciona información sobre la implementación de servidores RADIUS de servidor de directivas de red planificación en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9bb95428c17e5d7bde588693e84288ddfe64531
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-server"></a>Planear NPS como un servidor de radio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Cuando implementas \(NPS\) el servidor de directivas de red como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación, autorización y contabilidad solicitudes de conexión para el dominio local y en los dominios que confían en el dominio local. Puedes usar estas directrices de diseño para simplificar la implementación de RADIUS.

Estas directrices de diseño no incluyen circunstancias en que quieres implementar NPS como un proxy RADIUS. Cuando implementas NPS como proxy RADIUS, NPS reenvía solicitudes de conexión a un servidor que ejecute NPS u otros servidores RADIUS en dominios remotos, dominios de confianza o ambas cosas. 

Antes de implementar NPS como un servidor RADIUS de la red, usa las directrices siguientes para planear la implementación.

- Planear la configuración del servidor NPS.

- Planea a los clientes RADIUS.

- Planear el uso de métodos de autenticación.

- Planear las directivas de red.

- Planear la administración de cuentas de NPS.

## <a name="plan-nps-server-configuration"></a>Planear la configuración del servidor NPS

Debes decidir en qué dominio del servidor NPS es un miembro. Para entornos de varios dominios, un servidor NPS puede autenticar las credenciales para cuentas de usuario en el dominio del que es un miembro y para todos los dominios que confían en el dominio local del servidor NPS. Para permitir que el servidor NPS leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización, debes agregar la cuenta de equipo del servidor NPS al grupo de servidores RAS y NPS para cada dominio.

Después de determinar la pertenencia a dominio del servidor NPS, el servidor debe configurarse para comunicarse con los clientes RADIUS, también denominados servidores de acceso de red mediante el protocolo RADIUS. Además, puedes configurar los tipos de eventos que grabe NPS en el caso de registro y escribir una descripción para el servidor.

### <a name="key-steps"></a>Pasos clave

Durante la planificación de la configuración del servidor NPS, puedes usar los siguientes pasos.

- Determinar los puertos de radio que use el servidor NPS para recibir mensajes de radio de clientes de RADIUS. Los puertos predeterminados son los puertos UDP 1812 y 1645 para los mensajes de autenticación de RADIUS y puertos 1813 y 1646 para los mensajes RADIUS.

- Si el servidor NPS está configurado con varios adaptadores de red, determina los adaptadores sobre la que quieres que el tráfico de radio para poder.

- Determinar los tipos de eventos que desee NPS para grabar en el registro de eventos. Puede iniciar solicitudes de autenticación rechazadas, solicitudes de autenticación correcta o ambos tipos de solicitudes.

- Determinar si vas a implementar más de un servidor NPS. Para proporcionar tolerancia a errores para la autenticación basada en RADIUS y cuentas, usar al menos dos servidores NPS. Un servidor NPS se usa como el servidor RADIUS principal y el otro se usa como una copia de seguridad. Cada cliente RADIUS, a continuación, se configura en los servidores NPS. Si el servidor NPS principal no está disponible, los clientes RADIUS, a continuación, enviar mensajes de solicitud de acceso en el servidor NPS alternativo.

- Planear el script que se usa para copiar una configuración del servidor NPS a otros servidores NPS guardar en la sobrecarga administrativa y para evitar que la configuración incorrecta de un servidor. NPS proporciona los comandos Netsh que le permitan copiar todo o parte de una configuración del servidor NPS para importarlos en otro servidor NPS. Puedes ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guardas la secuencia de comandos como un script, puede ejecutar el script en una fecha posterior si decides cambiar la configuración del servidor.

## <a name="plan-radius-clients"></a>Clientes RADIUS de plan

Clientes de RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico, servidores de red privada virtual (VPN), 802.1X conmutadores compatible con X y los servidores de acceso telefónico. Servidores proxy de radio, que reenviar mensajes de solicitud a servidores RADIUS de conexión, también son clientes de RADIUS. NPS es compatible con todos los servidores de acceso de red y servidores proxy de radio que cumplan con el radio de protocolo como se describe en la RFC 2865, "Servicio autenticación remota telefónica de usuario (RADIUS)," y RFC 2866, "Cuentas RADIUS".

>[!IMPORTANT]
>Los clientes de acceso, como los equipos cliente, no son clientes de RADIUS. Solo los servidores de acceso de red y los servidores proxy que admiten el protocolo RADIUS son clientes de RADIUS.

Además, los puntos de acceso inalámbrico y cambia debe ser capaz de autenticación 802.1X. Si quieres implementar el protocolo de autenticación Extensible \(EAP\) o el protocolo de autenticación Extensible protegido \(PEAP\), puntos de acceso y modificadores deben admitir el uso de EAP.

Para probar la interoperabilidad básica para las conexiones de PPP para puntos de acceso inalámbrico, configurar el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Usa protocolos de autenticación adicional basado en PPP, como PEAP, hasta que haya probado las que vayas a usar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para clientes RADIUS, puedes usar los siguientes pasos.

- Atributos de documento específico del proveedor (VSA) debe configurar en NPS. Si los servidores de acceso de red necesitan VSA, registrar la información de VSA para su uso posterior al configurar las directivas de red en NPS. 

- Documentar las direcciones IP de los clientes RADIUS y el servidor NPS para simplificar la configuración de todos los dispositivos. Cuando implementas los clientes RADIUS, debes configurar para usar el protocolo RADIUS, con la dirección IP del servidor NPS escrita como el servidor de autenticación. Y cuando se configura NPS para comunicarse con los clientes RADIUS, tienes que escribir las direcciones IP de cliente RADIUS en el complemento de NPS.

- Crear secretos compartidos para la configuración de los clientes RADIUS y en el complemento NPS. Debes configurar a clientes RADIUS con un secreto compartido o la contraseña, que también se especifica en el complemento NPS durante la configuración de clientes de RADIUS en NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planear el uso de métodos de autenticación

NPS es compatible con ambos métodos de autenticación basada en contraseña y basada en certificados. Sin embargo, no todos los servidores de acceso de red admiten los mismos métodos de autenticación. En algunos casos, es posible que quieras implementar un método de autenticación diferentes en función del tipo de acceso de red.

Por ejemplo, es posible que quieras implementar conexión inalámbrica y el acceso VPN para la organización, pero utilizar un método de autenticación diferentes para cada tipo de acceso: EAP-TLS para las conexiones VPN, debido a la seguridad sólida que proporciona EAP con seguridad de la capa de transporte (EAP-TLS) y PEAP-MS-CHAPv2 para conexiones inalámbricas 802.1X.

Vuelve a conectar PEAP con Microsoft protocolo de autenticación versión 2 (PEAP-MS-CHAPv2) proporciona una característica denominada rápida que está diseñado específicamente para su uso con equipos portátiles y otros dispositivos inalámbricos. Permite a los clientes inalámbricos moverse entre puntos de acceso inalámbrico en la misma red sin necesidad de autenticarse cada vez que se asocian a un nuevo punto de acceso de la reconexión rápida. Esto proporciona una mejor experiencia para los usuarios inalámbricos y les permite moverse entre puntos de acceso sin tener que introducir sus credenciales.
Debido a rápido vuelve a conectar y la seguridad que proporciona PEAP-MS-CHAPv2, PEAP-MS-CHAPv2 es una opción lógica como método de autenticación para conexiones inalámbricas.

Para las conexiones VPN, EAP-TLS es un método de autenticación basada en certificados que ofrece una seguridad sólida que protege el tráfico de red, incluso cuando se transmite a través de Internet desde casa o móvil equipos a los servidores VPN de la organización.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticación basada en certificados

Métodos de autenticación basada en certificados tienen la ventaja de proporcionar seguridad sólida; y tienen la desventaja de ser más difíciles de implementar que métodos de autenticación basada en contraseña.

PEAP-MS-CHAPv2 y EAP-TLS son métodos de autenticación basada en certificados, pero hay muchas diferencias entre ellos y la forma en que se implementan.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa certificados para la autenticación de cliente y servidor y requiere que implementar una infraestructura de clave pública (PKI) de la organización. Implementar una PKI pueden ser compleja y requiere una fase de diseño que es independiente de diseño para el uso de NPS como un servidor RADIUS.

Con EAP-TLS, el servidor NPS inscribe un certificado de servidor de una entidad de certificación \(CA\) y el certificado se guarda en el equipo local en el almacén de certificados. Durante el proceso de autenticación, autenticación de servidor se produce cuando el servidor NPS envía su certificado de servidor al cliente acceso para probar su identidad al cliente de acceso. El cliente de acceso examina diversas propiedades de certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos de certificado de servidor mínimo y emitido por una entidad de certificación que confíe en el cliente de acceso, el cliente correctamente autentica el servidor NPS.

Del mismo modo, autenticación de cliente se produce durante el proceso de autenticación cuando el cliente envía su certificado de cliente en el servidor NPS para probar su identidad en el servidor NPS. El servidor NPS examina el certificado y, si el certificado de cliente cumple los requisitos de certificado de cliente mínimo y emitido por una entidad de certificación que confíe en el servidor NPS, el cliente de acceso se autenticó correctamente por el servidor NPS.

Si bien es necesario que el certificado de servidor se almacena en el almacén de certificados en el servidor NPS, el certificado de cliente o usuario puede almacenarse en el almacén de certificados en el cliente o en una tarjeta inteligente.

Para que este proceso de autenticación correctamente, es necesario que todos los equipos tienen el certificado de CA de la organización en el almacén de certificados de entidades de certificación raíz de confianza para el equipo Local y el usuario actual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAPv2

PEAP-MS-CHAPv2 usa un certificado de autenticación de servidores y credenciales basadas en contraseña para la autenticación de usuario. Como los certificados se usan solo para la autenticación de servidor, no son necesarios para implementar una PKI para poder usar PEAP-MS-CHAPv2. Al implementar PEAP-MS-CHAPv2, puede obtener un certificado de servidor para el servidor NPS en una de las siguientes dos maneras:

- Puedes instalar servicios de certificados de Active Directory (AD CS) y, a continuación, los certificados de inscripción automática en servidores NPS. Si usas este método, también debe inscribir el certificado de CA en los equipos cliente conectarse a la red para que confiar en el certificado emitido para el servidor NPS.

- Puedes comprar un certificado de servidor de una CA pública, como VeriSign. Si usas este método, asegúrate de que seleccionar una entidad de certificación que ya sea de confianza para los equipos cliente. Para determinar si los equipos cliente confían en una entidad de certificación, abre el complemento Microsoft Management Console (MMC) de certificados en un equipo cliente y, a continuación, ver el almacén de entidades de certificación raíz de confianza para el equipo Local y para el usuario actual. Si hay un certificado de la CA en estos almacenes de certificados, el equipo cliente confía en la CA y, por tanto, confíe en cualquier certificado emitido por la CA.

Durante el proceso de autenticación con PEAP-MS-CHAPv2, autenticación de servidor se produce cuando el servidor NPS envía su certificado de servidor en el equipo cliente. El cliente de acceso examina diversas propiedades de certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos de certificado de servidor mínimo y emitido por una entidad de certificación que confíe en el cliente de acceso, el cliente correctamente autentica el servidor NPS.

Autenticación de usuario se produce cuando un usuario intenta conectarse a las credenciales de red tipos basada en contraseña e intenta iniciar sesión. NPS recibe las credenciales y realiza la autenticación y autorización. Si el usuario está autenticado y autorizado correctamente, y si el equipo cliente correctamente autentica el servidor NPS, se concede la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para el uso de métodos de autenticación, puedes usar los siguientes pasos.

- Identificar los tipos de acceso a la red que estás pensando ofrecer, por ejemplo, inalámbrico, VPN, 802.1X modificador compatible con X y acceso telefónico.

- Determinar el método de autenticación o métodos que quieres usar para cada tipo de acceso. Se recomienda que puedes usar los métodos de autenticación basada en certificados que proporcionan una seguridad eficaz; Sin embargo, no sería práctico para implementar una PKI, por lo que otros métodos de autenticación podrían proporcionar un equilibrio mejor de lo que necesitas para la red.

- Si vas a implementar EAP-TLS, planear la implementación de infraestructura de clave pública. Esto incluye la planeación de las plantillas de certificado que se va a usar para certificados de servidor y certificados de equipo cliente. También incluye determinar cómo inscribirse certificados a determinar si quieres usar tarjetas inteligentes, miembro de dominio y equipos miembro no del dominio.

- Si vas a implementar PEAP-MS-CHAPv2, determinar si desea instalar AD CS para emitir certificados de servidor a los servidores NPS o si quieres comprar certificados de servidor de una CA pública, como VeriSign.

### <a name="plan-network-policies"></a>Plan de directivas de red

Las directivas de red se usan por NPS para determinar si las solicitudes de conexión recibidas de los clientes RADIUS están autorizadas. NPS también usa las propiedades de marcado de la cuenta de usuario para realizar una determinación de autorización.

Dado que se procesan las directivas de red en el orden en que aparecen en el complemento NPS, planea colocar las directivas más restrictivas primero en la lista de directivas. Para cada solicitud de conexión, NPS se intenta hacer coincidir las condiciones de la directiva con las propiedades de solicitud de conexión. NPS examina cada directiva de red en orden hasta que encuentra a una coincidencia. Si no encuentra a una coincidencia, se rechaza la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante el diseño de directivas de red, puedes usar los siguientes pasos.

- Determinar el orden de procesamiento de NPS preferido de directivas de red, desde la más restrictiva a menos restrictiva.

- Determinar el estado de la directiva. El estado de directiva puede tener el valor de habilitado o deshabilitado. Si la directiva está habilitada, NPS evalúa la directiva al realizar la autorización. Si no se habilita la directiva, no se evalúa.

- Determinar el tipo de directiva. Debes determinar si la directiva está diseñada para conceder acceso cuando se cumplen las condiciones de la directiva de la solicitud de conexión o si la directiva está diseñada para denegar el acceso cuando se cumplen las condiciones de la directiva de la solicitud de conexión. Por ejemplo, si quieres explícitamente denegar acceso inalámbrico a los miembros de un grupo de Windows, puedes crear una directiva de red que especifica el grupo, el método de conexión inalámbrica y que tiene una directiva de configuración de denegar el acceso de tipo.

- Determinar si se desea NPS pasar por alto las propiedades de marcado de cuentas de usuario que son miembros del grupo en el que se basa la directiva. Cuando no se habilita esta configuración, las propiedades de marcado de cuentas de usuario invalidación la configuración de directivas de red. Por ejemplo, si se configura una directiva de red que concede acceso a un usuario, pero se establecen las propiedades de marcado de la cuenta de usuario para ese usuario para denegar el acceso, el usuario se deniega el acceso. Sin embargo, si habilitas las propiedades de configuración telefónica de cuenta de usuario de omitir del tipo de directiva, el mismo usuario se concede acceso a la red.

- Determinar si la directiva usa la configuración de directiva de origen. Esta configuración permite fácilmente especificar un origen para todas las solicitudes de acceso. Orígenes posibles son una puerta de enlace de servicios de Terminal (TS Gateway), un servidor de acceso remoto (telefónico o VPN), un servidor DHCP, un punto de acceso inalámbrico y un servidor de la entidad de registro de estado. Como alternativa, puedes especificar un origen de específicos del proveedor.

- Determinar las condiciones que deben cumplirse en orden para que se aplica la directiva de red.

- Determinar la configuración que se aplica si se cumplen las condiciones de la directiva de red por la solicitud de conexión.

- Determinar si desea usar, modificar o eliminar las directivas de red de forma predeterminada.

## <a name="plan-nps-accounting"></a>Planear la administración de cuentas de NPS

NPS proporciona la capacidad para registrar los datos de cuentas de RADIUS, como la autenticación de usuario y solicitudes de cuentas, en tres formatos: formato IAS, formato compatible con la base de datos y el registro de Microsoft SQL Server. 

Formato de IAS y base de datos compatible crean archivos de registro en el servidor NPS local en formato de archivo de texto. 

Registro de SQL Server proporciona la capacidad de iniciar la sesión SQL Server 2000 o base de datos compatible con SQL Server 2005 XML, ampliación de cuentas de RADIUS para aprovechar las ventajas de inicio de sesión en una base de datos relacional.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para las cuentas de NPS, puedes usar los siguientes pasos.

- Determinar si desea almacenar datos de cuentas de NPS en archivos de registro o en una base de datos de SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilidad NPS con archivos de registro local

Solicitudes de autenticación y cuentas de usuario de grabación en archivos de registro se usa principalmente para fines de análisis y la facturación de conexión y también es útiles como una herramienta de investigación de seguridad, proporcionar un método para realizar el seguimiento de la actividad de un usuario malintencionado después de un ataque.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para las cuentas de NPS con archivos de registro local, puedes usar los siguientes pasos.

- Determinar el formato de archivo de texto que quieras usar para los archivos de registro NPS.

- Elegir el tipo de información que quieres iniciar sesión. Puede iniciar solicitudes de cuentas, las solicitudes de autenticación y estado periódico.

- Determinar la ubicación de disco duro donde quieres almacenar los archivos de registro.

- Diseña la solución de copia de seguridad de archivos de registro. La ubicación de disco duro en las que almacenar los archivos de registro debe ser una ubicación que permite hacer copias de seguridad de los datos. Además, la ubicación de disco duro debe estar protegida mediante la configuración de la lista de control de acceso (ACL) de la carpeta donde se almacenan los archivos de registro.

- Determinar la frecuencia que desee crear nuevos archivos de registro. Si quieres que los archivos de registro se crean basándose en el tamaño del archivo, determinar el tamaño máximo permitido antes de que se crea un nuevo archivo de registro NPS.

- Determinar si se desea NPS para eliminar archivos de registro anteriores si el disco duro se quede sin espacio de almacenamiento.

- Determinar la aplicación o aplicaciones que quieras usar para ver los datos de cuentas y generar informes.

### <a name="nps-sql-server-logging"></a>Registro de NPS SQL Server

Se usa el registro de NPS SQL Server cuando necesitas información de estado de sesión, para fines de análisis de datos y creación de informes y centralizar y simplificar la administración de los datos de contabilidad.

NPS proporciona la capacidad de usar SQL Server registro hasta la autenticación de usuario del registro y cuentas de las solicitudes que reciben de uno o varios servidores de acceso de red con un origen de datos en un equipo que ejecute el \(MSDE 2000\) motor de escritorio de Microsoft SQL Server, o cualquier versión de SQL Server posterior a SQL Server 2000.

Datos de cuentas se pasan de NPS en formato XML a un procedimiento almacenado en la base de datos, que es compatible con ambas lenguaje de consulta estructurado \(SQL\) y \(SQLXML\) XML. Grabación de autenticación de usuario y solicitudes en una base de datos de SQL Server compatible con XML de cuentas permiten que varios servidores NPS tener un origen de datos.

### <a name="key-steps"></a>Pasos clave

Durante la planificación para las cuentas de NPS mediante el registro de NPS SQL Server, puedes usar los siguientes pasos.

- Determinar si puedes u otro miembro de tu organización tiene experiencia de desarrollo de bases de datos relacionales de SQL Server 2000 o SQL Server 2005 y comprender cómo usar estos productos para crear, modificar, administrar y administrar las bases de datos de SQL Server.

- Determinar si está instalado SQL Server en el servidor NPS o en un equipo remoto.

- Diseña el procedimiento que se usará en la base de datos de SQL Server para procesar los archivos XML entrantes que contienen datos de cuentas de NPS.

- Diseñar la estructura de replicación de base de datos de SQL Server y el flujo.

- Determinar la aplicación o aplicaciones que quieras usar para ver los datos de cuentas y generar informes.

- Va a utilizar los servidores de acceso de red que enviar el atributo de clase en todas las solicitudes de cuentas. El atributo de clase se envía al cliente RADIUS en un mensaje de aceptación de acceso y es útil para la correspondencia con las sesiones de autenticación de mensajes de solicitud de contabilidad. Si el servidor de acceso de red en los mensajes de la solicitud de contabilidad envía el atributo de clase, se puede usar para que coincida con los registros de autenticación y cuentas. La combinación de los atributos único--número de serie, hora de reinicio de servicio y la dirección del servidor debe ser un identificador único para cada autenticación que acepta el servidor.

- Va a utilizar los servidores de acceso de red que admiten cuentas temporales.

- Va a utilizar los servidores de acceso de red que envían mensajes y las cuentas desactivación.

- Planea usar servidores de acceso de red que admiten el almacenamiento y reenvío de datos de cuentas. Servidores de acceso de red que admiten esta característica pueden almacenar datos de cuentas cuando el servidor de acceso de red no puede comunicarse con el servidor NPS. Cuando el servidor NPS está disponible, el servidor de acceso de red reenvía los registros almacenados en el servidor NPS, proporcionar una mayor fiabilidad en la administración de cuentas en los servidores de acceso de red que no proporcionas esta característica.

- Planear siempre configurar el atributo de intervalo de provisionales de cuenta en las directivas de red. El atributo de intervalo de provisionales de cuenta establece el intervalo (en segundos) entre cada actualización intermedia que envía el servidor de acceso de red. Según RFC 2869, el valor del atributo de intervalo de provisionales de cuenta no debe ser inferior a 60 segundos o un minuto y no debe ser inferior a 600 segundos o 10 minutos. Para obtener más información, consulta RFC 2869, "RADIUS extensiones".

- Asegúrate de que el registro de estado periódico esté habilitado en los servidores NPS.

