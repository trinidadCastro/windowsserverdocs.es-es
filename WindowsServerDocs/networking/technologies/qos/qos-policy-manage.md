---
title: Administrar la directiva QoS
description: En este tema se proporcionan instrucciones sobre cómo crear y administrar la Directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3cff51b3cf76d3224832bf99ff966bf473d6ff6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871849"
---
# <a name="manage-qos-policy"></a>Administrar la directiva QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca del uso del Asistente para directivas de QoS con el fin de crear, editar o eliminar una directiva de QoS.

>[!NOTE]
>  Además de este tema, está disponible la siguiente documentación de administración de directivas de QoS.
> 
>  - [Eventos y errores de la Directiva de QoS](qos-policy-errors.md)

En los sistemas operativos Windows, la directiva QoS combina la funcionalidad de QoS basada en estándares con la capacidad de administración de directiva de grupo. La configuración de esta combinación facilita la aplicación de directivas de QoS a directiva de grupo objetos. Windows incluye un asistente para directivas QoS que le ayudará a realizar las siguientes tareas.

-  [Crear una directiva de QoS](#bkmk_createpolicy)

-  [Ver, editar o eliminar una directiva de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Crear una directiva de QoS

Antes de crear una directiva de QoS, es importante que comprenda los dos controles clave de QoS que se usan para administrar el tráfico de red:

- Valor de DSCP

-   Velocidad del acelerador

### <a name="prioritizing-traffic-with-dscp"></a>Priorizar el tráfico con DSCP

Como se indicó en el ejemplo anterior de la aplicación de línea de negocio, puede definir la prioridad del tráfico de red saliente mediante el uso de **especificar el valor de DSCP** para configurar una directiva de QoS con un valor de DSCP específico. 

Tal y como se describe en RFC 2474, DSCP permite especificar valores de 0 a 63 en el campo TOS de un paquete IPv4 y en el campo clase de tráfico de IPv6. Los enrutadores de red usan el valor de DSCP para clasificar los paquetes de red y ponerlos en cola de forma adecuada.
  
> [!NOTE]
>  De forma predeterminada, el tráfico de Windows tiene un valor de DSCP de 0.
  
El número de colas y su comportamiento de asignación de prioridades deben diseñarse como parte de la estrategia de QoS de la organización. Por ejemplo, su organización puede elegir tener cinco colas: tráfico sensible a la latencia, tráfico de control, tráfico crítico para la empresa, tráfico de mejor esfuerzo y tráfico de transferencia de datos masivos.  
  
### <a name="throttling-traffic"></a>Limitar el tráfico

Junto con los valores de DSCP, la limitación es otro control clave para administrar el ancho de banda de red. Como se mencionó anteriormente, puede usar la opción **especificar velocidad del acelerador** para configurar una directiva de QoS con una velocidad de limitación específica para el tráfico de salida. Mediante el uso de la limitación, una directiva de QoS limita el tráfico de red saliente a una velocidad de limitación especificada. Tanto el límite como el marcado de DSCP se pueden utilizar en conjunto para administrar el tráfico de manera eficaz.

>[!NOTE]
>De forma predeterminada, la casilla **Especificar velocidad del acelerador** no está activada.

Para crear una directiva de QoS, edite la configuración de un objeto de directiva de grupo (GPO) desde la herramienta Consola de administración de directivas de grupo (GPMC). Después, GPMC abre el Editor de objetos de directiva de grupo.

Los nombres de las directivas QoS deben ser exclusivos. La forma en que se aplican las directivas a los servidores y los usuarios finales depende de dónde se almacena la Directiva de QoS en el Editor de objetos de directiva de grupo:

- Una directiva QoS ubicada en Configuración del equipo\Configuración de Windows\QoS basada en directivas se aplica a equipos, independientemente del usuario que tenga iniciada la sesión actualmente. Por lo general, se utilizan directivas Qos basadas en equipos para los equipos de servidores.

- Una directiva QoS ubicada en Configuración de usuario\Configuración de Windows\QoS basada en directivas se aplica a los usuarios después de que iniciaron sesión, independientemente del equipo en el que iniciaron sesión.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para crear una nueva Directiva de QoS con el Asistente para directivas de QoS

-   En Editor de objetos de directiva de grupo, haga clic con el botón secundario en cualquiera de los nodos de la **Directiva de QoS** y, a continuación, haga clic en **crear una nueva Directiva**.

### <a name="wizard-page-1---policy-profile"></a>Asistente Página 1-Perfil de Directiva

En la primera página del Asistente para directivas de QoS, puede especificar un nombre de directiva y configurar el modo en que QoS controla el tráfico de red saliente.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar la página Perfil de directiva del Asistente para directiva basada en QoS

1. En **Nombre de directiva**, escriba un nombre para la directiva QoS. El nombre debe identificar de forma única la Directiva.

2. Opcionalmente, use **especificar el valor de DSCP** para habilitar el marcado de DSCP y, a continuación, configure un valor de DSCP entre 0 y 63.

3. De forma opcional, use **Especificar velocidad del acelerador** para habilitar el límite de tráfico y configurar la velocidad del acelerador. El valor de velocidad del acelerador debe ser mayor que 1 y puede especificar unidades de kilobytes por \(segundo\) kbps o megabytes por \(segundo\)Mbps.

4. Haga clic en **Next**.

### <a name="wizard-page-2---application-name"></a>Asistente (Página 2): nombre de la aplicación

En la segunda página del Asistente para directivas de QoS puede aplicar la Directiva a todas las aplicaciones, a una aplicación específica, identificada por su nombre de archivo ejecutable, a una ruta de acceso y un nombre de aplicación, o a las aplicaciones de servidor HTTP que controlan las solicitudes de una dirección URL específica.

- **Todas las aplicaciones** especifica que la configuración de administración de tráfico de la primera página del Asistente para directivas QoS se aplica a todas las aplicaciones.

- **Solo las aplicaciones con este nombre de archivo ejecutable** especifican que la configuración de administración de tráfico de la primera página del Asistente para directivas de QoS sea para una aplicación específica. El nombre del archivo ejecutable debe terminar con la extensión de nombre de archivo .exe.

- **Solo las aplicaciones de servidor HTTP que responden a las solicitudes de esta dirección URL** especifican que la configuración de administración de tráfico de la primera página del Asistente para directivas de QoS se aplique solo a determinadas aplicaciones de servidor http.

Opcionalmente, puede especificar la ruta de acceso de la aplicación. Para especificar la ruta de acceso de una aplicación, incluya la ruta de acceso con el nombre de la aplicación. La ruta de acceso puede incluir variables de entorno. Por ejemplo, %ProgramFiles%\ruta de acceso de aplicación\MyApp.exe, o c:\archivos de programa\ruta de acceso de aplicación\myapp.exe.

>[!NOTE]
>La ruta de acceso de la aplicación no puede incluir una ruta de acceso que se resuelva como un vínculo simbólico.

La dirección URL debe ajustarse a [RFC 1738](https://tools.ietf.org/html/rfc1738), con el `http[s]://<hostname\>:<port\>/<url-path>`formato. Puede usar un carácter comodín, `‘*'`, para `<hostname>` y `https://training.\*/, https://\*.\*`/o `<port>`, por ejemplo,, pero el carácter comodín no puede indicar una subcadena `<port>`de `<hostname>` o.

En otras palabras, `https://my\*site/` ni ni `https://\*training\*/` son válidos. 

Opcionalmente, puede activar **incluir subdirectorios y archivos** para realizar la búsqueda de coincidencias en todos los subdirectorios y archivos que siguen a una dirección URL. Por ejemplo, si se activa esta opción y la dirección URL `https://training`es, la Directiva de QoS considerará las solicitudes de` https://training/video` una buena coincidencia.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar la página nombre de aplicación del Asistente para directivas de QoS

1. En **esta directiva de QoS se aplica a**, seleccione **todas las aplicaciones** o **solo aplicaciones con este nombre de archivo ejecutable**.

2. Si selecciona **Sólo las aplicaciones con el siguiente nombre de archivo ejecutable**, especifique un archivo ejecutable que termine con la extensión de nombre de archivo .exe.

3. Haga clic en **Next**.

### <a name="wizard-page-3---ip-addresses"></a>Asistente página 3-direcciones IP

En la tercera página del Asistente para directivas de QoS puede especificar las condiciones de dirección IP de la Directiva de QoS, incluidas las siguientes:

- Todas las direcciones IPv4 o IPv6 de origen o direcciones IPv4 o IPv6 de orígenes específicos

- Todas las direcciones IPv4 o IPv6 de destino o direcciones IPv4 o IPv6 de destino específicas

Si selecciona **Sólo para el siguiente prefijo o dirección IP de origen:** o **Sólo para el siguiente prefijo o dirección IP de destino:** , debe escribir una de las siguientes opciones:

- Una dirección IPv4, como`192.168.1.1`

- Un prefijo de dirección IPv4 mediante la notación de longitud de prefijo de red, como`192.168.1.0/24`

- Una dirección IPv6, como`3ffe:ffff::1`

- Un prefijo de dirección IPv6, como`3ffe:ffff::/48`

Si selecciona **solo para la siguiente dirección IP de origen** y **solo para la siguiente dirección IP de destino**, ambas direcciones o prefijos de dirección deben estar basadas en IPv4 o IPv6.

Si especificó la dirección URL de las aplicaciones de servidor HTTP en la página anterior del asistente, observará que la dirección IP de origen de la Directiva de QoS en esta página del asistente está atenuada. 

Esto es así porque la dirección IP de origen es la dirección del servidor HTTP y no se puede configurar aquí. Por otro lado, puede personalizar la Directiva especificando la dirección IP de destino. Esto permite crear directivas diferentes para distintos clientes mediante el uso de las mismas aplicaciones de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar la página direcciones IP del Asistente para directivas de QoS

1. En **esta directiva de QoS se aplica a** (origen), seleccione **cualquier dirección IP de origen** o **solo para la siguiente dirección de origen IP**.

2. Si seleccionó **solo la siguiente dirección de origen IP**, especifique un prefijo o una dirección IPv4 o IPv6.

3. En **esta directiva de QoS se aplica a** (destino), seleccione **cualquier dirección de destino** o **solo para la siguiente dirección IP de destino.**

4. Si seleccionó **sólo para la siguiente dirección IP de destino**, especifique una dirección o un prefijo IPv4 o IPv6 que se corresponda con el tipo de dirección o prefijo especificado para la dirección de origen.

5.  Haga clic en **Next**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Asistente página 4: protocolos y puertos

En la cuarta página del Asistente para directivas de QoS, puede especificar los tipos de tráfico y los puertos que se controlan mediante la configuración de la primera página del asistente. Se puede especificar:  
-   Tráfico TCP, tráfico UDP o ambos  

-   Todos los puertos de origen, un intervalo de puertos de origen o un puerto de origen específico

-   Todos los puertos de destino, un intervalo de puertos de destino o un puerto de destino específico  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar la página protocolos y puertos del Asistente para directivas de QoS

1. En **Seleccione el protocolo para el que se aplica esta directiva de QoS**, seleccione **TCP**, **UDP** o **TCP y  UDP**.

2. En **Especifique el número de puerto de origen**, seleccione **Cualquier puerto de origen** o **Desde este intervalo o número de puerto de origen**.

3. Si seleccionó **desde este número de puerto de origen**, escriba un número de Puerto entre 1 y 65535.

     Opcionalmente, puede especificar un intervalo de puertos, con el formato "*Low*:*High*", donde *Low* y *High* representan los límites inferiores y los límites superiores del intervalo de puertos, ambos incluidos. *Low* y *High* deben ser un número entre 1 y 65535. No se permite ningún espacio entre el carácter de dos puntos (:) y los números.

4. En **Especifique el número de puerto de destino**, seleccione **A cualquier puerto de destino** o **A este intervalo o número de puerto de destino**.

5. Si seleccionó **A este intervalo o número de puerto de destino** en el paso anterior, escriba un número de puerto entre 1 y 65535.

Para completar la creación de la nueva Directiva de QoS, haga clic en **Finalizar** en la página **protocolos y puertos** del Asistente para directivas de QoS. Una vez finalizada, la nueva Directiva de QoS se muestra en el panel de detalles del Editor de objetos de directiva de grupo.  
  
Para aplicar la configuración de la Directiva de QoS a usuarios o equipos, vincule el GPO en el que se encuentran las directivas de QoS a un contenedor Active Directory Domain Services, como un dominio, un sitio o una unidad organizativa (OU).  
  
##  <a name="bkmk_editpolicy"></a>Ver, editar o eliminar una directiva de QoS

Las páginas del Asistente para directivas de QoS descritas anteriormente corresponden a las páginas de propiedades que se muestran al ver o editar las propiedades de una directiva.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para visualizar las propiedades de una directiva QoS  
  
-   Haga clic con el botón secundario en el nombre de la Directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **propiedades**.  
  
     En el Editor de objetos de directiva de grupo se muestra la página de propiedades con las siguientes pestañas:  
  
    -   Perfil de directiva  
  
    -   Nombre de la aplicación  
  
    -   Direcciones IP  
  
    -   Protocolos y puertos  
  
### <a name="to-edit-a-qos-policy"></a>Para editar una directiva QoS  
  
-   Haga clic con el botón secundario en el nombre de la Directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **Editar directiva existente**.  
  
     En el Editor de objetos de directiva de grupo se muestra el cuadro de diálogo **editar una directiva QoS existente** .  
  
### <a name="to-delete-a-qos-policy"></a>Para eliminar una directiva de QoS  
  
-   Haga clic con el botón secundario en el nombre de la Directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **eliminar Directiva**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Informes de GPMC de directivas QoS 

Después de haber aplicado varias directivas de QoS en la organización, puede ser útil o necesario revisar periódicamente cómo se aplican las directivas. Un resumen de las directivas de QoS para un usuario o equipo específico se puede ver mediante informes de GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para ejecutar el Asistente para resultados de directiva de grupo para un informe de directivas QoS  
  
-   En GPMC, haga clic con el botón secundario en el nodo **resultados de directiva de grupo** y, a continuación, seleccione la opción de menú de **Directiva de grupo Asistente para resultados.**  
  
Una vez generados los resultados de directiva de grupo, haga clic en la pestaña **configuración** . En la pestaña Configuración, las directivas de QoS se pueden encontrar en los nodos " **configuración** de equipo \ Directiva de Settings\QoS de usuario" y "Directiva de usuario \ configuración de Settings\QoS de usuario".  
  
En la pestaña **configuración** , las directivas de QoS se enumeran por sus nombres de directiva de QoS con el valor de DSCP, la velocidad del acelerador, las condiciones de la Directiva y el GPO ganador que aparecen en la misma fila.  
  
La vista de resultados de directiva de grupo identifica de forma única el GPO ganador. Cuando varios GPO tienen directivas de QoS con el mismo nombre de directiva de QoS, se aplica el GPO con la mayor prioridad de GPO. Este es el GPO ganador. No se aplican las directivas de QoS en conflicto (identificadas por el nombre de directiva) que se adjuntan a un GPO de prioridad inferior. Tenga en cuenta que las prioridades de GPO definen qué directivas de QoS se implementan en el sitio, dominio o unidad organizativa, según corresponda. Después de la implementación, en el nivel de usuario o equipo, las reglas de prioridad de la [Directiva de QoS](#BKMK_precedencerules) determinan qué tráfico se permite y se bloquea.  
  
El valor de DSCP, la velocidad del acelerador y las condiciones de la Directiva de QoS también están visibles en Editor de objetos de directiva de grupo (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configuración avanzada para usuarios móviles y remotos  
Con la Directiva de QoS, el objetivo es administrar el tráfico en la red de una empresa. En escenarios móviles, es posible que los usuarios envíen o salgan tráfico en la red de la empresa. Dado que las directivas QoS no son relevantes mientras están fuera de la red de la empresa, las directivas QoS solo se habilitan en las interfaces de red que están conectadas a la empresa para Windows 8, Windows 7 o Windows Vista.  
  
Por ejemplo, un usuario podría conectar su equipo portátil a la red de su empresa a través de una red privada virtual (VPN) desde una cafetería. Para VPN, la interfaz de red física (por ejemplo, inalámbrica) no tendrá aplicada ninguna directiva de QoS. Sin embargo, la interfaz VPN tendrá directivas de QoS aplicadas porque se conecta a la empresa. Si más adelante el usuario entra en la red de otra empresa que no tiene una relación de confianza AD DS, las directivas de QoS no se habilitarán.  
  
Tenga en cuenta que estos escenarios móviles no se aplican a las cargas de trabajo de servidor. Por ejemplo, un servidor con varios adaptadores de red podría permanecer en el perímetro de la red de una empresa. El Departamento de ti puede elegir que las directivas de QoS limiten el tráfico que salida de la empresa. sin embargo, este adaptador de red que envía el tráfico de salida no tiene que volver a conectarse a la red empresarial. Por este motivo, las directivas de QoS siempre están habilitadas en todas las interfaces de red de un equipo que ejecuta Windows Server 2012.  
  
> [!NOTE]
>  La habilitación selectiva solo se aplica a las directivas de QoS y no a la configuración avanzada de QoS que se describe a continuación en este documento.  
  
### <a name="advanced-qos-settings"></a>Configuración avanzada de QoS

La configuración avanzada de QoS proporciona controles adicionales para los administradores de TI con el fin de administrar el consumo de red del equipo y las marcas de DSCP. La configuración avanzada de QoS solo se aplica en el nivel de equipo, mientras que las directivas de QoS se pueden aplicar en los niveles de equipo y usuario.

#### <a name="to-configure-advanced-qos-settings"></a>Para configurar opciones avanzadas de QoS

1.  Haga clic en **configuración del equipo**y, a continuación, en **configuración de Windows en Directiva de grupo**.
  
2.  Haga clic con el botón secundario en **Directiva de QoS**y, a continuación, haga clic en **Configuración avanzada de QoS**.

     En la ilustración siguiente se muestran las dos pestañas de configuración avanzada de QoS: El **tráfico TCP entrante** y el **marcado de DSCP invalidan**.
  
> [!NOTE]
>  La configuración avanzada de QoS es una configuración de directiva de grupo de nivel de equipo.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Configuración avanzada de QoS: tráfico entrante de TCP

El **tráfico TCP entrante** controla el consumo de ancho de banda TCP en el lado del receptor, mientras que las directivas QoS afectan al tráfico TCP y UDP saliente. 

Al establecer un nivel de rendimiento inferior en la pestaña **tráfico TCP entrante** , TCP limitará el tamaño de la ventana de recepción TCP anunciada. El efecto de esta configuración será aumentar las tasas de rendimiento y el uso de vínculos para las conexiones TCP con mayores anchos de banda o latencias (producto de retraso de ancho de banda). De forma predeterminada, los equipos que ejecutan Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista se establecen en el nivel de rendimiento máximo.
  
La ventana de recepción TCP ha cambiado en Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista desde versiones anteriores de Windows. Las versiones anteriores de Windows limitaban la ventana de recepción de TCP a un máximo de 64 kilobytes (KB), mientras que Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista dimensionan dinámicamente la ventana del lado de recepción hasta 16 megabytes (MB ). En el control de tráfico TCP entrante, puede controlar el nivel de rendimiento entrante estableciendo el valor máximo que puede alcanzar la ventana de recepción TCP. Los niveles corresponden a los siguientes valores máximos. 
  
|Nivel de rendimiento de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

El tamaño de la ventana real puede ser un valor igual o menor que el máximo, dependiendo de las condiciones de la red.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para establecer la ventana de recepción de TCP

1. En Editor de objetos de directiva de grupo, haga clic en **Directiva de equipo local**, en **configuración de Windows**, haga clic con el botón secundario en **Directiva de QoS**y, a continuación, haga clic en **Configuración avanzada de QoS**.
  
2. En **rendimiento de recepción de TCP**, seleccione **configurar el rendimiento de recepción TCP**y, a continuación, seleccione el nivel de rendimiento que desee.

3.  Vincule el GPO a la unidad organizativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Configuración avanzada de QoS: Invalidación de marcado de DSCP

La invalidación del marcado de DSCP restringe la capacidad de las aplicaciones de especificar, o "marcar", valores de DSCP distintos de los especificados en las directivas de QoS. Al especificar que las aplicaciones tienen permitido establecer valores de DSCP, las aplicaciones pueden establecer valores de DSCP distintos de cero. 

Al especificar **Ignore**, las aplicaciones que usan las API de QoS tendrán sus valores DSCP establecidos en cero y solo las directivas QoS pueden establecer valores de DSCP. 

De forma predeterminada, los equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista permiten a las aplicaciones especificar valores de DSCP; las aplicaciones y los dispositivos que no usan las API de QoS no se invalidan.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores de valor de DSCP y de multimedia inalámbrico

La [Alianza Wi-Fi](https://go.microsoft.com/fwlink/?LinkId=160769) ha establecido una certificación para la tecnología \(WMM\) de multimedia inalámbrica que define \(cuatro\) categorías de acceso WMM_AC para priorizar el tráfico de red transmitido en un\-Red inalámbrica de fi. Las categorías de acceso \(incluyen en orden de prioridad\)más alta a mínima: voz, vídeo, mejor esfuerzo y fondo; se abrevian respectivamente como VO, VI, ser y BK. La especificación de WMM define qué valores de DSCP se corresponden con cada una de las cuatro categorías de acceso:
  
|Valor de DSCP|Categoría de acceso de WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Mejor esfuerzo (ser)|
|8-23|Background (BK)|

Puede crear directivas de QoS que usen estos valores de DSCP para asegurarse de que los equipos\-portátiles con certificados Wi-Fi™ para los adaptadores inalámbricos de WMM reciban\-un control de prioridades cuando estén asociados con los puntos de acceso de la certificación Wi-Fi para WMM.
  
### <a name="BKMK_precedencerules"></a>Reglas de prioridad de la directiva QoS

De forma similar a las prioridades de los GPO, las directivas de QoS tienen reglas de prioridad para resolver conflictos cuando se aplican varias directivas de QoS a un conjunto específico de tráfico. En el tráfico TCP o UDP saliente, solo se puede aplicar una directiva de QoS a la vez, lo que significa que las directivas QoS no tienen un efecto acumulativo, como cuando se suman las tasas de limitación.

En general, la Directiva de QoS con las condiciones más coincidentes gana. Cuando se aplican varias directivas de QoS, las reglas se dividen en tres categorías: nivel de usuario frente a nivel de equipo; aplicación frente a la red quíntuplo; y entre la red quíntuplo.

Por *quíntuplo de red*, nos referimos a la dirección IP de origen, la dirección IP de destino, el puerto \(de origen,\)el puerto de destino y el protocolo TCP/UDP.  

 **La directiva QoS de nivel de usuario tiene prioridad sobre la directiva QoS de nivel de equipo**

Esta regla facilita en gran medida la administración de GPO de QoS de los administradores de red, especialmente en el caso de las directivas basadas en grupos de usuarios. Por ejemplo, si el administrador de red desea definir una directiva de QoS para un grupo de usuarios, solo puede crear y distribuir un GPO a ese grupo. No tienen que preocuparse de los equipos en los que los usuarios inician sesión y de si esos equipos tendrán definidas directivas de QoS en conflicto, ya que, si existe un conflicto, la Directiva de nivel de usuario siempre tiene prioridad.

> [!NOTE]
>  Una directiva de QoS de nivel de usuario solo se aplica al tráfico generado por ese usuario. Otros usuarios de un equipo específico y el propio equipo no estarán sujetos a ninguna directiva de QoS definida para ese usuario.

 **Especificidad de la aplicación y tener prioridad sobre la red quíntuplo**

Cuando varias directivas de QoS coinciden con el tráfico específico, se aplica la directiva más específica. Entre las directivas que identifican las aplicaciones, una directiva que incluye la ruta de acceso del archivo de la aplicación de envío se considera más específica que otra directiva que solo identifica el nombre de la aplicación (sin ruta de acceso). Si todavía se aplican varias directivas a las aplicaciones, las reglas de prioridad usan el quíntuplo de red para encontrar la mejor coincidencia.

Como alternativa, se pueden aplicar varias directivas de QoS al mismo tráfico especificando condiciones no superpuestas. Entre las condiciones de las aplicaciones y el quíntuplo de red, la Directiva que especifica la aplicación se considera más específica y se aplica. 

Por ejemplo, policy_A solo especifica un nombre de aplicación (App. exe) y policy_B especifica la dirección IP de destino 192.168.1.0/24. Cuando este conflicto \(de directivas de QoS envía el tráfico a una dirección IP dentro del intervalo de 192.168.4.0/24\), se aplica policy_A.

 **Una mayor especificidad tiene prioridad en el quíntuplo de red**

En el caso de conflictos de directivas dentro de la red quíntuplo, la Directiva con las condiciones más coincidentes tiene prioridad. Por ejemplo, supongamos que policy_C especifica la dirección IP de origen "any", la dirección IP de destino 10.0.0.1, el puerto de origen "any", el puerto de destino "any" y el protocolo "TCP". 

A continuación, supongamos que policy_D especifica la dirección IP de origen "any", la dirección IP de destino 10.0.0.1, el puerto de origen "any", el puerto de destino 80 y el protocolo "TCP". Después, policy_C y policy_D coinciden con las conexiones con el destino 10.0.0.1:80. Dado que la directiva QoS aplica la Directiva con las condiciones de coincidencia más específicas, policy_D tiene prioridad en este ejemplo.  
  
Sin embargo, las directivas QoS pueden tener un número igual de condiciones. Por ejemplo, varias directivas pueden especificar solo una parte de la red quíntuplo (pero no la misma). Entre el quíntuplo de red, el orden siguiente es de mayor a menor prioridad:

- Dirección IP de origen

- Dirección IP de destino

- Puerto de origen

- Puerto de destino

- Protocolo (TCP o UDP)

Dentro de una condición específica, como la dirección IP, se trata una dirección IP más específica con mayor precedencia; por ejemplo, una dirección IP 192.168.4.1 es más específica que 192.168.4.0/24.

Diseñe las directivas de QoS lo más específicamente posible para simplificar la capacidad de su organización de comprender qué directivas están en vigor.

Para el siguiente tema de esta guía, vea [eventos y errores](qos-policy-errors.md)de la Directiva de QoS.

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).
