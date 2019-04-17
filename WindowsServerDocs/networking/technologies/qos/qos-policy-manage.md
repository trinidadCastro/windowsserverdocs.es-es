---
title: Administrar la directiva de QoS
description: Este tema proporciona instrucciones sobre cómo crear y administrar la directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>Administrar la directiva de QoS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo usar al Asistente para directivas de QoS para crear, editar o eliminar una directiva de QoS.

>[!NOTE]
>  Además de este tema, la siguiente documentación de administración de directiva de QoS está disponible.
> 
>  - [Errores y eventos de directiva de QoS](qos-policy-errors.md)

En sistemas operativos Windows, directiva QoS combina la funcionalidad de QoS basadas en estándares y con la capacidad de administración de directiva de grupo. Configuración de esta combinación hace que para una fácil aplicación de directivas de QoS a objetos de directiva de grupo. Windows incluye un asistente de directiva de QoS para ayudarte a realizar las siguientes tareas.

-  [Crear una directiva de QoS](#bkmk_createpolicy)

-  [Ver, editar o eliminar una directiva de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Crear una directiva de QoS

Antes de crear una directiva de QoS, es importante comprender los dos controles QoS claves que se usan para administrar el tráfico de red:

- Valor DSCP

-   Velocidad de aceleración

### <a name="prioritizing-traffic-with-dscp"></a>Dar prioridad al tráfico con DSCP

Como se mencionó en el ejemplo anterior de la aplicación de línea de negocio, puedes definir la prioridad de tráfico de red saliente usando **especificar el valor de DSCP** para configurar una directiva de QoS con un valor DSCP específico. 

Como se describe en la RFC 2474, DSCP admite valores de 0a 63 especificarse en el campo de procedimientos de un paquete IPv4 y en el campo de la clase de tráfico en IPv6. Enrutadores de red usan el valor DSCP para clasificar los paquetes de red y a la cola de forma adecuada.
  
> [!NOTE]
>  De manera predeterminada, el tráfico de Windows tiene un valor DSCP de 0.
  
El número de colas y su comportamiento priorización debe diseñarse como parte de la estrategia de QoS de su organización. Por ejemplo, la organización puede tener cinco colas: tráfico sensibles a la latencia, el tráfico de control, tráfico crítico para la empresa, mejor esfuerzo tráfico y tráfico de transferencia de datos de forma masiva.  
  
### <a name="throttling-traffic"></a>Limitación del tráfico

Además de los valores DSCP, limitación es otro control clave para administrar el ancho de banda de red. Como se mencionó anteriormente, puedes usar la **especificar velocidad de aceleración** configuración para configurar una directiva de QoS con una velocidad de aceleración específicos para el tráfico saliente. Mediante el uso de limitación, una directiva de QoS limita el tráfico de red saliente a una velocidad de aceleración especificado. Tanto el marcado de límite DSCP puede usarse juntas para administrar eficazmente el tráfico.

>[!NOTE]
>De manera predeterminada, la **especificar velocidad de aceleración** casilla no está seleccionada.

Para crear una directiva de QoS, modifica la configuración de un objeto de directiva de grupo (GPO) desde dentro de la herramienta de consola de administración de directivas de grupo (GPMC). GPMC, a continuación, abre el Editor de objetos de directiva de grupo.

Los nombres de directiva de QoS deben ser únicos. ¿Cómo se aplican directivas a servidores y los usuarios finales depende de la directiva de QoS se almacena en el Editor de objetos de directiva de grupo:

- Una directiva de QoS en equipo equipo\Configuración Settings\QoS directiva se aplica a equipos, independientemente del usuario que ha iniciado sesión. Normalmente se utilizan las directivas de QoS basados en el equipo para los equipos servidor.

- Una directiva de QoS en la directiva de usuario equipo\Configuración Settings\QoS se aplica a los usuarios después de que hayan iniciado sesión, independientemente del equipo que hayan iniciado sesión.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para crear una nueva directiva de QoS con el Asistente para directivas de QoS

-   En el Editor de objetos de directiva de grupo, haz clic en cualquiera de los **directiva QoS** nodos y, a continuación, haz clic en **crear una nueva directiva **.

### <a name="wizard-page-1---policy-profile"></a>Página del asistente 1: perfil de la directiva

En la primera página del Asistente para la directiva de QoS, puedes especificar un nombre de directiva y configurar cómo QoS controla el tráfico de red saliente.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar la página de perfil de la directiva del Asistente para la directiva basada en QoS

1. En **nombre de la directiva**, escribe un nombre para la directiva QoS. El nombre debe identificar la directiva.

2. También puedes usar **especificar el valor de DSCP** para habilitar marcado DSCP y, a continuación, configurar un valor de DSCP entre 0 y 63.

3. También puedes usar **especificar velocidad de aceleración** para habilitar la limitación del tráfico y configurar la velocidad de aceleración. El valor de velocidad de acelerador debe ser mayor que 1 y puedes especificar unidades de kilobytes por segundo \(KBps\) o megabytes por segundo \(MBps\).

4. Haz clic en **siguiente**.

### <a name="wizard-page-2---application-name"></a>Página del asistente 2: nombre de la aplicación

En la segunda página del Asistente para la directiva QoS puede aplicar la directiva a todas las aplicaciones, a una aplicación específica, según se identifica por su nombre del archivo ejecutable, en una ruta de acceso y nombre de la aplicación, o para las aplicaciones de servidor HTTP que controlan las solicitudes de una dirección URL específica.

- **Todas las aplicaciones** especifica que la configuración de administración de tráfico en la primera página del Asistente para la directiva de QoS se aplica a todas las aplicaciones.

- **Solo las aplicaciones con este nombre ejecutable** especifica que la configuración de administración de tráfico en la primera página del Asistente para la directiva QoS para una aplicación específica. El archivo ejecutable asigna un nombre y debe acabar con la extensión de nombre de archivo .exe.

- **Solo aplicaciones de servidor HTTP responder a las solicitudes para esta dirección URL** especifica que la configuración de administración de tráfico en la primera página del Asistente para la directiva de QoS se aplica a solo determinadas aplicaciones de servidor HTTP.

Opcionalmente, puedes escribir la ruta de acceso de la aplicación. Para especificar una ruta de acceso de la aplicación, incluye la ruta de acceso con el nombre de la aplicación. La ruta de acceso puede incluir variables de entorno. Por ejemplo, %ProgramFiles%\My Path\MyApp.exe o c:\program files\my application path\myapp.exe.

>[!NOTE]
>La ruta de acceso de la aplicación no puede incluir una ruta de acceso que se resuelve en un vínculo simbólico.

La dirección URL debe ajustarse al [RFC 1738](http://tools.ietf.org/html/rfc1738), en forma de `http[s]://<hostname\>:<port\>/<url-path>`. Puedes usar un comodín, `‘*’`, para `<hostname>`o `<port>`, por ejemplo, `http://training.\*/, https://\*.\*`, pero el carácter comodín no puede indicar una subcadena del `<hostname>`o `<port>`.

En otras palabras, ninguno `http://my\*site/`ni `https://\*training\*/`es válido. 

Opcionalmente, puedes comprobar **incluyen los subdirectorios y archivos** para llevar a cabo la coincidencia en todos los subdirectorios y archivos siguiendo una dirección URL. Por ejemplo, si esta opción está activada y la dirección URL es `http://training`, directiva de QoS tendrá en cuenta las solicitudes de` http://training/video` coincide bien.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar la página de nombre de la aplicación del Asistente para la directiva QoS

1. En **QoS esta directiva se aplica a**, seleccione **todas las aplicaciones** o **solo las aplicaciones con este nombre ejecutable **.

2. Si seleccionas **solo las aplicaciones con este nombre ejecutable**, especifica un nombre de ejecutable terminan con la extensión de nombre de archivo .exe.

3. Haz clic en **siguiente**.

### <a name="wizard-page-3---ip-addresses"></a>Página del asistente 3 - direcciones IP

En la tercera página del Asistente para la directiva QoS puede especificar las condiciones de dirección IP para la directiva de QoS, incluidos los siguientes:

- Todos los origen direcciones IPv4 o IPv6 o direcciones IPv4 o IPv6 de origen específico

- Todas las direcciones IPv4 o IPv6 de destino o direcciones IPv4 o IPv6 de destino específico

Si seleccionas **solo para la siguiente dirección IP de origen** o **solo para la siguiente dirección IP de destino**, debes escribir uno de los siguientes:

- Abordar un IPv4, como `192.168.1.1`

- Un prefijo de dirección IPv4 mediante notación de longitud de prefijo de red, como `192.168.1.0/24`

- Abordar un IPv6, como `3ffe:ffff::1`

- Dirección de un IPv6 prefijo, como `3ffe:ffff::/48`

Si seleccionas ambos **solo para la siguiente dirección IP de origen** y **solo para la siguiente dirección IP de destino**, direcciones o prefijos de direcciones deben ser de cualquier IPv4 o IPv6 basados en.

Si se especifica la dirección URL HTTP para aplicaciones de servidor en la página anterior del asistente, verás que la dirección IP de origen para la directiva de QoS en esta página del asistente está atenuada. 

Esto sucede porque la dirección IP de origen es la dirección del servidor HTTP y no es configurable aquí. Por otro lado, se puede personalizar la directiva especificando la dirección IP de destino. Esto permite crear diferentes directivas para distintos clientes mediante el uso de las mismas aplicaciones de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar la página de direcciones IP del Asistente para la directiva QoS

1. En **QoS esta directiva se aplica a** (origen), selecciona **cualquier dirección IP de origen** o **solo para el siguiente IP de origen dirección **.

2. Si has seleccionado **solo la siguiente dirección IP de origen**y especificar una dirección IPv4 o IPv6 o prefijo.

3. En **QoS esta directiva se aplica a** (destino), selecciona **cualquier dirección de destino** o **solo para la siguiente dirección IP de destino.**

4. Si has seleccionado **solo para la siguiente dirección IP de destino**, especificar una dirección IPv4 o IPv6 o un prefijo que corresponde al tipo de dirección o prefijo especificado para la dirección de origen.

5.  Haz clic en **siguiente**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Página del asistente 4: protocolos y puertos

En la cuarta página del Asistente para la directiva de QoS, puedes especificar los tipos de tráfico y los puertos que se controlan con la configuración en la primera página del asistente. Puedes especificar:  
-   El tráfico TCP, tráfico UDP o ambos  

-   Todos los origen puertos, un intervalo de puertos de origen o un puerto de origen específico

-   Todos los puertos de destino, un intervalo de puertos de destino o un puerto de destino específico  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar la página de protocolos y puertos del Asistente para la directiva QoS

1. En **seleccione el protocolo que se aplica esta directiva QoS a**, selecciona **TCP**, **UDP**, o **TCP y UDP **.

2. En **especificar el número de puerto de origen**, selecciona **desde cualquier puerto origen** o **desde este número de puerto de origen **.

3. Si has seleccionado **desde este número de puerto de origen**, escribe un número de puerto entre 1 y 65535.

     Opcionalmente, puedes especificar un intervalo de puertos en el formato de "*bajo*:*alto*," donde *bajo* y *alto* representan los límites inferiores límites y superior del intervalo de puertos, ambos inclusive. *Bajo* y *alto* cada uno de ellos debe ser un número entre 1 y 65535. No se permiten espacios entre el carácter de dos puntos (:) y los números.

4. En **especificar el número de puerto de destino**, selecciona **a cualquier puerto destino** o **a este número de puerto de destino**.

5. Si has seleccionado **a este número de puerto de destino** en el paso anterior, escribe un número de puerto entre 1 y 65535.

Para completar la creación de la nueva directiva de QoS, haz clic en **finalizar** en la **protocolos y puertos** página del Asistente para la directiva QoS. Una vez completado, la nueva directiva de QoS aparece en el panel de detalles del Editor de objetos de directiva de grupo.  
  
Para aplicar la configuración de directiva de QoS a usuarios o equipos, vincular el GPO en el que se encuentran a un contenedor de Active Directory Domain Services, como un dominio, un sitio o una unidad organizativa (OU) las directivas de QoS.  
  
##  <a name="bkmk_editpolicy"></a>Ver, editar o eliminar una directiva de QoS

Las páginas de la directiva de QoS Asistente descrito anteriormente corresponden a las páginas de propiedades que se muestran cuando ver o edición las propiedades de una directiva.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para ver las propiedades de una directiva de QoS  
  
-   Haz clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haz clic en **propiedades**.  
  
     El Editor de objetos de directiva de grupo se muestra la página de propiedades con las fichas siguientes:  
  
    -   Perfil de la directiva  
  
    -   Nombre de la aplicación  
  
    -   Direcciones IP  
  
    -   Protocolos y puertos  
  
### <a name="to-edit-a-qos-policy"></a>Para editar una directiva de QoS  
  
-   Haz clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haz clic en **Editar directiva existente**.  
  
     Muestra el Editor de objetos de directiva de grupo la **editar una directiva existente de QoS** cuadro de diálogo.  
  
### <a name="to-delete-a-qos-policy"></a>Para eliminar una directiva de QoS  
  
-   Haz clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haz clic en **Eliminar directiva**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Informes de directiva de QoS GPMC 

Después de aplicar un número de directivas de QoS en toda la organización, puede ser útil o necesario revisar cómo se aplican las directivas de manera periódica. Un resumen de las directivas de QoS para un usuario específico o el equipo puede verse mediante informes de GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para ejecutar al Asistente para resultados de directiva de grupo para un informe de las directivas de QoS  
  
-   En GPMC, haz clic en el **resultados de la directiva de grupo** nodo y, a continuación, selecciona el menú de la opción para **Asistente para resultados de directiva de grupo.**  
  
Después de que se generan los resultados de la directiva de grupo, haz clic en el **configuración** pestaña. En la **configuración** ficha, las directivas de QoS pueden encontrarse en los nodos "Directiva de equipo equipo\Configuración Settings\QoS" y "Directiva de usuario equipo\Configuración Settings\QoS".  
  
En la **configuración** ficha, se enumeran las directivas de QoS por sus nombres de directiva de QoS con su valor DSCP, velocidad de aceleración, las condiciones de la directiva y GPO prevalente enumerados en la misma fila.  
  
La vista de resultados de la directiva de grupo identifica los GPO prevalentes. Cuando varios GPO tienen las directivas de QoS con el mismo nombre de la directiva de QoS, se aplica el GPO con la prioridad más alta del GPO. Se trata de los GPO prevalentes. Directivas en conflicto QoS (identificadas por nombre de directiva) que están vinculadas a una prioridad más baja, no se aplicarán los GPO. Ten en cuenta que las prioridades de GPO definen qué directivas QoS se implementan en el sitio, dominio o unidad organizativa, según corresponda. Después de la implementación en un nivel de usuario o equipo, la [las reglas de prioridad de directiva de QoS](#BKMK_precedencerules) determinar qué tráfico se permite o bloquea.  
  
Valor DSCP, velocidad de aceleración y condiciones de la directiva de la directiva de QoS también son visibles en el Editor de objetos de directiva de grupo (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configuración avanzada de usuarios remotos y móviles  
Con la directiva de QoS, el objetivo es administrar el tráfico de red de la empresa. En escenarios móviles, los usuarios podrían enviar tráfico o desactivar la red de empresa. Como las directivas de QoS no son relevantes cuando está fuera de la red empresarial, las directivas de QoS se habilitan solo en las interfaces de red que están conectadas a la empresa para Windows 8, Windows 7 o Windows Vista.  
  
Por ejemplo, un usuario puede conectarse su equipo portátil a su red empresarial a través de una red privada virtual (VPN) de una cafetería. Para VPN, la interfaz de red física (como inalámbricos) no tendrán las directivas de QoS aplicadas. Sin embargo, la interfaz VPN tendrá las directivas de QoS aplica porque se conecta a la empresa. Si el usuario escribe más adelante otra red empresarial que no tiene una relación de confianza de AD DS, no se habilitará las directivas de QoS.  
  
Ten en cuenta que estos escenarios móviles no se aplican a las cargas de trabajo del servidor. Por ejemplo, un servidor con varios adaptadores de red puede sentarse en el borde de la red de la empresa. El departamento de TI puede optar por hacer QoS directivas del Acelerador tráfico que sale de la empresa; Sin embargo, este adaptador de red que envía el tráfico de salida no necesariamente se conecta a la red de empresa. Por este motivo, las directivas de QoS siempre están habilitadas en todas las interfaces de red de un equipo que ejecute Windows Server 2012.  
  
> [!NOTE]
>  Habilitación selectiva solo se aplica a las directivas de QoS y no a la configuración avanzada de QoS que se explica a continuación en este documento.  
  
### <a name="advanced-qos-settings"></a>Configuración avanzada de QoS

Configuración avanzada de QoS proporciona controles adicionales para los administradores de TI a administrar el consumo de red del equipo y el marcado de DSCP. Configuración avanzada de QoS se aplica solo en el nivel de equipo, mientras que las directivas de QoS pueden aplicarse en el equipo y el usuario niveles.

#### <a name="to-configure-advanced-qos-settings"></a>Para configurar la configuración avanzada de QoS

1.  Haz clic en **configuración del equipo**y, a continuación, haz clic en **configuración de Windows en la directiva de grupo**.
  
2.  Haz clic en **directiva QoS**y, a continuación, haz clic en **configuración avanzada de QoS**.

     La figura siguiente muestra las dos avanzada QoS las pestañas de configuración: **tráfico TCP entrante** y **marcar invalidar DSCP**.
  
> [!NOTE]
>  Configuración avanzada de QoS son opciones de configuración de directiva de grupo de nivel de equipo.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>QoS configuración avanzada: el tráfico TCP entrante

**El tráfico TCP entrante** controla el consumo de ancho de banda TCP en el lado del destinatario, mientras que las directivas de QoS afectan el tráfico saliente TCP y UDP. 

Estableciendo un rendimiento más bajo nivel en el **tráfico TCP entrante** ficha, TCP limita el tamaño de la se anuncia la ventana de recepción TCP. El efecto de esta configuración se velocidades de un mayor rendimiento y uso de las conexiones TCP con anchos de banda mayores o latencias (producto del retraso de ancho de banda) vínculos. De manera predeterminada, los equipos que ejecutan Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista se establecen en el nivel de rendimiento máximo.
  
La recepción de TCP ventana ha cambiado en Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista de versiones anteriores de Windows. Las versiones anteriores de Windows limita la ventana de recepción TCP a un máximo de entre 64 kilobytes (KB), mientras que Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista dinámicamente tamaño de la ventana de recepción hasta 16 megabytes (MB). En el control de tráfico TCP entrante, puedes controlar el nivel de rendimiento de entrada, Establece el valor máximo que puede alcanzar la ventana recibir TCP. Los niveles se corresponden con los siguientes valores máximos. 
  
|Nivel de rendimiento de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1MB|
|3|16MB|

El tamaño real de la ventana puede ser un valor igual o inferior de la máxima, según las condiciones de red.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para definir el lado de recepción TCP

1. En el Editor de objetos de directiva de grupo, haz clic en **directiva de equipo Local**, haz clic en **configuración de Windows**, haga clic **directiva QoS**y, a continuación, haz clic en **configuración avanzada de QoS**.
  
2. En **el rendimiento de recepción TCP**, selecciona **configurar el rendimiento de recepción de TCP**y, a continuación, selecciona el nivel de rendimiento que desee.

3.  Vincular el GPO a la unidad organizativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>QoS configuración avanzada: DSCP invalidación de marcado

Invalidación de marcado de DSCP restringe la capacidad de aplicaciones especificar, o "marcar": DSCP otros valores que los especificados en las directivas de QoS. Especificando que las aplicaciones se pueden establecer valores DSCP, las aplicaciones pueden establecer valores DSCP distinto de cero. 

Especificando **Ignorar**, las aplicaciones que usan QoS APIs tendrán sus valores DSCP establecidos en cero, y solo las directivas de QoS pueden establecer valores DSCP. 

De manera predeterminada, equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista permiten que las aplicaciones especificar valores DSCP; aplicaciones y dispositivos que no uses las APIs QoS no se reemplazan.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores de Multimedia inalámbrica y DSCP

La [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) ha establecido una certificación para aplicaciones Multimedia inalámbrica \(WMM\) que define cuatro categorías de acceso \(WMM_AC\) para establecer prioridades de tráfico de red para transmitir en una red inalámbrica de Wi\-Fi. El acceso a las categorías incluyen \ (en orden de mayor a menor priority\): voz, vídeo, mejor esfuerzo y en segundo plano; respectivamente abreviar como VO, VI, BE y BK. La especificación de WMM define qué DSCP valores corresponden con cada una de las categorías de cuatro acceso:
  
|Valor DSCP|Categoría de acceso WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Mejor esfuerzo (BE)|
|8-23|En segundo plano (negro)|

Puedes crear directivas de QoS que usan estos valores DSCP para asegurarte de que los equipos portátiles con Wi\-Fi Certified™ para adaptadores inalámbricos de WMM reciban prioritario control cuando se asocia con Wi\-Fi Certified WMM puntos de acceso.
  
### <a name="BKMK_precedencerules"></a>Reglas de prioridad de directiva de QoS

Al igual que las prioridades del GPO, las directivas de QoS tienen reglas de prioridad para resolver conflictos cuando varias directivas de QoS se aplican a un conjunto específico de tráfico. Para el tráfico TCP o UDP saliente, solo una directiva de QoS puede aplicarse a la vez, lo que significa que las directivas de QoS no tienen un efecto acumulativo, como donde se podrían sumar tasas de acelerador.

En general, la directiva de QoS con el máximo coincidentes condiciones wins. Cuando se aplican varias directivas de QoS, las reglas se dividen en tres categorías: nivel de usuario en comparación con el nivel de equipo; aplicación frente a la quíntuple red; y entre la red quintuple.

Por *red quíntuplo*, que significa la dirección IP de origen, la dirección IP de destino, el puerto de origen, el puerto de destino y \(TCP/UDP\) de protocolo.  

 **Directiva de QoS de nivel de usuario tiene prioridad sobre la directiva de QoS de nivel de equipo**

Esta regla enormemente facilita la administración de los administradores de red de QoS GPO, especialmente para las directivas de usuario basados en grupo. Por ejemplo, si el Administrador de red quiere definir una directiva de QoS para un grupo de usuarios, solo puede crear y distribuir un GPO a ese grupo. No tienen por qué preocuparte acerca de los equipos que los usuarios han iniciado sesión y si los equipos no tendrá directivas en conflicto de QoS definidas, porque, si existe un conflicto, la directiva del nivel de usuario siempre tendrá prioridad.

> [!NOTE]
>  Una directiva de QoS de nivel de usuario solo es aplicable para el tráfico que se genera de ese usuario. Otros usuarios de un equipo específico y el propio equipo, no estará sujeto a las directivas de QoS que se definen para ese usuario.

 **Especificidad de la aplicación y tiene preferencia sobre quíntuple de red**

Cuando varias directivas de QoS coincide con el tráfico específico, se aplica la directiva más específica. Las directivas que identifican las aplicaciones, se considera que una directiva que incluye la ruta del archivo de la aplicación de envío más específica que otra directiva que solo identifica el nombre de la aplicación (ninguna ruta de acceso). Si aún se aplican varias directivas de aplicaciones, las reglas de prioridad usan la red quíntuplo para encontrar a la mejor coincidencia.

Como alternativa, pueden aplicar de varias directivas de QoS para el mismo tráfico especificando condiciones no se superponen. Entre las condiciones de las aplicaciones y el quíntuple de red, la directiva que especifica la aplicación se considera más específica y se aplica. 

Por ejemplo, policy_A especifica únicamente una aplicación nombre (app.exe) y policy_B especifica destino IP dirección 192.168.1.0/24. Cuando estas directivas QoS conflicto \ (app.exe envía el tráfico a una dirección IP dentro del intervalo de 192.168.4.0/24\), se aplica policy_A.

 **Más especificidad prevalece dentro de la red quíntuplo**

Conflictos de directiva en el quíntuple de red, la directiva con las condiciones más coincidentes tiene prioridad. Por ejemplo, supongamos que policy_C especifica la dirección IP de origen "any", dirección IP de destino de direcciones 10.0.0.1, puerto de origen "any", el puerto de destino "any" y "TCP" de protocolo. 

A continuación, se supone policy_D especifica la dirección IP de origen "any", dirección IP de destino 10.0.0.1, puerto de origen de direcciones "any", puerto 80 de destino y protocolo "TCP". A continuación, policy_C y policy_D coincidir con las conexiones a 10.0.0.1:80 de destino. Como directiva QoS se aplica la directiva con las condiciones coincidentes más específicas, policy_D tiene prioridad en este ejemplo.  
  
Sin embargo, las directivas de QoS podrían tener un número igual de condiciones. Por ejemplo, varias directivas pueden cada uno especifica solo una de ellas (pero no es lo mismo) parte de la quíntuple de red. Entre quíntuple de red, el siguiente orden es de mayor a menor prioridad:

- Dirección IP de origen

- Dirección IP de destino

- Puerto de origen

- Puerto de destino

- Protocolo (TCP o UDP)

Dentro de una condición específica, como la dirección IP, una dirección IP más específica se trata con mayor prioridad; Por ejemplo, una dirección IP 192.168.4.1 es más específica que 192.168.4.0/24.

Diseñar las directivas de QoS específicamente posible para simplificar la capacidad de su organización para comprender qué directivas están en vigor.

El siguiente tema en esta guía, encontrarás [errores y eventos de la directiva de QoS](qos-policy-errors.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).
