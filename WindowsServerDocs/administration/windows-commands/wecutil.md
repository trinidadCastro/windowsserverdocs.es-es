---
title: wecutil
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 04fb86d813049dc5f0aa6d4fba51e45dccbd1b80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440183"
---
# <a name="wecutil"></a>wecutil



Permite crear y administrar las suscripciones a eventos que se reenvían desde equipos remotos. El equipo remoto debe admitir el protocolo WS-Management. Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).


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

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{es \| enum-subscription}|Muestra los nombres de todas las suscripciones de eventos remotos que existen.|
|{gs \| get-subscription} \<Subid > [/ f:\<formato >] [/ uni:\<Unicode >]|Muestra información de configuración de suscripción remoto. \<Subid > es una cadena que identifica una suscripción. \<Subid > es el mismo que la cadena que se especificó en el \<SubscriptionId > etiqueta del archivo de configuración XML, que se usó para crear la suscripción.|
|{gr \| get-subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|Muestra el estado en tiempo de ejecución de una suscripción. \<Subid > es una cadena que identifica una suscripción. \<Subid > es el mismo que la cadena que se especificó en el \<SubscriptionId > etiqueta del archivo de configuración XML, que se usó para crear la suscripción. \<EventSource > es una cadena que identifica un equipo que actúa como un origen de eventos. \<EventSource > debe ser un nombre de dominio completo, un nombre NetBIOS o una dirección IP.|
|{ss \| set-subscription} \<Subid > [/ e: [\<Subenabled >]] [/ SEC:\<dirección >] [/ ese: [\<Srcenabled >]] [/aes] / [res] [/ anular:\<Username >] [/ hasta:\< Contraseña >] [/ d:\<Desc >] [/ uri:\<Uri >] [/ cm:\<Configmode >] [/ p. ej.:\<Expires >] [/ p:\<consulta >] [/ dia:\<dialecto >] [/ tn:\< TransportName >] [/ tp:\<Transportport >] [/ dm:\<Deliverymode >] [/ dmi:\<Deliverymax >] [/ dmlt:\<Deliverytime >] [/ Hola:\<latido >] [/ cf:\< Contenido >] [/ l:\<configuración regional >] [/ ree: [\<Readexist >]] [/ lf:\<ArchivoDeRegistro >] [/ pn:\<Publishername >] [/ essp:\<Enableport >] [/ hn:\< Nombre de host >] [/ ct:\<tipo >]</br>o bien</br>{ss \| /c: conjunto suscripción\<Configfile > [/ cun:\<Comusername >/taza:\<Compassword >]|Cambia la configuración de la suscripción. Puede especificar el identificador de suscripción y las opciones adecuadas para cambiar los parámetros de suscripción, o puede especificar un archivo de configuración XML para cambiar los parámetros de suscripción.|
|{cs \| crear suscripción} \<Configfile > [/ cun:\<nombre de usuario >/taza:\<contraseña >]|Crea una suscripción remota. \<ConfigFile > especifica la ruta de acceso al archivo XML que contiene la configuración de la suscripción. La ruta de acceso puede ser absoluta o relativa al directorio actual.|
|{ds \| delete-subscription} \<Subid >|Elimina una suscripción y cancela la suscripción de todos los orígenes de eventos que entregan eventos en el registro de eventos para la suscripción. No se eliminan los eventos que ya ha recibido y ha iniciado. \<Subid > es una cadena que identifica una suscripción. \<Subid > es el mismo que la cadena que se especificó en el \<SubscriptionId > etiqueta del archivo de configuración XML, que se usó para crear la suscripción.|
|{rs \| reintento-subscription} \<Subid > [\<Eventsource >...]|Reintentos para establecer una conexión y enviar una solicitud de suscripción remoto a una suscripción inactiva. Intenta volver a activar todos los orígenes de eventos o especificar los orígenes de eventos. No se reintentan orígenes deshabilitados. \<Subid > es una cadena que identifica una suscripción. \<Subid > es el mismo que la cadena que se especificó en el \<SubscriptionId > etiqueta del archivo de configuración XML, que se usó para crear la suscripción. \<EventSource > es una cadena que identifica un equipo que actúa como un origen de eventos. \<EventSource > debe ser un nombre de dominio completo, un nombre NetBIOS o una dirección IP.|
|{qc \| quick-config} [/q:[\<Quiet>]]|Configura el servicio Recopilador de eventos de Windows para asegurarse de se puede crear una suscripción y sostenida a través de reinicios. Esto incluye los pasos siguientes:</br>1.  Habilite el canal de eventos reenviados si está deshabilitado.</br>2.  Configure el servicio de recopilador de eventos de Windows para retrasar el inicio.</br>3.  Inicie el servicio de recopilador de eventos de Windows si no se está ejecutando.|

## <a name="options"></a>Opciones

|Opción|Descripción|
|------|-----------|
|/ f:\<formato >|Especifica el formato de la información que se muestra. \<Formato > puede ser XML o XML. Si <Format> es XML, se muestra el resultado en formato XML. Si \<formato > es XML, se muestra el resultado en pares nombre / valor. El valor predeterminado es XML.|
|/ c:\<Configfile >|Especifica la ruta de acceso al archivo XML que contiene una configuración de la suscripción. La ruta de acceso puede ser absoluta o relativa al directorio actual. Esta opción solo se puede usar con el **/cun** y **/taza** opciones y es mutuamente excluyente con todas las demás opciones.|
|/ e: [\<Subenabled >]|Habilita o deshabilita una suscripción. \<Subenabled > puede ser true o false. El valor predeterminado de esta opción es true.|
|/esa:\<Address>|Especifica la dirección de un origen de eventos. \<Dirección > es una cadena que contiene un nombre de dominio completo, un nombre NetBIOS o una dirección IP, que identifica un equipo que actúa como un origen de eventos. Esta opción debe usarse con la **/ese**, **/aes**, **/res**, o **/un** y **/up** opciones.|
|/ese: [\<Srcenabled >]|Habilita o deshabilita un origen de eventos. \<Srcenabled > puede ser true o false. Esta opción solo se permite si el **/esa** se especifica la opción. El valor predeterminado de esta opción es true.|
|/AES|Agrega el origen del evento especificado por el **/esa** opción si ya no forma parte de la suscripción. Si la dirección especificada por el **/esa** opción ya forma parte de la suscripción, se notifica un error. Esta opción solo se permite si el **/esa** se especifica la opción.|
|/res|Quita el origen del evento especificado por el **/esa** opción si ya es una parte de la suscripción. Si la dirección especificada por el **/esa** opción no es una parte de la suscripción, se notifica un error. Esta opción solo se permite si **/esa** se especifica la opción.|
|/un:\<nombre de usuario >|Especifica la credencial de usuario para usar con el origen del evento especificado por el **/esa** opción. Esta opción solo se permite si el **/esa** se especifica la opción.|
|o arriba:\<contraseña >|Especifica la contraseña que se corresponde con la credencial de usuario. Esta opción solo se permite si el **/un** se especifica la opción.|
|/d:\<Desc>|Proporciona una descripción para la suscripción.|
|/uri:\<Uri>|Especifica el tipo de los eventos consumidos por la suscripción. \<URI > contiene una cadena URI que se combina con la dirección del equipo de origen de eventos para identificar el origen de los eventos. La cadena de URI se usa para todas las direcciones de origen de eventos en la suscripción.|
|/cm:\<Configmode >|Establece el modo de configuración. \<Configmode > puede ser una de las siguientes cadenas: Normal, personalizado, MinLatency o MinBandwidth. Los modos Normal, MinLatency y MinBandwidth establecer el modo de entrega, elementos de entrega máxima, intervalo de latido y tiempo máximo de latencia de entrega. El **/dm**, **/dmi**, **/hi** o **/dmlt** opciones solo se pueden especificar si el modo de configuración está establecido en personalizado.|
|/ p. ej.:\<expira >|Establece la hora de expiración de la suscripción. \<Expira > debe definirse en el formato de fecha y hora estándar de XML o ISO8601: aaaa-MM-ddTHH [.sss] [Z], donde T es el separador de hora y Z indica la hora UTC.|
|/ q:\<consulta >|Especifica la cadena de consulta para la suscripción. El formato de \<consulta > puede ser diferente para los distintos valores URI y se aplica a todos los orígenes en la suscripción.|
|/DIA:\<dialecto >|Define el dialecto que usa la cadena de consulta.|
|/TN:\<Transportname >|Especifica el nombre del transporte que se usa para conectarse a un origen de eventos remotos.|
|/TP:\<Transportport >|Establece el número de puerto utilizado por el transporte al conectarse a un origen de eventos remotos.|
|/DM:\<Deliverymode >|Especifica el modo de entrega. \<DeliveryMode > puede ser de inserción o extracción. Esta opción solo es válida si la **/cm** opción está establecida en Custom.|
|/DMI:\<Deliverymax >|Establece el número máximo de elementos para la entrega por lotes. Esta opción solo es válida si **/cm** está establecido en personalizado.|
|/dmlt:\<Deliverytime >|La latencia máxima se establece en la entrega de un lote de eventos. \<Deliverytime > es el número de milisegundos. Esta opción solo es válida si **/cm** está establecido en personalizado.|
|/hi:\<Heartbeat>|Define el intervalo de latidos. \<Latido > es el número de milisegundos. Esta opción solo es válida si **/cm** está establecido en personalizado.|
|/CF:\<contenido >|Especifica el formato de los eventos que se devuelven. \<Contenido > pueden ser eventos o información de configuración. Cuando el valor es la información de configuración, se devuelven los eventos con las cadenas localizadas (por ejemplo, la descripción del evento) asociadas al evento. El valor predeterminado es información de configuración.|
|/ l:\<locale >|Especifica la configuración regional para la entrega de las cadenas localizadas en formato de información de configuración. \<Configuración regional > es un identificador de idioma y país o región, por ejemplo, "EN-us". Esta opción solo es válida si la **/cf** opción está establecida en la información de configuración.|
|/ree:[\<Readexist>]|Identifica los eventos que se entregan de la suscripción. \<Readexist > puede true o false. Cuando el <Readexist> es true, todos los eventos existentes se leen desde los orígenes de eventos de suscripción. Cuando el <Readexist> es false, se entregan futuros eventos (entrantes). El valor predeterminado es true para un **/ree** sin un valor. Si no hay ningún **/ree** opción se especifica, el valor predeterminado es false.|
|/lf:\<Logfile>|Especifica el registro de eventos local que se usa para almacenar eventos recibidos de los orígenes de eventos.|
|/ pn:\<Publishername >|Especifica el nombre del publicador. Debe ser un publicador que posea o importa el registro especificado por el **/lf** opción.|
|/ESSP:\<Enableport >|Especifica que se debe anexar el número de puerto para el nombre principal de servicio del servicio remoto. \<Enableport > puede ser true o false. Se anexa el número de puerto cuando <Enableport> es true. Cuando se anexa el número de puerto, alguna configuración deban impedir el acceso a orígenes de eventos desde que se va a denegar.|
|/HN:\<nombre de host >|Especifica el nombre DNS del equipo local. Este nombre se usa con el origen de eventos remotos va a devolver los eventos y debe usarse solo para una suscripción de inserción.|
|/CT:\<tipo >|Establece el tipo de credencial para el acceso remoto de origen. \<Tipo > debe ser uno de los siguientes valores: predeterminado, negotiate, implícita, basic o localmachine. El valor predeterminado es el valor predeterminado.|
|/CUN:\<Comusername >|Establece la credencial de usuario compartidas que se usará para los orígenes de eventos que no tienen sus propias credenciales de usuario. Si se especifica esta opción con la **/c** , nombre de usuario y UserPassword la configuración de opciones para orígenes de eventos individuales del archivo de configuración se omite. Si desea usar una credencial diferente para un origen de evento específico, debe reemplazar este valor mediante la especificación de la **/un** y **/up** opciones para un origen de evento específico en la línea de comandos de otra **ss** comando.|
|/ taza:\<Compassword >|Establece la contraseña de usuario para la credencial de usuario compartida. Cuando \<Compassword > está establecida en * (asterisco), la contraseña se lee desde la consola. Esta opción solo es válida cuando la **/cun** se especifica la opción.|
|/q:[\<Quiet>]|Especifica si el procedimiento de configuración solicita confirmación. \<Quiet > puede ser true o false. Si <Quiet> es true, el procedimiento de configuración no solicita confirmación. El valor predeterminado de esta opción es false.|

## <a name="remarks"></a>Comentarios

> [!IMPORTANT]
> ¿Si recibe el mensaje, "el servidor RPC no está disponible? al intentar ejecutar wecutil, deberá iniciar el servicio Recopilador de eventos de Windows (wecsvc). Para empezar a wecsvc, en un símbolo del sistema con privilegios elevados, escriba net inicia wecsvc.

- El ejemplo siguiente muestra el contenido de un archivo de configuración:  
  ```
  <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path="Application">
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled="true">
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language="EN-US"></Locale>
  </Subscription>
  ```

## <a name="BKMK_examples"></a>Ejemplos

Información de configuración de salida para una suscripción denominada sub1:
```
wecutil gs sub1
```
Ejemplo de resultado:
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
Mostrar el estado de tiempo de ejecución de una suscripción denominada sub1:
```
wecutil gr sub1
```
Actualice la configuración de la suscripción denominada sub1 desde un archivo XML nuevo llamado WsSelRg2.xml:
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
Actualice la configuración de la suscripción denominada sub2 con varios parámetros:
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
Crear una suscripción para reenviar eventos a un aplicación de Windows Vista del registro de eventos de un equipo remoto en mySource.myDomain.com en el registro de eventos reenviados (vea la sección Comentarios para obtener un ejemplo de un archivo de configuración):
```
wecutil cs subscription.xml
```
Eliminar una suscripción denominada sub1:
```
wecutil ds sub1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
