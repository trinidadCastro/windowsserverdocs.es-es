---
title: Planear NPS como servidor RADIUS
description: En este tema se proporciona información acerca de la implementación de servidor RADIUS de servidor de directivas de red planeación en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5fd89ef8d95735b8cbe1334ba51ed0059595dbc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839176"
---
# <a name="plan-nps-as-a-radius-server"></a>Planear NPS como servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al implementar el servidor de directivas de red \(NPS\) como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación, autorización y contabilidad para las solicitudes de conexión para el dominio local y para dominios que confiar en el dominio local. Puede utilizar estas directrices de planeamiento para simplificar la implementación de RADIUS.

Estas instrucciones para la planeación no incluyen circunstancias en que desea implementar NPS como proxy RADIUS. Al implementar NPS como proxy RADIUS, NPS reenvía las solicitudes de conexión a un servidor que ejecuta NPS u otros servidores RADIUS en dominios remotos, dominios sin confianza o ambos. 

Antes de implementar NPS como servidor RADIUS en la red, utilice las siguientes directrices para planear la implementación.

- Planear la configuración de NPS.

- Planear a los clientes RADIUS.

- Planear el uso de métodos de autenticación.

- Planeación de las directivas de red.

- Plan de contabilidad de NPS.

## <a name="plan-nps-configuration"></a>Planear la configuración de NPS

Debe decidir en qué dominio, NPS es miembro. Para entornos de varios dominios, NPS puede autenticar las credenciales de cuentas de usuario en el dominio del que es miembro y para todos los dominios que confían en el dominio local de NPS. Para permitir que el NPS leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización, debe agregar la cuenta de equipo de NPS para el grupo de RAS y NPSs para cada dominio.

Después de determinar la pertenencia al dominio de NPS, el servidor debe configurarse para comunicarse con los clientes RADIUS, también denominados servidores de acceso de red, mediante el protocolo RADIUS. Además, puede configurar los tipos de eventos que registra NPS en el registro y se puede escribir una descripción para el servidor de eventos.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de la configuración de NPS, puede usar los pasos siguientes.

- Determinar los puertos RADIUS que usa NPS para recibir mensajes RADIUS de los clientes RADIUS. Los puertos predeterminados son los puertos UDP 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos 1813 y 1646 para los mensajes de cuentas RADIUS.

- Si el NPS se configura con varios adaptadores de red, determinar los adaptadores a través de la que desea que se permite el tráfico RADIUS.

- Determinar los tipos de eventos que desea que NPS registre en el registro de eventos. Puede registrar las solicitudes de autenticación rechazados, solicitudes de autenticación correctas o ambos tipos de solicitudes.

- Determine si va a implementar más de un servidor NPS. Para proporcionar tolerancia a errores para la autenticación basada en RADIUS y cuentas, use al menos dos NPSs. Se usa un NPS como servidor RADIUS principal y la otra se usa como una copia de seguridad. Cada cliente RADIUS, a continuación, se configura en ambos NPSs. Si el NPS principal deja de estar disponible, los clientes RADIUS, envían mensajes de solicitud de acceso a NPS alternativo.

- Planee la secuencia de comandos que se usa para copiar una configuración de NPS en otros NPSs para guardar en la sobrecarga administrativa y evitar que la configuración incorrecta de un servidor. NPS proporciona los comandos Netsh que le permitan copiar todo o parte de una configuración de NPS para importarlos en otro NPS. Puede ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guarda la secuencia de comandos como una secuencia de comandos, puede ejecutar el script en una fecha posterior si decide cambiar las configuraciones de servidor.

## <a name="plan-radius-clients"></a>Planear a los clientes RADIUS

Los clientes RADIUS son servidores de acceso de red, como puntos de acceso inalámbricos, servidores de red privada virtual (VPN), 802.1X conmutadores compatibles con X y los servidores de acceso telefónico. Proxy RADIUS que reenvían los mensajes de solicitud a servidores RADIUS de conexión, también son clientes RADIUS. NPS es compatible con todos los servidores de acceso de red y proxy RADIUS que cumplen con el radio del protocolo como se describe en RFC 2865, "Servicio autenticación remota telefónica de usuario (RADIUS)," y RFC 2866, "Contabilidad de RADIUS".

>[!IMPORTANT]
>Los clientes de acceso, como los equipos cliente, no son clientes RADIUS. Solo los servidores de acceso de red y servidores proxy que admiten el protocolo RADIUS son clientes RADIUS.

Además, los puntos de acceso inalámbrico y conmutadores deben ser capaces de autenticación 802.1X. Si desea implementar el protocolo de autenticación Extensible \(EAP\) o protocolo de autenticación Extensible protegido \(PEAP\), conmutadores y puntos de acceso deben admitir el uso de EAP.

Para probar la interoperabilidad básica para las conexiones PPP para puntos de acceso inalámbrico, configure el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Usar protocolos de autenticación adicionales basadas en PPP, como PEAP, hasta que haya probado los que se va a utilizar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de clientes RADIUS, puede usar los pasos siguientes.

- Documente los atributos específicos del proveedor (VSA), debe configurar en NPS. Si los servidores de acceso de red requieren VSA, registrar la información de VSA para su uso posterior cuando configura las directivas de red en NPS. 

- Documente las direcciones IP de clientes RADIUS y el NPS para simplificar la configuración de todos los dispositivos. Al implementar los clientes RADIUS, debe configurar para que usen el protocolo RADIUS, con la dirección IP de NPS especificada como el servidor de autenticación. Y cuando se configura NPS para comunicarse con los clientes RADIUS, debe especificar las direcciones IP del cliente RADIUS en el complemento NPS.

- Cree los secretos compartidos para la configuración de los clientes RADIUS y en el complemento NPS. Debe configurar a clientes RADIUS con un secreto compartido, o la contraseña, que también va a escribir en el complemento NPS al configurar clientes RADIUS en NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planear el uso de métodos de autenticación

NPS es compatible con ambos métodos de autenticación basada en contraseña y basada en certificados. Sin embargo, no todos los servidores de acceso de red admiten los mismos métodos de autenticación. En algunos casos, es posible que desea implementar un método de autenticación diferente según el tipo de acceso de red.

Por ejemplo, es posible que desee implementar inalámbrico y el acceso VPN para su organización, pero use un método de autenticación diferente para cada tipo de acceso: EAP-TLS para las conexiones VPN, debido a la seguridad sólida que proporciona EAP con seguridad de la capa de transporte (EAP-TLS) y PEAP-MS-CHAP v2 para conexiones inalámbricas 802.1X.

PEAP con protocolo de autenticación de Microsoft desafío mutuo versión 2 (PEAP-MS-CHAP v2) proporciona una característica denominada fast volver a conectar que está diseñado específicamente para su uso con equipos portátiles y otros dispositivos inalámbricos. Reconexión rápida permite a los clientes inalámbricos moverse entre puntos de acceso inalámbrico en la misma red sin que se va a volver a autenticarse cada vez que se asocian a un nuevo punto de acceso. Esto proporciona una mejor experiencia para usuarios inalámbricos y les permite para moverse entre puntos de acceso sin tener que volver a escribir sus credenciales.
Debido a fast volver a conectarse y la seguridad que proporciona PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 es una elección lógica como método de autenticación para conexiones inalámbricas.

Para las conexiones VPN, EAP-TLS es un método de autenticación basada en certificados que proporciona seguridad sólida que protege el tráfico de red, incluso cuando se transmiten a través de Internet desde los equipos móviles o de inicio para los servidores VPN de la organización.

### <a name="certificate-based-authentication-methods"></a>Métodos de autenticación basada en certificados

Métodos de autenticación basada en certificados tienen la ventaja de proporcionar seguridad sólida; y tienen la desventaja de ser más difícil de implementar que los métodos de autenticación basado en contraseña.

PEAP-MS-CHAP v2 y EAP-TLS son métodos de autenticación basada en certificados, pero existen muchas diferencias entre ellas y la manera en que se implementan.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS usa certificados para la autenticación de cliente y servidor y requiere que se implemente una infraestructura de clave pública (PKI) de su organización. Implementar una PKI puede ser compleja y requiere una fase de planeación que es independiente de la planeación para el uso de NPS como servidor RADIUS.

Con EAP-TLS, el NPS inscribe un certificado de servidor de una entidad de certificación \(CA\), y el certificado se guarda en el equipo local en el almacén de certificados. Durante el proceso de autenticación, autenticación de servidor se produce cuando el NPS envía su certificado de servidor al cliente de acceso para demostrar su identidad al cliente de acceso. El cliente de acceso examina diversas propiedades del certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos de certificado de servidor mínima y emitido por una CA que confía en el cliente de acceso, NPS se autentica correctamente con el cliente.

De forma similar, autenticación de cliente se produce durante el proceso de autenticación cuando el cliente envía su certificado de cliente a NPS para demostrar su identidad a NPS. NPS examina el certificado y, si el certificado de cliente cumple los requisitos de certificado de cliente mínima y emitido por una CA que confía en el NPS, NPS autentica correctamente el cliente de acceso.

Aunque es necesario que el certificado de servidor se almacena en el almacén de certificados en el NPS, se puede almacenar el certificado de cliente o usuario en el almacén de certificados en el cliente o en una tarjeta inteligente.

Este proceso de autenticación se realice correctamente, se requiere que todos los equipos tienen el certificado de entidad de certificación de su organización en el almacén de certificados entidades emisoras raíz de confianza para el equipo Local y el usuario actual.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 usa un certificado para la autenticación de servidor y credenciales basadas en contraseña para la autenticación de usuario. Dado que los certificados se usan solo para la autenticación de servidor, no son necesarios para implementar una PKI para poder usar PEAP-MS-CHAP v2. Al implementar PEAP-MS-CHAP v2, puede obtener un certificado de servidor de NPS en una de las dos maneras siguientes:

- Puede instalar servicios de certificados de Active Directory (AD CS) y, a continuación, inscribir automáticamente certificados en NPSs. Si utiliza este método, también debe inscribir el certificado de CA a equipos cliente se conectan a la red para que confíen en el certificado emitido a NPS.

- Puede adquirir un certificado de servidor de una CA pública, como VeriSign. Si utiliza este método, asegúrese de que seleccionar una entidad de certificación que ya sea de confianza para los equipos cliente. Para determinar si los equipos cliente confiarán en una entidad de certificación, abra el complemento Microsoft Management Console (MMC) de certificados en un equipo cliente y, a continuación, ver el almacén entidades emisoras raíz de confianza para el equipo Local y para el usuario actual. Si hay un certificado de la CA en estos almacenes de certificados, el equipo cliente confía en la CA y, por tanto, confíe en cualquier certificado emitido por la entidad de certificación.

Durante el proceso de autenticación con PEAP-MS-CHAP v2, autenticación de servidor se produce cuando el NPS envía su certificado de servidor en el equipo cliente. El cliente de acceso examina diversas propiedades del certificado para determinar si el certificado es válido y es adecuado para su uso durante la autenticación de servidor. Si el certificado de servidor cumple los requisitos de certificado de servidor mínima y emitido por una CA que confía en el cliente de acceso, NPS se autentica correctamente con el cliente.

Autenticación de usuario se produce cuando un usuario intenta conectarse a las credenciales de red tipos basado en contraseña y se intenta iniciar sesión. NPS recibe las credenciales y realiza la autenticación y autorización. Si el usuario está autenticado y autorizado correctamente y si el equipo cliente autentica correctamente el NPS, se concede la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Al planear el uso de métodos de autenticación, puede usar los pasos siguientes.

- Identifique los tipos de acceso de red que se va a ofrecer, como inalámbricas, 802.1X, VPN, conmutador compatible con X y acceso telefónico.

- Determinar el método de autenticación o los métodos que se va a usar para cada tipo de acceso. Se recomienda que utilice los métodos de autenticación basada en certificados que proporcionan una gran seguridad; Sin embargo, no sería práctico para implementar una PKI, por lo que otros métodos de autenticación pueden proporcionar un mejor equilibrio de lo que necesita para la red.

- Si va a implementar que EAP-TLS, planee la implementación de PKI. Esto incluye la planificación de las plantillas de certificado que se va a usar para los certificados de servidor y certificados de equipo cliente. También incluye la determinación de cómo inscribir certificados para el miembro de dominio y equipos miembro que no sea de dominio y determinar si desea usar tarjetas inteligentes.

- Si implementas PEAP-MS-CHAP v2, determine si desea instalar AD CS para emitir certificados de servidor a sus NPSs o si desea adquirir certificados de servidor de una CA pública, como VeriSign.

### <a name="plan-network-policies"></a>Planeación de las directivas de red

Se usan las directivas de red NPS para determinar si están autorizadas las solicitudes de conexión procedentes de clientes RADIUS. NPS también usa las propiedades de acceso telefónico de la cuenta de usuario para tomar una decisión de autorización.

Dado que las directivas de red se procesan en el orden en que aparecen en el complemento NPS, planea colocar las directivas más restrictivas en primer lugar en la lista de directivas. Para cada solicitud de conexión, NPS intenta hacer coincidir las condiciones de la directiva con las propiedades de la solicitud de conexión. NPS examina cada directiva de red en orden hasta que encuentra a una coincidencia. Si no encuentra a una coincidencia, se rechaza la solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de directivas de red, puede usar los pasos siguientes.

- Determinar el orden de procesamiento de NPS preferido de las directivas de red, desde el más restrictivo al menos restrictivo.

- Determinar el estado de la directiva. El estado de la directiva puede tener el valor de habilitado o deshabilitado. Si está habilitada la directiva, NPS evalúa la directiva al realizar la autorización. Si no está habilitada la directiva, no se evalúa.

- Determinar el tipo de directiva. Debe determinar si la directiva está diseñada para conceder acceso cuando se cumplan las condiciones de la directiva de la solicitud de conexión o si la directiva está diseñada para denegar el acceso cuando se cumplan las condiciones de la directiva de la solicitud de conexión. Por ejemplo, si desea denegar explícitamente el acceso inalámbrico a los miembros de un grupo de Windows, puede crear una directiva de red que especifica el grupo, el método de conexión inalámbrica y que tiene una directiva de un tipo de configuración de denegar el acceso.

- Determine si desea que NPS para que omita las propiedades de marcado de cuentas de usuario que son miembros del grupo en el que se basa la directiva. Cuando esta opción no está habilitada, las propiedades de marcado de cuentas de usuario reemplazan valores configurados en las directivas de red. Por ejemplo, si se configura una directiva de red que se concede acceso a un usuario, pero las propiedades de acceso telefónico de la cuenta de usuario para que el usuario se configuran para denegar el acceso, el usuario se deniega el acceso. Pero si habilita las propiedades de configuración de acceso telefónico de cuenta de usuario de omitir del tipo de directiva, el mismo usuario se concede acceso a la red.

- Determine si la directiva utiliza la configuración de directiva de origen. Esta configuración le permite especificar fácilmente una fuente para todas las solicitudes de acceso. Posibles orígenes son una puerta de enlace de Terminal Services (puerta de enlace de TS), un servidor de acceso remoto (VPN o telefónico), un servidor DHCP, un punto de acceso inalámbrico y un servidor de autoridad de registro de mantenimiento. Como alternativa, puede especificar un origen específico del proveedor.

- Determinar las condiciones que deben cumplirse en orden para que se aplicará la directiva de red.

- Determine la configuración que se aplica si se cumplen las condiciones de la directiva de red por la solicitud de conexión.

- Determine si desea utilizar, modificar o eliminar las directivas de red predeterminada.

## <a name="plan-nps-accounting"></a>Plan de contabilidad de NPS

NPS proporciona la capacidad de registrar los datos de cuentas de RADIUS, como la autenticación de usuario y las solicitudes de cuentas, en tres formatos: Formato IAS, formato compatible con la base de datos y registro de Microsoft SQL Server. 

Formato IAS y formato compatible con la base de datos crean archivos de registro en el NPS local en formato de archivo de texto. 

Registro de SQL Server proporciona la capacidad de iniciar en un SQL Server 2000 o una base de datos compatibles con SQL Server 2005 XML, extender las cuentas RADIUS para aprovechar las ventajas del registro en una base de datos relacional.

### <a name="key-steps"></a>Pasos clave

Durante la planificación para la contabilidad de NPS, puede usar los pasos siguientes.

- Determine si desea almacenar los datos de cuentas NPS en los archivos de registro o en una base de datos de SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Contabilidad de NPS mediante archivos de registro local

Grabación de autenticación de usuarios y solicitudes en los archivos de registro de cuentas se usa principalmente para fines de facturación y análisis de la conexión y también es útil como herramienta de investigación de seguridad, proporcionar un método para realizar el seguimiento de la actividad de un usuario malintencionado después de un ataque.

### <a name="key-steps"></a>Pasos clave

Durante la planificación para la contabilidad de NPS mediante archivos de registro local, puede usar los pasos siguientes.

- Determinar el formato de archivo de texto que desea usar para los archivos de registro NPS.

- Elija el tipo de información que desea registrar. Puede registrar las solicitudes de cuentas, las solicitudes de autenticación y estado periódico.

- Determinar la ubicación de disco duro donde desea almacenar los archivos de registro.

- Diseñe su solución de copia de seguridad de archivos de registro. La ubicación del disco duro donde se almacenan los archivos de registro debe ser una ubicación que permite realizar fácilmente una copia de seguridad de los datos. Además, la ubicación del disco duro debe protegerse mediante la configuración de la lista de control de acceso (ACL) para la carpeta donde se almacenan los archivos de registro.

- Determine la frecuencia a la que desea crear nuevos archivos de registro. Si desea que los archivos de registro se crean según el tamaño del archivo, determinar el tamaño de archivo máximo permitido antes de que se crea un nuevo archivo de registro NPS.

- Determine si desea que NPS para eliminar los archivos de registro antiguos si el disco duro se queda sin espacio de almacenamiento.

- Determine la aplicación o aplicaciones que desea usar para ver los datos de cuentas y producir informes.

### <a name="nps-sql-server-logging"></a>Registro del servidor SQL de NPS

Registro del servidor SQL de NPS se usa cuando se necesita información de estado de sesión, con fines de análisis de datos, y creación de informes y para centralizar y simplificar la administración de los datos de cuentas.

NPS ofrece la posibilidad de usar SQL Server, inicio de sesión autenticación de usuario del registro y teniendo en cuenta las solicitudes recibidas desde uno o varios servidores de acceso de red a un origen de datos en un equipo que ejecuta Microsoft SQL Server Desktop Engine \(MSDE 2000\), o cualquier versión de SQL Server posterior a SQL Server 2000.

Se pasan los datos de cuentas de NPS en formato XML a un procedimiento almacenado en la base de datos, que es compatible con ambos lenguaje de consulta estructurado \(SQL\) y XML \(SQLXML\). Grabación de autenticación de usuario y teniendo en cuenta las solicitudes en una base de datos de SQL Server compatible con XML permiten varios NPSs tener un origen de datos.

### <a name="key-steps"></a>Pasos clave

Durante la planificación para la contabilidad de NPS mediante el registro de SQL Server de NPS, puede usar los pasos siguientes.

- Determinar si usted o a otro miembro de su organización tiene experiencia de desarrollo de base de datos relacional de SQL Server 2000 o SQL Server 2005 y comprender cómo usar estos productos para crear, modificar, administrar y administrar bases de datos de SQL Server.

- Determinar si SQL Server está instalado en el NPS o en un equipo remoto.

- Diseñe el procedimiento almacenado que se usará en la base de datos de SQL Server para procesar archivos XML entrantes que contienen datos de contabilidad de NPS.

- Diseñar la estructura de la replicación de base de datos de SQL Server y el flujo.

- Determine la aplicación o aplicaciones que desea usar para ver los datos de cuentas y producir informes.

- Plan para usar servidores de acceso de red que enviar el atributo de clase en todas las solicitudes de cuentas. El atributo de clase se envía al cliente RADIUS en un mensaje de aceptación de acceso y es útil para correlacionar los mensajes de solicitud de cuentas con las sesiones de autenticación. Si el atributo de clase se envía mediante el servidor de acceso de red en los mensajes de solicitud de contabilidad, se puede usar para que coincida con los registros de autenticación y cuentas. La combinación de los atributos Unique-: número de serie, hora de reinicio de servicio y dirección del servidor debe ser una identificación única para cada autenticación que acepta el servidor.

- Considere usar servidores de acceso de red que admitan cuentas temporales.

- Considere usar servidores de acceso de red que envían mensajes y las cuentas o desactivadas.

- Considere usar servidores de acceso de red que admiten el almacenamiento y reenvío de datos de cuentas. Servidores de acceso de red que admitan esta característica pueden almacenar los datos de cuentas cuando el servidor de acceso de red no puede comunicarse con el NPS. Cuando NPS está disponible, el servidor de acceso de red reenvía los registros almacenados en NPS, que proporciona una mayor confiabilidad en administración de cuentas a través de servidores de acceso de red que no proporcionen esta característica.

- Plan para configurar siempre el atributo Acct-Interim-Interval en las directivas de red. El atributo Acct-Interim-Interval establece el intervalo (en segundos) entre cada actualización intermedia que envía el servidor de acceso de red. Según RFC 2869, el valor del atributo Acct-Interim-Interval no debe ser menor que 60 segundos o de un minuto y no debe ser inferior a 600 segundos o 10 minutos. Para obtener más información, consulte RFC 2869, "Extensiones RADIUS".

- Asegúrese de que está habilitado el registro de estado periódico en sus NPSs.

