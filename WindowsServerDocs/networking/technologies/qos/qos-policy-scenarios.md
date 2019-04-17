---
title: Escenarios de la directiva de QoS
description: Este tema proporciona escenarios de la directiva de calidad de servicio (QoS), que muestran cómo usar la directiva de grupo para priorizar el tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>Escenarios de la directiva de QoS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para revisar hipotéticos escenarios que muestran cómo, cuándo y por qué se debe usar la directiva de QoS.

Los dos escenarios de este tema son:

1. Establecer prioridades para el tráfico de red para una aplicación de línea de negocio
2. Establecer prioridades para el tráfico de red para una aplicación de servidor HTTP

>[!NOTE]
>Algunas secciones de este tema contienen los pasos generales que puede seguir para realizar las acciones descritas. Para obtener más instrucciones acerca de cómo administrar la directiva de QoS, consulta [administrar la directiva de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Escenario 1: Establecer prioridades para el tráfico de red para una aplicación de línea de negocio

En este escenario, un departamento de TI tiene varios objetivos que puede realizar mediante la directiva de QoS:

- Proporciona un mejor rendimiento de red para aplicaciones críticas mission\.
- Proporciona un mejor rendimiento de red para un conjunto de claves de los usuarios mientras están usando una aplicación específica.
- Asegúrate de que la aplicación de la copia de seguridad de datos de todo el company\ no dificultar el rendimiento de red usando demasiado ancho de banda al mismo tiempo.

Decide el departamento de TI configurar la directiva de QoS para dar prioridad a aplicaciones específicas mediante el uso de valores de puntos de código de servicio de diferenciación \(DSCP\) para clasificar el tráfico de red y para configurar sus enrutadores para proporcionar preferencia para el tráfico mayor prioridad. 

>[!NOTE]
>Para obtener más información sobre DSCP, consulta la sección **definir QoS prioridad a través de un diferenciadas servicios puntos de código** en el tema [directiva de calidad de servicio (QoS)](qos-policy-top.md).

Además de los valores DSCP, las directivas de QoS pueden especificar una velocidad de aceleración. Limitación tiene el efecto de limitar todo el tráfico saliente que coincide con la directiva de QoS a un determinado envíe velocidad.

### <a name="qos-policy-configuration"></a>Configuración de directiva de QoS

Con tres objetivos independientes para realizar, el Administrador de TI decide crear tres diferentes directivas QoS.

#### <a name="qos-policy-for-lob-app-servers"></a>Directiva de QoS para servidores de la aplicación LOB

La primera aplicación mission\ críticos para el que el departamento de TI crea una directiva de QoS es un recurso de empresa de todo el company\ planear \(ERP\) aplicación. La aplicación ERP está hospedada en varios equipos que ejecutan Windows Server 2016. En servicios de dominio de Active Directory, estos equipos son miembros de una unidad organizativa \(OU\) que se creó para servidores de aplicaciones de línea de negocio \(LOB\). El componente de la aplicación ERP client\ está instalado en equipos que ejecutan Windows 10 y Windows 8.1.

En la directiva de grupo, un administrador de TI selecciona la \(GPO\) objeto de directiva de grupo en el que se aplica la directiva QoS. Mediante el Asistente para la directiva de QoS, el Administrador de TI crea una directiva de QoS denominada "servidor LOB directiva" que especifica un valor de DSCP high\ prioridad de 44 para todas las aplicaciones, cualquier dirección IP, TCP y UDP y número de puerto.

Se aplica la directiva de QoS solo a los servidores de línea de negocio al vincular el GPO a la unidad organizativa que contiene solo estos servidores a través de la herramienta de consola de administración de directivas de grupo \(GPMC\). Esta directiva de línea de negocio inicial del servidor aplica el valor DSCP high\ prioridad siempre que el equipo envía el tráfico de red. Más adelante se pueden editar esta directiva QoS \ (en el Editor de objetos de directiva de grupo tool\) para incluir los números de puerto de la aplicación ERP, lo que limita la directiva para aplicar solo cuando se usa el número de puerto especificado.

#### <a name="qos-policy-for-the-finance-group"></a>Directiva de QoS para el grupo de finanzas

Mientras que muchos de los grupos dentro de la empresa tener acceso a la aplicación ERP, el grupo de finanzas depende de esta aplicación ante los clientes y requiere que el grupo de forma coherente de alto rendimiento de la aplicación.

Para garantizar que el grupo de finanzas admite a sus clientes, la directiva QoS debe clasificar el tráfico de estos usuarios como de alta prioridad. Sin embargo, la directiva no debe aplicarse cuando los miembros del grupo de Finanzas usan aplicaciones que no sean de la aplicación ERP. 

Por este motivo, el departamento de TI define una directiva de QoS segunda llamada "cliente LOB directiva" en la herramienta del Editor de objetos de directiva de grupo que se aplica un valor DSCP de 60 cuando el grupo de usuarios de finanzas ejecuta la aplicación ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Directiva QoS para una aplicación de la copia de seguridad

Una aplicación independiente de copia de seguridad se ejecuta en todos los equipos. Para garantizar el que tráfico de la copia de seguridad de la aplicación no usa todos los recursos de red disponible, el departamento de TI crea una directiva de copia de seguridad de datos. Esta directiva copia de seguridad especifica un valor de 1 basado en el nombre del archivo ejecutable de la aplicación de copia de seguridad, que es DSCP **backup.exe **. 

Un tercer GPO se crea y se implementa para todos los equipos cliente del dominio. Siempre que la aplicación de la copia de seguridad envía datos, se aplica el valor DSCP de baja prioridad, incluso si se origina desde equipos del departamento de finanzas.
  
>[!NOTE]
>Envía el tráfico de red sin una directiva de QoS con un valor DSCP de 0.

### <a name="scenario-policies"></a>Directivas de escenario

La siguiente tabla resume las directivas de QoS para este escenario.
  
|Nombre de la directiva|Valor DSCP|Velocidad de aceleración|Aplica a las unidades organizativas|Descripción|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Ninguna directiva]|0|Ninguno|[Ninguna implementación]|Mejor tratamiento de esfuerzo (predeterminado) para el tráfico sin clasificar.|  
|Datos de copia de seguridad|1|Ninguno|Todos los clientes|Se aplica un valor DSCP de baja prioridad para estos datos de forma masiva.|  
|Servidor de línea de negocio|44|Ninguno|Equipo unidad organizativa para los servidores ERP|Se aplica DSCP de alta prioridad para el tráfico de servidor ERP|  
|Cliente de línea de negocio|60|Ninguno|Grupo de usuarios de finanzas|Se aplica DSCP de alta prioridad ERP tráfico de clientes|  

>[!NOTE]
>Valores DSCP se representan en formato decimal.

Con las directivas de QoS definido y aplicado mediante la directiva de grupo, el tráfico de red saliente recibe el valor DSCP directivas específicas. Los enrutadores, a continuación, proporcionan diferencial tratamiento basado en estos valores DSCP mediante la cola. Para este departamento de TI, se configuran los enrutadores con cuatro colas: prioridad alta, prioridad medio, mejor esfuerzo y baja prioridad.

Cuando llegue el tráfico en el enrutador con los valores DSCP de "servidor LOB directiva" y "cliente LOB directiva," se colocan los datos en las colas de alta prioridad. Tráfico con un valor de 0 DSCP recibe un nivel mayor esfuerzo de servicio. Paquetes con un valor de 1 (desde la aplicación de la copia de seguridad) DSCP reciben tratamiento de baja prioridad.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Requisitos previos para dar prioridad a una aplicación de línea de negocio

Para completar esta tarea, asegúrate de que cumples los requisitos siguientes:

- Los equipos implicados ejecutan sistemas operativos de QoS\ compatible.

- Los equipos implicados son miembros de un dominio de Active Directory Domain Services \(AD DS\) para que se pueden configurar mediante la directiva de grupo.

- Redes TCP/IP se configuran con enrutadores configurados para DSCP \(RFC 2474\). Para obtener más información, consulta [RFC 2474](http://www.ietf.org/rfc/rfc2474.txt).

- Cumple los requisitos de las credenciales administrativas.

#### <a name="administrative-credentials"></a>Credenciales administrativas

Para completar esta tarea, debe ser capaz de crear e implementar objetos de directiva de grupo.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configurar el entorno de prueba para dar prioridad a una aplicación de línea de negocio

Para configurar el entorno de prueba, completa las siguientes tareas.

- Crear un dominio de AD DS con los clientes y los usuarios que se agrupan en las unidades de organización. Para obtener instrucciones sobre la implementación de AD DS, consulta el [guía básica de red ](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurar los enrutadores a differentially cola basadas en valores DSCP. Por ejemplo, el valor DSCP 44 entra en una cola de "Platino" y todos los demás son ponderada fair-en cola.

>[!NOTE]
> Puede ver valores DSCP mediante capturas de red con herramientas como el Monitor de red. Después de realizar una captura de red, puede observar el campo de procedimientos en los datos capturados.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Pasos para dar prioridad a una aplicación de línea de negocio

Para dar prioridad a una aplicación de línea de negocio, completa las siguientes tareas:

1. Crear y vincular un \(GPO\) objeto de directiva de grupo con una directiva de QoS.

2. Configurar los enrutadores para tratar differentially un line-of-business aplicación (mediante Queue Server) en función de los valores DSCP seleccionados. Los procedimientos de este procedimiento variará en función del tipo de enrutadores que tienes.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Escenario 2: Establecer prioridades para el tráfico de red para una aplicación de servidor HTTP

En Windows Server 2016, basada en directivas QoS incluye la característica directivas basadas en la dirección URL. Las directivas de la dirección URL permiten administrar el ancho de banda para servidores HTTP.

Muchas aplicaciones de empresa se desarrolladas para y se hospedan en servidores web de Internet Information Services \(IIS\), y se accede a las aplicaciones Web de exploradores en los equipos cliente.

En este escenario, se supone que administran un conjunto de servidores IIS ese vídeos de formación de host para todos los empleados de su organización. El objetivo es asegurarse de que el tráfico de estos servidores de vídeo no saturar la red y asegúrese de que el tráfico de vídeo se diferencian de voz y datos de tráfico en la red. 

La tarea es similar a la tarea en el escenario 1. Vas a diseñar y configurar las opciones de administración de tráfico, como el valor DSCP para el tráfico de vídeo, y el límite de velocidad de la misma como lo harías para las aplicaciones de línea de negocio. Pero cuando se especifica el tráfico, en lugar de proporcionar el nombre de la aplicación, solo escribe la dirección URL a la que la aplicación de servidor HTTP responderá: por ejemplo,https://hrweb/training.
  
> [!NOTE]
>No puedes usar directivas de QoS basada en la dirección URL para priorizar el tráfico de red en equipos que ejecutan sistemas operativos Windows que se publicaron antes de instalar Windows 7 y Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Reglas de prioridad para directivas basadas en la dirección URL

Todas las direcciones URL siguientes son válidas y pueden ser especificado en la directiva QoS y aplicar simultáneamente a un equipo o un usuario:

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

¿Pero cuál va a recibir prioridad? Las reglas son simples. Se da prioridad a las directivas basadas en la dirección URL en un orden de lectura de izquierda a derecha. Por lo tanto, desde la máxima prioridad a la prioridad más baja, los campos de URL son:
  
[1. Esquema de dirección URL de](#bkmk_QoS_UrlScheme)

[2. Host de la dirección URL de](#bkmk_QoS_UrlHost)

[3. Puerto de la dirección URL de](#bkmk_QoS_UrlPort)

[4. Ruta de acceso de dirección URL de](#bkmk_QoS_UrlPath)

Detalles son los siguientes:

####  <a name="bkmk_QoS_UrlScheme"></a>1. esquema de dirección URL de

 `https://` Tiene una prioridad mayor que `http://`.

####  <a name="bkmk_QoS_UrlHost"></a>2. host de la dirección URL de

 De la prioridad mayor a menor, son:

1. Nombre de host

2. Dirección IPv6

3. Dirección IPv4

4. Comodín

En el caso de nombre de host, un nombre de host con elementos más puntos (más profundidad) tiene una prioridad mayor que un nombre de host con menos elementos puntos. Por ejemplo, entre los nombres de host siguientes:

- video.internal.training.hr.mycompany.com (profundidad = 6)
  
- selfguide.training.mycompany.com (profundidad = 4)
  
- Formación (profundidad = 1)
  
- Biblioteca (profundidad = 1)
  
 **video.internal.training.hr.mycompany.com** tiene la prioridad más alta, y **selfguide.training.mycompany.com** tiene la máxima prioridad siguiente. **Formación** y **biblioteca** comparten la misma prioridad más baja.  
  
####  <a name="bkmk_QoS_UrlPort"></a>3. puerto de la dirección URL de

Una determinada o un número de puerto implícito tiene mayor prioridad que un puerto de comodín.

####  <a name="bkmk_QoS_UrlPath"></a>4. ruta de acceso de dirección URL de

Como un nombre de host, una ruta de acceso de la dirección URL puede constar de varios elementos. Con más elementos siempre tiene una prioridad mayor que el que tiene el menor. Por ejemplo, se muestran las siguientes rutas de acceso por prioridad:  

1.  /eBooks/Tech/Windows/Networking/QoS

2.  / libros electrónicos/técnica/windows /

3.  /ebooks

4.  /

Si un usuario elige incluir todos los subdirectorios y archivos siguiendo una ruta de acceso de la dirección URL, esta ruta de acceso de la dirección URL tendrá una prioridad más baja que tendría si la opción no se han realizado.

Un usuario también puede especificar una dirección IP de destino en una directiva basada en la dirección URL. La dirección IP de destino tiene una prioridad menor a cualquiera de los cuatro campos de URL que se han descrito anteriormente.
  
### <a name="quintuple-policy"></a>Directiva quíntuplo

Una directiva quíntuplo se especifica mediante el identificador del protocolo, dirección IP de origen, puerto de origen, dirección IP y puerto de destino. Una directiva quíntuplo siempre tiene una prioridad mayor que cualquier directiva basada en la dirección URL. 

Si ya se aplica una directiva quíntuplo para un usuario, una nueva directiva basada en la dirección URL no provocará conflictos en cualquier parte del cliente de que los usuarios de equipos.

El siguiente tema en esta guía, encontrarás [administrar la directiva de QoS](qos-policy-manage.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).
