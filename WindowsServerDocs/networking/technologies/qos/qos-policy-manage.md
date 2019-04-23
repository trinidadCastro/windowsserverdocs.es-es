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
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851676"
---
# <a name="manage-qos-policy"></a>Administrar la directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre cómo usar al Asistente para directivas de QoS para crear, editar o eliminar una directiva de QoS.

>[!NOTE]
>  Además de este tema, está disponible la siguiente documentación de administración de directivas de QoS.
> 
>  - [Errores y eventos de directiva de QoS](qos-policy-errors.md)

En los sistemas operativos de Windows, directiva de QoS combina la funcionalidad de QoS basada en estándares con la capacidad de administración de directivas de grupo. Hace que la configuración de esta combinación facilita la aplicación de directivas de QoS a objetos de directiva de grupo. Windows incluyen un Asistente para la directiva de QoS para ayudarle a realizar las tareas siguientes.

-  [Crear una directiva QoS](#bkmk_createpolicy)

-  [Ver, editar o eliminar una directiva de QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Crear una directiva QoS

Antes de crear una directiva de QoS, es importante que comprenda los dos controles de calidad de servicio claves que se usan para administrar el tráfico de red:

- Valor de DSCP

-   Tasa de limitación

### <a name="prioritizing-traffic-with-dscp"></a>Dar prioridad al tráfico con DSCP

Como se indicó en el ejemplo anterior de la aplicación de línea de negocio, puede definir la prioridad del tráfico de red saliente con **especificar el valor de DSCP** para configurar una directiva de QoS con un valor específico DSCP. 

Como se describe en RFC 2474, DSCP permite especificar valores de 0 a 63 especificarse en el campo TOS de un paquete IPv4 y en el campo de clase de tráfico de IPv6. Enrutadores de red usan el valor DSCP para clasificar los paquetes de red y que ponerlos en cola correctamente.
  
> [!NOTE]
>  De forma predeterminada, el tráfico de Windows tiene un valor DSCP de 0.
  
El número de colas y su comportamiento de asignación de prioridades deben diseñarse como parte de la estrategia de QoS de la organización. Por ejemplo, su organización puede tener cinco colas: tráfico susceptible a latencia, tráfico de control, tráfico crítico para la empresa, tráfico de mejor esfuerzo y tráfico de transferencia de datos de forma masiva.  
  
### <a name="throttling-traffic"></a>Limitar el tráfico

Junto con los valores DSCP, la limitación es otro control de clave para la administración de ancho de banda de red. Como se mencionó anteriormente, puede usar el **especificar velocidad del Acelerador** opción para configurar una directiva de QoS con una velocidad del Acelerador específico para el tráfico saliente. Mediante el uso de limitación, una directiva de QoS limita el tráfico de red saliente a una velocidad del acelerador especificado. Tanto el límite como el marcado de DSCP se pueden utilizar en conjunto para administrar el tráfico de manera eficaz.

>[!NOTE]
>De forma predeterminada, la casilla **Especificar velocidad del acelerador** no está activada.

Para crear una directiva de QoS, edite la configuración de un objeto de directiva de grupo (GPO) desde dentro de la herramienta de administración de directivas de grupo (GPMC). GPMC, a continuación, se abre el Editor de objetos de directiva de grupo.

Los nombres de las directivas QoS deben ser exclusivos. Cómo se aplican las directivas a los servidores y los usuarios finales depende de la directiva de QoS se guarda en el Editor de objetos de directiva de grupo:

- En la directiva de equipo equipo\Configuración Settings\QoS una directiva de QoS se aplica a equipos, independientemente del usuario que ha iniciado sesión actualmente. Por lo general, se utilizan directivas Qos basadas en equipos para los equipos de servidores.

- Una directiva de QoS en la directiva de usuario equipo\Configuración Settings\QoS se aplica a los usuarios una vez que hayan iniciado sesión, independientemente del equipo que iniciaron sesión en.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Para crear una nueva directiva de QoS con el Asistente para directivas de QoS

-   En el Editor de objetos de directiva de grupo, haga clic en cualquiera de los **directiva QoS** nodos y, a continuación, haga clic en **crear una nueva directiva**.

### <a name="wizard-page-1---policy-profile"></a>1 - perfil de directiva de página del Asistente

En la primera página del Asistente para la directiva de QoS, puede especificar un nombre de directiva y configurar cómo controla el tráfico de red saliente QoS.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Para configurar la página Perfil de directiva del Asistente para directiva basada en QoS

1. En **Nombre de directiva**, escriba un nombre para la directiva QoS. El nombre debe identificar exclusivamente la directiva.

2. Opcionalmente, use **especificar el valor de DSCP** para habilitar el marcado de DSCP y, a continuación, configure un valor de DSCP entre 0 y 63.

3. De forma opcional, use **Especificar velocidad del acelerador** para habilitar el límite de tráfico y configurar la velocidad del acelerador. El valor de la velocidad del acelerador debe ser mayor que 1 y puede especificar unidades de kilobytes por segundo \(KBps\) o megabytes por segundo \(MBps\).

4. Haz clic en **Siguiente**.

### <a name="wizard-page-2---application-name"></a>Página del asistente 2 - nombre de la aplicación

En la segunda página del Asistente para la directiva de QoS se puede aplicar la directiva a todas las aplicaciones, a una aplicación específica según se identifica por su nombre del archivo ejecutable, para una ruta de acceso y nombre de la aplicación, así como las aplicaciones de servidor HTTP que controlan las solicitudes para una dirección URL específica.

- **Todas las aplicaciones** especifica que la configuración de administración de tráfico de la primera página del Asistente para la directiva de QoS se aplica a todas las aplicaciones.

- **Sólo las aplicaciones con este nombre ejecutable** especifica que la configuración de administración de tráfico de la primera página del Asistente para la directiva de QoS para una aplicación específica. El nombre del archivo ejecutable debe terminar con la extensión de nombre de archivo .exe.

- **Solo aplicaciones de servidor HTTP responde a las solicitudes para esta dirección URL** especifica que la configuración de administración de tráfico de la primera página del Asistente para la directiva de QoS se aplica a determinadas aplicaciones de servidor HTTP solo.

Opcionalmente, puede especificar la ruta de acceso de la aplicación. Para especificar la ruta de acceso de una aplicación, incluya la ruta de acceso con el nombre de la aplicación. La ruta de acceso puede incluir variables de entorno. Por ejemplo, %ProgramFiles%\ruta de acceso de aplicación\MyApp.exe, o c:\archivos de programa\ruta de acceso de aplicación\myapp.exe.

>[!NOTE]
>La ruta de acceso de la aplicación no puede incluir una ruta de acceso que se resuelve en un vínculo simbólico.

La dirección URL debe ajustarse a [RFC 1738](https://tools.ietf.org/html/rfc1738), en forma de `http[s]://<hostname\>:<port\>/<url-path>`. Puede usar un carácter comodín, `‘*’`, para `<hostname>` o `<port>`, por ejemplo, `https://training.\*/, https://\*.\*`, pero el carácter comodín no puede indicar una subcadena de `<hostname>` o `<port>`.

En otras palabras, ninguna de ellas `https://my\*site/` ni `https://\*training\*/` es válido. 

Si lo desea, puede comprobar **incluir subdirectorios y archivos** para buscar coincidencias en todos los subdirectorios y archivos después de una dirección URL. Por ejemplo, si se activa esta opción y la dirección URL es `https://training`, directiva de QoS se tendrá en cuenta las solicitudes de` https://training/video` una coincidencia acertada.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Para configurar la página Nombre de la aplicación del Asistente para la directiva QoS

1. En **esta directiva de QoS se aplica a**, seleccione **todas las aplicaciones** o **sólo las aplicaciones con este nombre de archivo ejecutable**.

2. Si selecciona **Sólo las aplicaciones con el siguiente nombre de archivo ejecutable**, especifique un archivo ejecutable que termine con la extensión de nombre de archivo .exe.

3. Haz clic en **Siguiente**.

### <a name="wizard-page-3---ip-addresses"></a>Página del asistente 3 - direcciones IP

En la tercera página del Asistente para la directiva de QoS puede especificar las condiciones de dirección IP para la directiva de QoS, incluidas las siguientes:

- Todas las direcciones IPv4 o IPv6 de origen o direcciones IPv4 o IPv6 de orígenes específicos

- Todas las direcciones IPv4 o IPv6 de destino o direcciones IPv4 o IPv6 de destino específico

Si selecciona **Sólo para el siguiente prefijo o dirección IP de origen:** o **Sólo para el siguiente prefijo o dirección IP de destino:**, debe escribir una de las siguientes opciones:

- Dirección IPv4, como `192.168.1.1`

- Un prefijo de dirección IPv4 con notación de longitud de prefijo de red, como `192.168.1.0/24`

- Dirección IPv6, como `3ffe:ffff::1`

- IPv6, como prefijo, de dirección `3ffe:ffff::/48`

Si selecciona ambos **solo para la siguiente dirección IP de origen** y **solo para la siguiente dirección IP de destino**, direcciones o prefijos de direcciones deben ser ya sea IPv4 o IPv6-basado en.

Si especifica la dirección URL para aplicaciones de servidor HTTP en la página anterior del asistente, observará que la dirección IP de origen para la directiva de QoS en esta página del asistente está atenuada. 

Esto es cierto porque la dirección IP de origen es la dirección del servidor HTTP y no se puede configurar aquí. Por otro lado, se puede personalizar la directiva especificando la dirección IP de destino. Esto permite crear directivas diferentes para distintos clientes mediante el uso de las mismas aplicaciones de servidor HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Para configurar la página de direcciones IP del Asistente para la directiva QoS

1. En **esta directiva de QoS se aplica a** (origen), seleccione **cualquier dirección IP de origen** o **solo para la siguiente dirección IP de la dirección de origen**.

2. Si seleccionó **sólo la siguiente dirección IP de origen**, especifique una dirección IPv4 o IPv6 o prefijo.

3. En **esta directiva de QoS se aplica a** (destino), seleccione **cualquier dirección de destino** o **solo para la siguiente dirección IP de destino.**

4. Si seleccionó **solo para la siguiente dirección IP de destino**, especifique una dirección IPv4 o IPv6 o un prefijo que se corresponde con el tipo de dirección o prefijo especificado para la dirección de origen.

5.  Haz clic en **Siguiente**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Página 4: protocolos y puertos del Asistente

En la cuarta página del Asistente para la directiva de QoS, puede especificar los tipos de tráfico y los puertos que están controlados por la configuración en la primera página del asistente. Se puede especificar:  
-   Tráfico TCP, tráfico UDP o ambos  

-   Todos los puertos de origen, un intervalo de puertos de origen o un puerto de origen específico

-   Todos los puertos de destino, un intervalo de puertos de destino o un puerto de destino específico  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Para configurar la página protocolos y puertos del Asistente para la directiva QoS

1. En **Seleccione el protocolo para el que se aplica esta directiva de QoS**, seleccione **TCP**, **UDP** o **TCP y  UDP**.

2. En **Especifique el número de puerto de origen**, seleccione **Cualquier puerto de origen** o **Desde este intervalo o número de puerto de origen**.

3. Si seleccionó **desde este número de puerto de origen**, escriba un número de puerto entre 1 y 65535.

     Si lo desea, puede especificar un intervalo de puertos, en el formato de "*baja*:*alta*," donde *baja* y *alta* representan los límites inferiores y los límites superiores del puerto de intervalo, ambos inclusive. *Baja* y *alta* cada uno debe ser un número entre 1 y 65535. No se permite ningún espacio entre el carácter de dos puntos (:) y los números.

4. En **Especifique el número de puerto de destino**, seleccione **A cualquier puerto de destino** o **A este intervalo o número de puerto de destino**.

5. Si seleccionó **A este intervalo o número de puerto de destino** en el paso anterior, escriba un número de puerto entre 1 y 65535.

Para completar la creación de la nueva directiva de QoS, haga clic en **finalizar** en el **protocolos y puertos** página del Asistente para la directiva de QoS. Cuando haya completado, la nueva directiva de QoS se muestra en el panel de detalles del Editor de objetos de directiva de grupo.  
  
Para aplicar la configuración de directiva de QoS a usuarios o equipos, vincule el GPO en el que las directivas de QoS se encuentran en un contenedor de Active Directory Domain Services, como un dominio, un sitio o una unidad organizativa (OU).  
  
##  <a name="bkmk_editpolicy"></a>Ver, editar o eliminar una directiva de QoS

Las páginas del asistente se describe la directiva de QoS anteriormente corresponden a las páginas de propiedades que se muestran al ver o editar las propiedades de una directiva.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Para visualizar las propiedades de una directiva QoS  
  
-   Haga clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **propiedades**.  
  
     El Editor de objetos de directiva de grupo muestra la página de propiedades con las siguientes pestañas:  
  
    -   Perfil de directiva  
  
    -   Nombre de la aplicación  
  
    -   Direcciones IP  
  
    -   Protocolos y puertos  
  
### <a name="to-edit-a-qos-policy"></a>Para editar una directiva QoS  
  
-   Haga clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **Editar directiva existente**.  
  
     El Editor de objetos de directiva de grupo muestra la **editar una directiva QoS existente** cuadro de diálogo.  
  
### <a name="to-delete-a-qos-policy"></a>Para eliminar una directiva de QoS  
  
-   Haga clic en el nombre de la directiva en el panel de detalles del Editor de objetos de directiva de grupo y, a continuación, haga clic en **Eliminar directiva**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Informes de GPMC en la directiva QoS 

Después de haber aplicado una serie de directivas de QoS en toda la organización, puede ser útil o es necesario revisar periódicamente cómo se aplican las directivas. Un resumen de las directivas de QoS para un usuario o equipo específicos puede verse mediante el uso de informes de GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Para ejecutar al Asistente para resultados de directiva de grupo para un informe de las directivas de QoS  
  
-   En GPMC, haga clic en el **resultados de directivas de grupo** nodo y, a continuación, seleccione la opción del menú para **Asistente para resultados de directiva de grupo.**  
  
Después de generan los resultados de la directiva de grupo, haga clic en el **configuración** ficha. En el **configuración** ficha, las directivas de QoS se pueden encontrar en los nodos "Directiva de equipo equipo\Configuración Settings\QoS" y "Directiva de usuario equipo\Configuración Settings\QoS".  
  
En el **configuración** ficha, las directivas de QoS se enumeran por sus nombres de directiva de QoS con su valor de DSCP, velocidad del acelerador, condiciones de la directiva y GPO prevalente enumerados en la misma fila...  
  
La vista de resultados de directiva de grupo identifica de forma única el GPO ganador. Cuando varios GPO tienen directivas de QoS con el mismo nombre de directiva de QoS, se aplica el GPO con la prioridad más alta de GPO. Se trata de la GPO ganadora. En el conflicto de directivas de QoS (identificadas por el nombre de la directiva) que están conectadas a una prioridad inferior no se aplica el GPO. Tenga en cuenta que las prioridades de GPO definen qué directivas de QoS se implementan en el sitio, dominio o unidad organizativa, según corresponda. Después de la implementación en un nivel de usuario o equipo, el [las reglas de prioridad de directiva de QoS](#BKMK_precedencerules) determinar qué tráfico se permitidas y bloqueada.  
  
El valor de DSCP de la directiva de QoS, velocidad del acelerador y condiciones de la directiva también son visibles en el Editor de objetos de directiva de grupo (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Configuración avanzada para los usuarios móviles y remotos  
Con la directiva de QoS, el objetivo es administrar el tráfico de red de la empresa. En escenarios móviles, los usuarios pueden enviar tráfico o desactivar la red empresarial. Dado que las directivas de QoS no son relevantes mientras esté lejos de la red empresarial, se habilitan las directivas de QoS solo en interfaces de red que estén conectadas a la empresa para Windows 8, Windows 7 o Windows Vista.  
  
Por ejemplo, un usuario puede conectar su equipo portátil a su red empresarial a través de la red privada virtual (VPN) desde una cafetería. Para VPN, la interfaz de red física (por ejemplo, inalámbrico) no tendrán directivas QoS aplicadas. Sin embargo, la interfaz VPN tendrán directivas QoS aplicadas, porque se conecta a la empresa. Si el usuario escribe más adelante otra red empresarial que no tiene una relación de confianza de AD DS, las directivas de QoS no se habilitará.  
  
Tenga en cuenta que estos escenarios móviles no se aplican a las cargas de trabajo de servidor. Por ejemplo, un servidor con varios adaptadores de red podría permanecer en el borde de una red empresarial. El departamento de TI puede optar por tener la limitación de las directivas QoS tráfico que sale de la empresa; Sin embargo, este adaptador de red que envía el tráfico de salida no necesariamente se conecta a la red empresarial. Por este motivo, las directivas de QoS siempre están habilitadas en todas las interfaces de red de un equipo que ejecuta Windows Server 2012.  
  
> [!NOTE]
>  Habilitación selectiva solo se aplica a las directivas de QoS y no a la configuración avanzada de QoS que se describe a continuación en este documento.  
  
### <a name="advanced-qos-settings"></a>Configuración avanzada de QoS

Configuración avanzada de QoS proporciona controles adicionales para los administradores de TI a administrar el consumo de red de equipo y marcados de DSCP. Configuración avanzada de QoS se aplica sólo en el nivel de equipo, mientras que las directivas de QoS pueden aplicarse en los niveles de equipo y usuario.

#### <a name="to-configure-advanced-qos-settings"></a>Para configurar la configuración avanzada de QoS

1.  Haga clic en **configuración del equipo**y, a continuación, haga clic en **configuración de Windows en la directiva de grupo**.
  
2.  Haga clic en **directiva QoS**y, a continuación, haga clic en **configuración avanzada de QoS**.

     La siguiente ilustración muestra que los dos avanzada fichas de configuración de QoS: **El tráfico TCP entrante** y **invalidación de marcado de DSCP**.
  
> [!NOTE]
>  Configuración avanzada de QoS son la configuración de directiva de grupo de nivel de equipo.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Configuración de QoS avanzada: el tráfico TCP entrante

**El tráfico TCP entrante** controla el consumo de ancho de banda TCP en el lado del receptor, mientras que el tráfico saliente de TCP y UDP de afectar a las directivas de QoS. 

Estableciendo un rendimiento más bajo nivel en el **tráfico de entrada TCP** ficha, TCP, se limitará el tamaño de su TCP anunciado la ventana de recepción. El efecto de esta configuración se las tasas de mayor rendimiento y uso de las conexiones TCP con mayores anchos de banda o latencias (producto del retraso de ancho de banda) vínculos. De forma predeterminada, los equipos que ejecutan Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista se establecen en el nivel de rendimiento máximo.
  
La recepción de TCP ventana ha cambiado en Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista desde versiones anteriores de Windows. Las versiones anteriores de Windows limita la ventana de recepción TCP para un máximo de 64 kilobytes (KB), mientras que Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista dinámicamente tamaño de la ventana de recepción hasta 16 megabytes (MB ). En el control de tráfico TCP entrante, puede controlar el nivel de rendimiento entrante estableciendo el valor máximo que puede alcanzar la ventana de recepción de TCP. Los niveles se corresponden con los siguientes valores máximos. 
  
|Nivel de rendimiento de entrada|Máximo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

El tamaño de ventana real puede ser un valor igual o menor que el máximo, en función de las condiciones de red.

###### <a name="to-set-the-tcp-receive-side-window"></a>Para establecer la ventana de recepción TCP

1. En el Editor de objetos de directiva de grupo, haga clic en **directiva de equipo Local**, haga clic en **Windows configuración**, haga clic en **directiva QoS**y, a continuación, haga clic en **avanzada de QoS Configuración de**.
  
2. En **el rendimiento de recepción TCP**, seleccione **configurar el rendimiento de recepción de TCP**y, a continuación, seleccione el nivel de rendimiento que desee.

3.  Vincule el GPO a la unidad organizativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Configuración avanzada de QoS: Invalidación de marcado de DSCP

Invalidación de marcado de DSCP restringe la capacidad de especificar las aplicaciones, o "mark" — DSCP valores distintos de los especificados en las directivas de QoS. Al especificar que las aplicaciones pueden establecer valores DSCP, las aplicaciones pueden establecer los valores DSCP distinto de cero. 

Mediante la especificación de **omitir**, las aplicaciones que usan QoS APIs tendrán sus valores DSCP se establece en cero y solo las directivas de QoS pueden establecer valores DSCP. 

De forma predeterminada, los equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista permiten a las aplicaciones especificar los valores DSCP; las aplicaciones y dispositivos que no utilizan las APIs QoS no se reemplaza.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valores DSCP y Multimedia inalámbrica

El [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) ha establecido una certificación para Multimedia inalámbricas \(WMM\) que define cuatro categorías de acceso \(WMM_AC\) para dar prioridad al tráfico de red transmite en un Wi\-Fi de red inalámbrica. Las categorías de acceso incluyen \(en orden de mayor a menor prioridad\): voz, vídeo, mejor esfuerzo y en segundo plano; respectivamente abreviar como "bk", BE, VI y VO. La especificación de WMM define qué DSCP valores se corresponden con cada una de las categorías de acceso de cuatro:
  
|Valor de DSCP|Categoría de acceso WMM|
|----------|-------------------|
|48-63|Voz (VO)|
|32-47|Vídeo (VI)|
|24-31, 0-7|Mejor esfuerzo (BE)|
|8-23|En segundo plano (negro)|

Puede crear directivas de QoS que usan estos valores DSCP para asegurarse de que portable equipos con Wi\-Fi Certified™ para los adaptadores inalámbricos WMM reciben prioridad control cuando se asocia con Wi\-Fi certificada para WMM puntos de acceso.
  
### <a name="BKMK_precedencerules"></a>Reglas de prioridad de directiva de QoS

Al igual que las prioridades del GPO, las directivas de QoS tienen reglas de prioridad para resolver los conflictos cuando varias directivas de QoS se aplican a un conjunto específico de tráfico. Para el tráfico TCP o UDP saliente, sólo una directiva de QoS puede aplicarse a la vez, lo que significa que las directivas de QoS no tienen un efecto acumulativo, por ejemplo, donde se deben sumar las tarifas del acelerador.

En general, la directiva de QoS con condiciones de búsqueda de coincidencias más wins. Cuando se aplican varias directivas de QoS, las reglas se dividen en tres categorías: nivel de usuario frente a nivel de equipo; aplicación frente a la quíntuplo red; y entre la red quintuple.

Por *red quíntuplo*, nos referimos a la dirección IP de origen, dirección IP de destino, puerto de origen, puerto de destino y protocolo \(TCP/UDP\).  

 **Directiva de QoS de nivel de usuario tiene prioridad sobre la directiva QoS de nivel de equipo**

Esta regla facilita enormemente la administración de los administradores de red de los GPO de QoS, especialmente para las directivas de usuario basada en grupo. Por ejemplo, si el Administrador de red desea definir una directiva de QoS para un grupo de usuarios, solo puede crear y distribuir un GPO a ese grupo. No tienen que preocuparse acerca de los equipos que los usuarios inician sesión en y si esos equipos tendrá las directivas de QoS en conflicto definidas, porque, si existe un conflicto, la directiva de nivel de usuario siempre tiene prioridad.

> [!NOTE]
>  Una directiva de QoS de nivel de usuario solo es aplicable para el tráfico generado por ese usuario. Otros usuarios de un equipo específico y el propio equipo, no estará sujeto a las directivas de QoS que se definen para que el usuario.

 **Especificidad de la aplicación y tienen preferencia sobre quíntuplo de red**

Cuando varias directivas de QoS coincide con el tráfico específico, se aplica la directiva más específica. Las directivas que identifican las aplicaciones, una directiva que incluye la ruta de acceso de archivo de la aplicación de envío se considera más específica que otra directiva que solo identifica el nombre de la aplicación (sin ruta). Si se siguen aplican varias directivas con las aplicaciones, las reglas de precedencia usan la red quíntuplo para buscar a la mejor coincidencia.

Como alternativa, pueden aplicar varias directivas de QoS para el mismo tráfico especificando las condiciones no se superpongan. Entre las condiciones de las aplicaciones y el quíntuplo de red, la directiva que especifica la aplicación se considera más específica y se aplica. 

Por ejemplo, policy_A solo especifica un nombre de aplicación (app.exe) y policy_B especifica el destino IP direcciones 192.168.1.0/24. Cuando estas directivas QoS entran en conflicto \(app.exe envía tráfico a una dirección IP dentro del intervalo de 192.168.4.0/24\), policy_A se aplica.

 **Más especificidad prevalece dentro de la red quíntuplo**

Conflictos de directivas dentro de la quíntuplo de red, la directiva con las condiciones más parecidas tiene prioridad. Por ejemplo, suponga policy_C especifica la dirección IP de origen "any", dirección IP 10.0.0.1 de destino, puerto de origen "cualquiera", destino "cualquiera" de puerto y protocolo "TCP". 

A continuación, imagine policy_D especifica "any" dirección IP de origen, IP de destino "cualquiera" puerto 80 de destino de direcciones de 10.0.0.1, puerto de origen y protocolo "TCP". A continuación, policy_C y policy_D coincide con las conexiones a 10.0.0.1: 80 de destino. Porque la directiva de QoS se aplica la directiva con las condiciones de coincidencias más específicas, policy_D tiene prioridad en este ejemplo.  
  
Sin embargo, las directivas de QoS podrían tener el mismo número de condiciones. Por ejemplo, varias directivas es posible que cada uno especifica solo una (pero no es el mismo) parte de la quíntuplo de red. Entre lo quíntuplo de red, el orden siguiente es de mayor a menor precedencia:

- Dirección IP de origen

- Dirección IP de destino

- Puerto de origen

- Puerto de destino

- Protocolo (TCP o UDP)

Dentro de una condición específica, como la dirección IP, una dirección IP más específica se trata con mayor prioridad; Por ejemplo, una dirección IP 192.168.4.1 es más específica que 192.168.4.0/24.

Diseñar las directivas de QoS como más específica posible para simplificar la capacidad de su organización para comprender las directivas que están en vigor.

El siguiente tema en esta guía, consulte [errores y eventos de directiva de QoS](qos-policy-errors.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).
