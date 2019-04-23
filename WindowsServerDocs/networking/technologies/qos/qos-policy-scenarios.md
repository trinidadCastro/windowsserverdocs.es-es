---
title: Escenarios de directiva de QoS
description: Este tema proporcionan escenarios de directiva de calidad de servicio (QoS), que se muestran cómo usar la directiva de grupo para priorizar el tráfico de red de aplicaciones y servicios en Windows Server 2016 concretos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a24ce3213b1aa1c66a438d278f7499b6d53bf6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834356"
---
# <a name="qos-policy-scenarios"></a>Escenarios de directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para revisar los escenarios hipotéticos que muestran cómo, cuándo y por qué se debe usar la directiva de QoS.

Los dos escenarios en este tema son:

1. Priorizar el tráfico de red para una aplicación de línea de negocio
2. Priorizar el tráfico de red para una aplicación de servidor HTTP

>[!NOTE]
>Algunas secciones de este tema contienen pasos generales que puede seguir para realizar las acciones descritas. Para obtener más instrucciones sobre la administración de directivas de QoS, consulte [administrar Directiva de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Escenario 1: Priorizar el tráfico de red para una aplicación de línea de negocio

En este escenario, un departamento de TI tiene varios objetivos que pueden realizar mediante la directiva de QoS:

- Proporcionar un mejor rendimiento de red para la misión\-aplicaciones críticas.
- Proporcionar un mejor rendimiento de red para un conjunto de claves de los usuarios mientras están utilizando una aplicación específica.
- Asegúrese de que la empresa\-datos amplias aplicación de copia de seguridad no impiden la rendimiento de la red mediante el uso de demasiado ancho de banda al mismo tiempo.

El departamento de TI decide configurar la directiva de QoS para dar prioridad a aplicaciones específicas mediante el punto de código de servicio de diferenciación \(DSCP\) valores para clasificar el tráfico de red y para configurar sus enrutadores para proporcionar preferencial tratamiento para el tráfico mayor prioridad. 

>[!NOTE]
>Para obtener más información sobre DSCP, consulte la sección **definir QoS prioridad a través de unas diferenciados servicios de punto de código** en el tema [directiva de calidad de servicio (QoS)](qos-policy-top.md).

Además de los valores DSCP, las directivas de QoS pueden especificar una tasa de limitación. Limitación tiene el efecto de limitación de todo el tráfico saliente que coincide con la directiva de QoS a una determinada velocidad de envío.

### <a name="qos-policy-configuration"></a>Configuración de directiva de QoS

Con tres objetivos independientes para llevar a cabo, el Administrador de TI decide crear tres diferentes directivas de QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Directiva de QoS para los servidores de aplicaciones LOB

La misión de la primera\-crítico para la que el departamento de TI crea una directiva de QoS en la aplicación es una empresa\-planeamiento de recursos empresariales amplia \(ERP\) aplicación. La aplicación ERP se hospeda en varios equipos que ejecutan Windows Server 2016. En servicios de dominio de Active Directory, estos equipos son miembros de una unidad organizativa \(OU\) que se creó para la línea de negocio \(LOB\) servidores de aplicaciones. El cliente\-componente del lado de la aplicación ERP se instala en equipos que ejecutan Windows 10 y Windows 8.1.

En la directiva de grupo, un administrador de TI selecciona el objeto de directiva de grupo \(GPO\) al que se aplicará la directiva de QoS. Mediante el Asistente para directivas de QoS, el Administrador de TI crea una directiva de QoS que se llama a "servidor LOB directiva" que especifica un valor alto\-prioridad el valor de DSCP de 44 para todas las aplicaciones, cualquier dirección IP, TCP y UDP y número de puerto.

La directiva de QoS se aplica solo a los servidores LOB y vincule el GPO a la unidad organizativa que contiene solo estos servidores a través de la consola de administración de directivas de grupo \(GPMC\) herramienta. Esta directiva de línea de negocio inicial del servidor aplica el valor alto\-prioridad DSCP valor cada vez que el equipo envía el tráfico de red. Más adelante se puede editar esta directiva de QoS \(en la herramienta Editor de objetos de directiva de grupo\) para incluir los números de puerto de la aplicación ERP, lo que limita la directiva para que se aplican solo cuando se usa el número de puerto especificado.

#### <a name="qos-policy-for-the-finance-group"></a>Directiva de QoS para el grupo de finanzas

Aunque muchos grupos dentro de la empresa tener acceso a la aplicación ERP, el grupo de finanzas depende de esta aplicación cuando se trabaja con los clientes y requiere que el grupo de alto rendimiento constante de la aplicación.

Para asegurarse de que el grupo de finanzas puede admitir a sus clientes, la directiva QoS debe clasificar el tráfico de estos usuarios como de prioridad alta. Sin embargo, no debe aplicarse la directiva cuando los miembros del grupo de finanzas utilizan aplicaciones que no sean de la aplicación ERP. 

Por este motivo, el departamento de TI define una segunda directiva de QoS denominada "cliente LOB directiva" en la herramienta Editor de objetos de directiva de grupo que se aplica un valor DSCP de 60 cuando el grupo de usuarios de finanzas ejecuta la aplicación ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Directiva de QoS para una aplicación de copia de seguridad

Se está ejecutando una aplicación de copia de seguridad independiente en todos los equipos. Para asegurarse de que el tráfico de copia de seguridad de la aplicación no usa todos los recursos de red disponible, el departamento de TI crea una directiva de copia de seguridad de datos. Esta directiva de copia de seguridad especifica un valor DSCP de 1 según el nombre del archivo ejecutable para la aplicación de copia de seguridad, que es **backup.exe**. 

Una tercera GPO se crea e implementa para todos los equipos cliente en el dominio. Cada vez que la aplicación envía datos, se aplica el valor DSCP de prioridad baja, incluso si se origina desde los equipos del departamento de finanzas.
  
>[!NOTE]
>Envía el tráfico de red sin una directiva de QoS con un valor DSCP de 0.

### <a name="scenario-policies"></a>Directivas de escenario

En la tabla siguiente se resume las directivas de QoS para este escenario.
  
|Nombre de directiva|Valor de DSCP|Tasa de limitación|Aplicar a unidades organizativas|Descripción|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[No hay ninguna directiva]|0|Ninguno|[No hay implementación]|Tratamiento de mejor esfuerzo (valor predeterminado) para el tráfico sin clasificar.|  
|Datos de copia de seguridad|1|Ninguno|Todos los clientes|Se aplica un valor DSCP de prioridad baja para este volumen de datos.|  
|Servidor de LOB|44|Ninguno|Equipo unidad organizativa para servidores ERP|Se aplica DSCP de alta prioridad para el tráfico del servidor ERP|  
|Cliente de línea de negocio|60|Ninguno|Grupo de usuarios de finanzas|Se aplica DSCP de alta prioridad para el tráfico de cliente ERP|  

>[!NOTE]
>Los valores DSCP se representan en formato decimal.

Con las directivas de QoS definen y aplican mediante Directiva de grupo, el tráfico de red saliente recibe el valor DSCP especificado a la directiva. Los enrutadores, a continuación, proporcionan diferencial tratamiento basado en estos valores DSCP mediante la puesta en cola. De este departamento de TI, los enrutadores se configuran con cuatro colas: alta prioridad, prioridad intermedio, esfuerzo y prioridad baja.

Cuando llegue el tráfico en el enrutador con los valores DSCP de "servidor LOB directiva" y "cliente LOB directiva," los datos se colocan en colas de alta prioridad. El tráfico con un valor DSCP 0 recibe un nivel de esfuerzo de servicio. Los paquetes con un valor de DSCP de 1 (desde la aplicación de copia de seguridad) reciben un tratamiento de prioridad baja.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Requisitos previos para dar prioridad a una aplicación de línea de negocio

Para completar esta tarea, asegúrese de cumplir los requisitos siguientes:

- Los equipos implicados ejecutan QoS\-sistemas operativos compatibles.

- Los equipos implicados son miembros de un dominio de Active Directory Services \(AD DS\) dominio para que se puede configurar mediante la directiva de grupo.

- Redes TCP/IP se configuran con enrutadores configurados para DSCP \(RFC 2474\). Para obtener más información, consulte [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Se cumplen los requisitos de credenciales administrativas.

#### <a name="administrative-credentials"></a>Credenciales administrativas

Para completar esta tarea, debe ser capaz de crear e implementar objetos de directiva de grupo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurar el entorno de prueba para dar prioridad a una aplicación de línea de negocio

Para configurar el entorno de prueba, complete las tareas siguientes.

- Crear un dominio de AD DS con los clientes y los usuarios que se agrupan en unidades organizativas. Para obtener instrucciones sobre la implementación de AD DS, consulte el [Guía de red principal](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configure los enrutadores a GPS cola según los valores DSCP. Por ejemplo, el valor de DSCP 44 entra en una cola de "Platinum" y todos los demás son ponderados de justo en cola.

>[!NOTE]
> Puede ver los valores DSCP mediante capturas de red con herramientas como Monitor de red. Después de realizar una captura de red, puede observar el campo TOS en los datos capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Pasos para dar prioridad a una aplicación de línea de negocio

Para dar prioridad a una aplicación de línea de negocio, complete las tareas siguientes:

1. Crear y vincular un objeto de directiva de grupo \(GPO\) con una directiva de QoS.

2. Configure los enrutadores para tratar una línea de negocio de GPS aplicación (mediante el uso de puesta en cola) en función de los valores DSCP seleccionados. Los procedimientos de esta tarea variarán según el tipo de enrutadores que tiene.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Escenario 2: Priorizar el tráfico de red para una aplicación de servidor HTTP

En Windows Server 2016, QoS basada en directivas incluyen la característica directivas basadas en la dirección URL. Las directivas de dirección URL permiten administrar el ancho de banda para los servidores HTTP.

Muchas aplicaciones empresariales se desarrollan para y hospedadas en Internet Information Services \(IIS\) servidores web y las aplicaciones Web que se accede desde los exploradores en los equipos cliente.

En este escenario, se supone administra un conjunto de servidores IIS que vídeos de aprendizaje de host para todos los empleados de su organización. Su objetivo es asegurarse de que el tráfico desde estos servidores de vídeo no sobrecargar la red y asegúrese de que el tráfico de vídeo se diferencia del tráfico de voz y datos en la red. 

La tarea es similar a la tarea en el escenario 1. Se diseña y configurar las opciones de administración de tráfico, como el valor DSCP para el tráfico de vídeo, y la limitación de velocidad igual como lo haría para las aplicaciones de línea de negocio. Pero al especificar el tráfico, en lugar de proporcionar el nombre de la aplicación, sólo escriba la dirección URL a los que responderá la aplicación de servidor HTTP: por ejemplo, https://hrweb/training.
  
> [!NOTE]
>No puede usar las directivas de QoS basada en URL para priorizar el tráfico de red para los equipos que ejecutan sistemas operativos de Windows que se publicaron antes de Windows 7 y Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Reglas de prioridad para las directivas basadas en la dirección URL

Todas las direcciones URL siguientes son válidos y pueden ser especificado en la directiva de QoS y se aplican simultáneamente a un equipo o un usuario:

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

¿Pero cuál de ellos recibirá prioridad? Las reglas son simples. Las directivas basadas en URL tienen mayor prioridad en un orden de lectura de izquierda a derecha. Por lo tanto, desde la prioridad más alta a la prioridad más baja, los campos de dirección URL son:
  
[1. Esquema de dirección URL](#bkmk_QoS_UrlScheme)

[2. Host de URL](#bkmk_QoS_UrlHost)

[3. Puerto de dirección URL](#bkmk_QoS_UrlPort)

[4. Ruta de acceso URL](#bkmk_QoS_UrlPath)

Los detalles son como sigue:

####  <a name="bkmk_QoS_UrlScheme"></a> 1. Esquema de dirección URL

 `https://` tiene una prioridad más alta que `https://`.

####  <a name="bkmk_QoS_UrlHost"></a> 2. Host de URL

 Desde la prioridad más alta a la más baja, son:

1. Nombre de host

2. Dirección IPv6

3. IPv4 address

4. Comodín

En el caso de nombre de host, un nombre de host con elementos con más puntos (más profundidad) tiene una prioridad mayor que un nombre de host con menos elementos con puntos. Por ejemplo, entre los nombres de host siguientes:

- video.Internal.Training.hr.mycompany.com (profundidad = 6)
  
- selfguide.training.mycompany.com (depth = 4)
  
- entrenamiento (profundidad = 1)
  
- biblioteca (profundidad = 1)
  
 **video.Internal.Training.hr.mycompany.com** tiene la prioridad más alta, y **selfguide.training.mycompany.com** tiene la siguiente prioridad más alta. **Entrenamiento** y **biblioteca** comparten la misma prioridad más baja.  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3. Puerto de dirección URL

Un objeto específico o un número de puerto implícita tiene una prioridad mayor que un puerto de carácter comodín.

####  <a name="bkmk_QoS_UrlPath"></a> 4. Ruta de acceso URL

Al igual que un nombre de host, una ruta de acceso de dirección URL puede constar de varios elementos. Tiene más elementos siempre tiene una prioridad mayor que el otro con el menor. Por ejemplo, las rutas de acceso siguientes se enumeran por prioridad:  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Si un usuario decide incluir todos los subdirectorios y archivos que siguen una ruta de acceso de dirección URL, esta ruta de acceso de dirección URL tendrá una prioridad más baja que tendría si la opción no se han realizado.

Un usuario también puede especificar una dirección IP de destino en una directiva basada en la dirección URL. La dirección IP de destino tiene una prioridad inferior a cualquiera de los cuatro campos de dirección URL que se ha descrito anteriormente.
  
### <a name="quintuple-policy"></a>Directiva quíntuplo

Se especifica una directiva quíntuplo por Id. de protocolo, dirección IP de origen, puerto de origen, dirección IP de destino y puerto de destino. Una directiva quíntuplo siempre tiene una prioridad mayor que cualquier directiva basada en la dirección URL. 

Si ya se ha aplicado una directiva quíntuplo para un usuario, una nueva directiva basada en dirección URL no producirá los conflictos en cualquiera de los clientes de ese usuario equipos.

El siguiente tema en esta guía, consulte [administrar Directiva de QoS](qos-policy-manage.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).
