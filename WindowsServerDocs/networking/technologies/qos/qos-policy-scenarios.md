---
title: Escenarios de directivas QoS
description: En este tema se proporcionan escenarios de directiva de calidad de servicio (QoS), que muestran cómo usar directiva de grupo para priorizar el tráfico de red de aplicaciones y servicios específicos en Windows Server 2016.
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: f1a3b76c0939e0b6dbe445799bdc9c96c4f16f1f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950391"
---
# <a name="qos-policy-scenarios"></a>Escenarios de directivas QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para revisar escenarios hipotéticos que muestran cómo, Cuándo y por qué usar la Directiva de QoS.

Los dos escenarios de este tema son:

1. Priorizar el tráfico de red para una aplicación de línea de negocio
2. Priorizar el tráfico de red para una aplicación de servidor HTTP

>[!NOTE]
>En algunas secciones de este tema se incluyen los pasos generales que puede seguir para realizar las acciones descritas. Para obtener instrucciones más detalladas sobre la administración de la directiva QoS, consulte [Administración de directivas QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Escenario 1: priorizar el tráfico de red para una aplicación de línea de negocio

En este escenario, un departamento de TI tiene varios objetivos que pueden lograr mediante el uso de la Directiva de QoS:

- Ofrezca un mejor rendimiento de red para \- las aplicaciones críticas.
- Proporcionar un mejor rendimiento de red para un conjunto de claves de usuarios mientras usan una aplicación específica.
- Asegúrese de que la \- aplicación de copia de seguridad de datos de toda la compañía no impide el rendimiento de la red al usar demasiado ancho de banda al mismo tiempo.

El Departamento de ti decide configurar la Directiva de QoS para priorizar aplicaciones específicas mediante el uso de valores de DSCP de punto de código de servicio de diferenciación \( \) para clasificar el tráfico de red y configurar sus enrutadores para proporcionar el tratamiento preferencial para el tráfico de mayor prioridad.

>[!NOTE]
>Para obtener más información sobre DSCP, consulte la sección **definir la prioridad de QoS a través de un punto de código servicios diferenciados** en la [Directiva calidad de servicio (QoS)](qos-policy-top.md).

Además de los valores de DSCP, las directivas de QoS pueden especificar una velocidad de limitación. La limitación tiene el efecto de limitar todo el tráfico saliente que coincida con la Directiva de QoS a una tasa de envío específica.

### <a name="qos-policy-configuration"></a>Configuración de directiva de QoS

Con tres objetivos independientes para lograr, el administrador de ti decide crear tres directivas de QoS diferentes.

#### <a name="qos-policy-for-lob-app-servers"></a>Directiva QoS para servidores de aplicaciones LOB

La primera \- aplicación crítica para la que el Departamento de ti crea una directiva de QoS es una \- aplicación ERP de planeamiento de recursos empresariales de toda la empresa \( \) . La aplicación ERP se hospeda en varios equipos que ejecutan Windows Server 2016. En Active Directory Domain Services, estos equipos son miembros de la unidad organizativa de unidades organizativas \( \) que se creó para \( los servidores de aplicaciones LOB de línea de negocio \)  . El \- componente del lado cliente de la aplicación ERP se instala en los equipos que ejecutan Windows 10 y Windows 8.1.

En directiva de grupo, un administrador de ti selecciona el GPO de objeto directiva de grupo en \( \) el que se aplicará la directiva QoS. Mediante el Asistente para directivas de QoS, el administrador de ti crea una directiva de QoS denominada "Directiva de LOB del servidor" que especifica un \- valor de DSCP de alta prioridad de 44 para todas las aplicaciones, cualquier dirección IP, TCP, UDP y número de puerto.

La Directiva de QoS se aplica solo a los servidores LOB vinculando el GPO a la unidad organizativa que solo contiene estos servidores, a través de la \( herramienta consola de administración de directivas de grupo GPMC \) . Esta directiva de LOB del servidor inicial aplica el valor de DSCP de alta \- prioridad siempre que el equipo envía tráfico de red. Esta directiva de QoS se puede editar posteriormente \( en la herramienta editor de objetos de directiva de grupo \) para incluir los números de puerto de la aplicación ERP, lo que limita la Directiva que se va a aplicar solo cuando se usa el número de puerto especificado.

#### <a name="qos-policy-for-the-finance-group"></a>Directiva QoS para el grupo Finance

Aunque muchos grupos dentro de la empresa acceden a la aplicación ERP, el grupo Finance depende de esta aplicación cuando se trabaja con los clientes y el grupo requiere un rendimiento de alto nivel de la aplicación de forma coherente.

Para asegurarse de que el grupo Finance puede admitir sus clientes, la directiva QoS debe clasificar el tráfico de estos usuarios como de alta prioridad. Sin embargo, la Directiva no se debe aplicar cuando los miembros del grupo Finance usan aplicaciones distintas de la aplicación ERP.

Por este motivo, el Departamento de TI define una segunda directiva QoS denominada "Directiva de LOB de cliente" en la herramienta Editor de objetos de directiva de grupo que aplica un valor de DSCP de 60 cuando el grupo de usuarios de finanzas ejecuta la aplicación ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Directiva QoS para una aplicación de copia de seguridad

Se está ejecutando una aplicación de copia de seguridad independiente en todos los equipos. Para asegurarse de que el tráfico de la aplicación de copia de seguridad no usa todos los recursos de red disponibles, el Departamento de ti crea una directiva de datos de copia de seguridad. Esta directiva de copia de seguridad especifica un valor de DSCP de 1 basado en el nombre del archivo ejecutable de la aplicación de copia de seguridad, que es **backup.exe**.

Se crea un tercer GPO y se implementa para todos los equipos cliente del dominio. Cada vez que la aplicación de copia de seguridad envía datos, se aplica el valor de DSCP de prioridad baja, aunque se origine en los equipos del Departamento de finanzas.

>[!NOTE]
>El tráfico de red sin una directiva de QoS envía con un valor de DSCP de 0.

### <a name="scenario-policies"></a>Directivas de escenarios

En la tabla siguiente se resumen las directivas de QoS para este escenario.

|Nombre de la directiva|Valor de DSCP|Velocidad del acelerador|Aplicado a unidades organizativas|Descripción|
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Sin directiva]|0|None|[Sin implementación]|Tratamiento del mejor esfuerzo (valor predeterminado) para el tráfico sin clasificar.|
|Datos de copia de seguridad|1|None|Todos los clientes|Aplica un valor de DSCP de prioridad baja para estos datos masivos.|
|LOB de servidor|44|Ninguno|Unidad organizativa de equipos para servidores ERP|Aplica el DSCP de alta prioridad para el tráfico del servidor ERP|
|LOB de cliente|60|Ninguno|Grupo de usuarios de finanzas|Aplica DSCP de alta prioridad para el tráfico de cliente de ERP|

>[!NOTE]
>Los valores de DSCP se representan en formato decimal.

Con las directivas de QoS definidas y aplicadas mediante directiva de grupo, el tráfico de red saliente recibe el valor de DSCP especificado por la Directiva. Los enrutadores proporcionan el tratamiento diferencial basado en estos valores de DSCP mediante el uso de colas. Para este Departamento de ti, los enrutadores se configuran con cuatro colas: alta prioridad, prioridad media, mejor esfuerzo y prioridad baja.

Cuando el tráfico llega al enrutador con valores de DSCP de "Directiva de LOB del servidor" y "Directiva de LOB del cliente", los datos se colocan en colas de prioridad alta. El tráfico con un valor de DSCP de 0 recibe un nivel de servicio de mejor esfuerzo. Los paquetes con un valor de DSCP de 1 (desde la aplicación de copia de seguridad) reciben un tratamiento de baja prioridad.

### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Requisitos previos para la priorización de una aplicación de línea de negocio

Para completar esta tarea, asegúrese de que cumple los siguientes requisitos:

- Los equipos implicados ejecutan \- sistemas operativos compatibles con QoS.

- Los equipos implicados son miembros de un Active Directory Domain Services \( AD DS \) dominio para que se puedan configurar mediante Directiva de grupo.

- Las redes TCP/IP se configuran con los enrutadores configurados para DSCP \( RFC 2474 \) . Para obtener más información, consulte [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Se cumplen los requisitos de credenciales administrativas.

#### <a name="administrative-credentials"></a>Credenciales administrativas

Para completar esta tarea, debe poder crear e implementar directiva de grupo objetos.

#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurar el entorno de prueba para dar prioridad a una aplicación de línea de negocio

Para configurar el entorno de prueba, complete las siguientes tareas.

- Cree un dominio de AD DS con clientes y usuarios agrupados en unidades organizativas. Para obtener instrucciones sobre cómo implementar AD DS, consulte la [Guía de red principal](../../core-network-guide/core-network-guide.md).

- Configure los enrutadores para que se enruten de una cola diferencial según los valores de DSCP. Por ejemplo, el valor de DSCP 44 entra en una cola de "Platinum" y el resto se pondera en cola.

>[!NOTE]
> Puede ver los valores de DSCP mediante el uso de capturas de red con herramientas como Monitor de red. Después de realizar una captura de red, puede observar el campo TOS en los datos capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Pasos para dar prioridad a una aplicación de línea de negocio

Para dar prioridad a una aplicación de línea de negocio, complete las siguientes tareas:

1. Crear y vincular un GPO de objeto de directiva de grupo \( \) con una directiva de QoS.

2. Configure los enrutadores para que traten de forma diferencial una aplicación de línea de negocio (mediante el uso de colas) basándose en los valores de DSCP seleccionados. Los procedimientos de esta tarea variarán en función del tipo de enrutadores que tenga.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Escenario 2: priorizar el tráfico de red para una aplicación de servidor HTTP

En Windows Server 2016, QoS basada en Directiva incluye las directivas basadas en la dirección URL de la característica. Las directivas de dirección URL permiten administrar el ancho de banda de los servidores HTTP.

Muchas aplicaciones empresariales se desarrollan para y se hospedan en Internet Information Services \( \) servidores Web de IIS y se tiene acceso a las aplicaciones web desde exploradores en equipos cliente.

En este escenario, supongamos que administra un conjunto de servidores IIS que hospedan vídeos de aprendizaje para todos los empleados de la organización. El objetivo es asegurarse de que el tráfico procedente de estos servidores de vídeo no sobrecargue la red y de que el tráfico de vídeo se diferencia del tráfico de voz y de datos de la red.

La tarea es similar a la tarea en el escenario 1. Diseñará y configurará la configuración de administración del tráfico, como el valor de DSCP para el tráfico de vídeo, y la velocidad de limitación igual que haría para las aplicaciones de línea de negocio. Pero cuando se especifica el tráfico, en lugar de proporcionar el nombre de la aplicación, solo se especifica la dirección URL a la que responderá la aplicación de servidor HTTP: por ejemplo, https://hrweb/training .

> [!NOTE]
>No se pueden usar directivas QoS basadas en URL para priorizar el tráfico de red en equipos que ejecutan sistemas operativos Windows que se publicaron antes de Windows 7 y Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Reglas de prioridad para las directivas basadas en URL

Todas las direcciones URL siguientes son válidas y se pueden especificar en la directiva QoS y aplicarse simultáneamente a un equipo o un usuario:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech

- https://*/eBooks

¿Pero cuál recibirá la prioridad? Las reglas son sencillas. Las directivas basadas en URL se priorizan en un orden de lectura de izquierda a derecha. Por lo tanto, de la prioridad más alta a la más baja, los campos de dirección URL son:

[1. esquema de dirección URL](#bkmk_QoS_UrlScheme)

[2. host de dirección URL](#bkmk_QoS_UrlHost)

[3. Puerto de dirección URL](#bkmk_QoS_UrlPort)

[4. ruta de acceso URL](#bkmk_QoS_UrlPath)

Los detalles son los siguientes:

####  <a name="1-url-scheme"></a><a name="bkmk_QoS_UrlScheme"></a> 1. esquema de dirección URL

 `https://` tiene una prioridad mayor que `https://` .

####  <a name="2-url-host"></a><a name="bkmk_QoS_UrlHost"></a> 2. host de dirección URL

 De la prioridad más alta a la más baja, son:

1. Hostname

2. Dirección IPv6

3. Dirección IPv4

4. Wildcard (Carácter comodín)

En el caso del nombre de host, un nombre de host con más elementos de puntos (más profundidad) tiene una prioridad más alta que un nombre de host con menos elementos de puntos. Por ejemplo, entre los nombres de host siguientes:

- video.internal.training.hr.mycompany.com (profundidad = 6)

- selfguide.training.mycompany.com (profundidad = 4)

- entrenamiento (profundidad = 1)

- Biblioteca (profundidad = 1)

  **video.Internal.Training.hr.mycompany.com** tiene la prioridad más alta y **selfguide.Training.mycompany.com** tiene la siguiente prioridad más alta. El **entrenamiento** y el recurso compartido de **biblioteca** tienen la misma prioridad mínima.

####  <a name="3-url-port"></a><a name="bkmk_QoS_UrlPort"></a> 3. Puerto de dirección URL

Un número de puerto específico o implícito tiene una prioridad más alta que un puerto comodín.

####  <a name="4-url-path"></a><a name="bkmk_QoS_UrlPath"></a> 4. ruta de acceso URL

Al igual que un nombre de host, una ruta de dirección URL puede constar de varios elementos. El que tiene más elementos siempre tiene una prioridad más alta que la que tiene menos. Por ejemplo, las siguientes rutas de acceso se enumeran por prioridad:

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Si un usuario elige incluir todos los subdirectorios y archivos que siguen una ruta de dirección URL, esta ruta de acceso de dirección URL tendrá una prioridad inferior a la que tendría si no se hubiera elegido la opción.

Un usuario también puede elegir especificar una dirección IP de destino en una directiva basada en URL. La dirección IP de destino tiene una prioridad inferior a cualquiera de los cuatro campos de dirección URL descritos anteriormente.

### <a name="quintuple-policy"></a>Directiva quíntuplo

Una directiva quíntuplo se especifica mediante el identificador de protocolo, la dirección IP de origen, el puerto de origen, la dirección IP de destino y el puerto de destino. Una directiva quíntuplo siempre tiene una prioridad más alta que cualquier directiva basada en dirección URL.

Si ya se ha aplicado una directiva de quíntuplo para un usuario, una nueva directiva basada en URL no producirá conflictos en ninguno de los equipos cliente de ese usuario.

Para el siguiente tema de esta guía, consulte [Administración](qos-policy-manage.md)de la Directiva de QoS.

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).