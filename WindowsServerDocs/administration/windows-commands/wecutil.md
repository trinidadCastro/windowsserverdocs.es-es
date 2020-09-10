---
title: wecutil
description: Artículo de referencia de wecutil, que le permite crear y administrar suscripciones a eventos que se reenvían desde equipos remotos.
ms.topic: reference
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: fbf236082b710ef5f4319b1924856fe98784b1ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641247"
---
# <a name="wecutil"></a>wecutil



Permite crear y administrar suscripciones a eventos que se reenvían desde equipos remotos. El equipo remoto debe ser compatible con el protocolo WS-Management.


## <a name="syntax"></a>Sintaxis

```
wecutil  [{es | enum-subscription}]
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]]
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]]
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]]
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]]
[{ds | delete-subscription} <Subid>]
[{rs | retry-subscription} <Subid> [<Eventsource>…]]
[{qc | quick-config} [/q:[<Quiet>]]].
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{es \| enum-subscription}|Muestra los nombres de todas las suscripciones de eventos remotos que existen.|
|{GS \| Get-subscription} \<Subid> [/f: \<Format> ] [/UNI: \<Unicode> ]|Muestra información de configuración de la suscripción remota. \<Subid> es una cadena que identifica de forma única una suscripción. \<Subid> es igual que la cadena que se especificó en la \<SubscriptionId> etiqueta del archivo de configuración XML, que se usó para crear la suscripción.|
|{GR \| Get-subscriptionruntimestatus} \<Subid> [ \<Eventsource> ...]|Muestra el estado de tiempo de ejecución de una suscripción. \<Subid> es una cadena que identifica de forma única una suscripción. \<Subid> es igual que la cadena que se especificó en la \<SubscriptionId> etiqueta del archivo de configuración XML, que se usó para crear la suscripción. \<Eventsource> es una cadena que identifica un equipo que actúa como origen de eventos. \<Eventsource> debe ser un nombre de dominio completo, un nombre NetBIOS o una dirección IP.|
|{SS \| set-subscription} \<Subid> [/e: [ \<Subenabled> ]] [/esa: \<Address> ] [/ese: [ \<Srcenabled> ]] [/AES] [/RES] [/un: \<Username> ] [/up: \<Password> ] [/d: \<Desc> ] [/URI: \<Uri> ] [/cm: \<Configmode> ] [/ex: \<Expires> ] [/q: \<Query> ] [/dia: \<Dialect> ] [/TN: \<Transportname> ] [/TP: \<Transportport> ] [/DM: \<Deliverymode> ] [/DMI: \<Deliverymax> ] [/dmlt: \<Deliverytime> ] [/HI: \<Heartbeat> ] [/CF: \<Content> ] [/l: \<Locale> ] [/ree: [ \<Readexist> ]] [/LF: \<Logfile> ] [/PN: \<Publishername> ] [/ESSP: \<Enableport> ] [/HN: \<Hostname> ] [/CT: \<Type> ]</br>or</br>{SS \| set-subscription/c: \<Configfile> [/cun: \<Comusername> /Cup: \<Compassword> ]|Cambia la configuración de la suscripción. Puede especificar el identificador de suscripción y las opciones adecuadas para cambiar los parámetros de suscripción, o puede especificar un archivo de configuración XML para cambiar los parámetros de suscripción.|
|{CS \| Create-subscription} \<Configfile> [/cun: \<Username> /Cup: \<Password> ]|Crea una suscripción remota. \<Configfile> Especifica la ruta de acceso al archivo XML que contiene la configuración de la suscripción. La ruta de acceso puede ser absoluta o relativa al directorio actual.|
|{ \| eliminación de suscripción de DS} \<Subid>|Elimina una suscripción y cancela las suscripciones de todos los orígenes de eventos que entregan eventos en el registro de eventos de la suscripción. Los eventos que ya se hayan recibido y registrado no se eliminarán. \<Subid> es una cadena que identifica de forma única una suscripción. \<Subid> es igual que la cadena que se especificó en la \<SubscriptionId> etiqueta del archivo de configuración XML, que se usó para crear la suscripción.|
|{ \| reintento de RS-subscription} \<Subid> [ \<Eventsource> ...]|Intenta establecer una conexión y enviar una solicitud de suscripción remota a una suscripción inactiva. Intenta reactivar todos los orígenes de eventos o los orígenes de eventos especificados. Los orígenes deshabilitados no se reintentan. \<Subid> es una cadena que identifica de forma única una suscripción. \<Subid> es igual que la cadena que se especificó en la \<SubscriptionId> etiqueta del archivo de configuración XML, que se usó para crear la suscripción. \<Eventsource> es una cadena que identifica un equipo que actúa como origen de eventos. \<Eventsource> debe ser un nombre de dominio completo, un nombre NetBIOS o una dirección IP.|
|{ \| Configuración rápida de QC} [/q: [ \<Quiet> ]]|Configura el servicio Recopilador de eventos de Windows para asegurarse de que se puede crear y mantener una suscripción a través de reinicios. Esto incluye los pasos siguientes:</br>1. Habilite el canal de eventos reenviados si está deshabilitado.</br>2. establezca el servicio Recopilador de eventos de Windows para retrasar el inicio.</br>3. Inicie el servicio Recopilador de eventos de Windows si no se está ejecutando.|

## <a name="options"></a>Opciones

|Opción|Descripción|
|------|-----------|
|/f\<Format>|Especifica el formato de la información que se muestra. \<Format> puede ser XML o conciso. Si <Format> es XML, el resultado se muestra en formato XML. Si \<Format> es breve, el resultado se muestra en pares de nombre y valor. El valor predeterminado es conciso.|
|/c\<Configfile>|Especifica la ruta de acceso al archivo XML que contiene una configuración de suscripción. La ruta de acceso puede ser absoluta o relativa al directorio actual. Esta opción solo se puede usar con las opciones **/cun** y **/Cup** y es mutuamente excluyente con todas las demás opciones.|
|/e: [ \<Subenabled> ]|Habilita o deshabilita una suscripción. \<Subenabled> puede ser true o false. El valor predeterminado de esta opción es true.|
|See\<Address>|Especifica la dirección de un origen de eventos. \<Address> es una cadena que contiene un nombre de dominio completo, un nombre NetBIOS o una dirección IP, que identifica un equipo que actúa como origen de eventos. Esta opción debe usarse con las opciones **/ese**, **/AES**, **/res**o **/un** y **/up** .|
|/ese: [ \<Srcenabled> ]|Habilita o deshabilita un origen de eventos. \<Srcenabled> puede ser true o false. Esta opción solo se permite si se especifica la opción **/esa** . El valor predeterminado de esta opción es true.|
|/aes|Agrega el origen de eventos especificado por la opción **/esa** si aún no forma parte de la suscripción. Si la dirección especificada por la opción **/esa** ya forma parte de la suscripción, se registra un error. Esta opción solo se permite si se especifica la opción **/esa** .|
|/res|Quita el origen de eventos especificado por la opción **/esa** si ya forma parte de la suscripción. Si la dirección especificada por la opción **/esa** no forma parte de la suscripción, se registra un error. Esta opción solo se permite si se especifica la opción **/esa** .|
|CEPE\<Username>|Especifica la credencial de usuario que se va a usar con el origen de eventos especificado por la opción **/esa** . Esta opción solo se permite si se especifica la opción **/esa** .|
|/up\<Password>|Especifica la contraseña que corresponde a la credencial de usuario. Esta opción solo se permite si se especifica la opción **/un** .|
|/d.\<Desc>|Proporciona una descripción para la suscripción.|
|Uri\<Uri>|Especifica el tipo de los eventos consumidos por la suscripción. \<Uri> contiene una cadena de URI que se combina con la dirección del equipo de origen del evento para identificar de forma única el origen de los eventos. La cadena URI se usa para todas las direcciones de origen de eventos de la suscripción.|
|/cm\<Configmode>|Establece el modo de configuración. \<Configmode> puede ser una de las cadenas siguientes: normal, Custom, MinLatency o MinBandwidth. Los modos normal, MinLatency y MinBandwidth establecen el modo de entrega, los elementos máximos de entrega, el intervalo de latido y el tiempo máximo de latencia de entrega. Las opciones **/DM**, **/DMI**, **/HI** o **/dmlt** solo se pueden especificar si el modo de configuración está establecido en personalizado.|
|antiguo\<Expires>|Establece la hora a la que expira la suscripción. \<Expires> debe definirse en formato de fecha y hora XML estándar o ISO8601: AAAA-MM-ddThh: mm: SS [. SSS] [Z], donde T es el separador de hora y Z indica la hora UTC.|
|/q\<Query>|Especifica la cadena de consulta de la suscripción. El formato de \<Query> puede ser diferente para los distintos valores de URI y se aplica a todos los orígenes de la suscripción.|
|dia\<Dialect>|Define el dialecto que utiliza la cadena de consulta.|
|/TN\<Transportname>|Especifica el nombre del transporte que se utiliza para conectarse a un origen de eventos remoto.|
|/TP\<Transportport>|Establece el número de puerto utilizado por el transporte al conectarse a un origen de eventos remoto.|
|DM\<Deliverymode>|Especifica el modo de entrega. \<Deliverymode> puede ser de extracción o de inserción. Esta opción solo es válida si la opción **/cm** está establecida en Custom.|
|DMI\<Deliverymax>|Establece el número máximo de elementos para la entrega por lotes. Esta opción solo es válida si **/cm** está establecido en Custom.|
|/dmlt:\<Deliverytime>|Establece la latencia máxima en la entrega de un lote de eventos. \<Deliverytime> es el número de milisegundos. Esta opción solo es válida si **/cm** está establecido en Custom.|
|HI\<Heartbeat>|Define el intervalo de latido. \<Heartbeat> es el número de milisegundos. Esta opción solo es válida si **/cm** está establecido en Custom.|
|Nº\<Content>|Especifica el formato de los eventos que se devuelven. \<Content> puede ser Events o RenderedText. Cuando el valor es RenderedText, los eventos se devuelven con las cadenas localizadas (por ejemplo, la descripción del evento) asociadas al evento. El valor predeterminado es RenderedText.|
|/l:\<Locale>|Especifica la configuración regional para la entrega de las cadenas localizadas en formato RenderedText. \<Locale> es un identificador de idioma y de país o región, por ejemplo, EN-US. Esta opción solo es válida si la opción **/CF** está establecida en RenderedText.|
|/ree: [ \<Readexist> ]|Identifica los eventos que se entregan para la suscripción. \<Readexist> puede ser true o false. Cuando el <Readexist> valor de es true, todos los eventos existentes se leen desde los orígenes de eventos de suscripción. Cuando <Readexist> es false, solo se entregan los eventos futuros (llegados). El valor predeterminado es true para una opción **/REE** sin un valor. Si no se especifica ninguna opción **/REE** , el valor predeterminado es false.|
|/LF\<Logfile>|Especifica el registro de eventos local que se usa para almacenar los eventos recibidos de los orígenes de eventos.|
|/PN\<Publishername>|Especifica el nombre del publicador. Debe ser un publicador que posea o importe el registro especificado por la opción **/LF** .|
|/essp:\<Enableport>|Especifica que el número de puerto debe anexarse al nombre de entidad de seguridad de servicio del servicio remoto. \<Enableport> puede ser true o false. El número de puerto se anexa cuando <Enableport> es true. Cuando se anexa el número de puerto, puede que sea necesario realizar alguna configuración para evitar que se deniegue el acceso a los orígenes de eventos.|
|HN\<Hostname>|Especifica el nombre DNS del equipo local. Este nombre lo utiliza el origen de eventos remotos para devolver eventos y debe usarse solo para una suscripción de extracción.|
|/CT\<Type>|Establece el tipo de credencial para el acceso al origen remoto. \<Type> debe ser uno de los siguientes valores: default, Negotiate, Digest, Basic o LocalMachine. El valor predeterminado es default.|
|/cun:\<Comusername>|Establece la credencial de usuario compartido que se va a usar para los orígenes de eventos que no tienen sus propias credenciales de usuario. Si se especifica esta opción con la opción **/c** , se omite la configuración de nombre de usuario y UserPassword para los orígenes de eventos individuales del archivo de configuración. Si desea usar una credencial diferente para un origen de eventos específico, debe invalidar este valor especificando las opciones **/un** y **/up** para un origen de eventos específico en la línea de comandos de otro comando **SS** .|
|Cup\<Compassword>|Establece la contraseña de usuario para la credencial de usuario compartido. Cuando \<Compassword> se establece en * (asterisco), la contraseña se lee desde la consola. Esta opción solo es válida cuando se especifica la opción **/cun** .|
|/q: [ \<Quiet> ]|Especifica si el procedimiento de configuración solicita confirmación. \<Quiet> puede ser true o false. Si <Quiet> es true, el procedimiento de configuración no solicita confirmación. El valor predeterminado de esta opción es false.|

## <a name="remarks"></a>Observaciones

> [!IMPORTANT]
> Si recibe el mensaje "el servidor RPC no está disponible? al intentar ejecutar wecutil, debe iniciar el servicio Recopilador de eventos de Windows (wecsvc). Para iniciar wecsvc, en un símbolo del sistema con privilegios elevados, escriba net start wecsvc.

- Para mostrar el contenido de un archivo de configuración:
  ```
  <Subscription xmlns=https://schemas.microsoft.com/2006/03/windows/events/subscription>
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path=Application>
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled=true>
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language=EN-US></Locale>
  </Subscription>
  ```

## <a name="examples"></a>Ejemplos

Información de configuración de salida de una suscripción denominada Sub1:
```
wecutil gs sub1
```
Salida de ejemplo:
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
Mostrar el estado de tiempo de ejecución de una suscripción denominada Sub1:
```
wecutil gr sub1
```
Actualice la configuración de la suscripción denominada Sub1 desde un nuevo archivo XML denominado WsSelRg2.xml:
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Actualice la configuración de suscripción denominada sub2 con varios parámetros:
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Cree una suscripción para reenviar eventos desde un registro de eventos de aplicación de Windows Vista de un equipo remoto en mySource.myDomain.com al registro de eventos reenviados (consulte la sección Comentarios para obtener un ejemplo de un archivo de configuración):
```
wecutil cs subscription.xml
```
Elimine una suscripción denominada Sub1:
```
wecutil ds sub1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
