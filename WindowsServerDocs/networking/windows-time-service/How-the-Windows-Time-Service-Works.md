---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Funcionamiento del servicio Hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 67c3471a726df354e0faa9e3aced491c4084e9e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864346"
---
# <a name="how-the-windows-time-service-works"></a>Funcionamiento del servicio Hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

**En esta sección**  
  
-   [Arquitectura del servicio de hora de Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocolos de tiempo de servicio de hora de Windows](#w2k3tr_times_how_ekoc)  
  
-   [Hora de Windows Service procesos e interacciones](#w2k3tr_times_how_izcr)  
  
-   [Puertos de red usados por el servicio de hora de Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> En este tema se explica solo cómo funciona el servicio de hora de Windows (W32Time). Para obtener información sobre cómo configurar el servicio de hora de Windows, consulte [configurar sistemas de alta precisión](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina servicio de directorio de Active Directory. En Windows Server 2008 y versiones posteriores, el servicio de directorio se denomina Active Directory Domain Services (AD DS). El resto de este tema hace referencia a AD DS, pero la información también es aplicable a Active Directory.  
  
Aunque el servicio de hora de Windows no es una implementación exacta de protocolo de tiempo de red (NTP), que usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos a lo largo de una red son tan precisos como sea posible. Idealmente, todos los relojes de los equipos en un dominio de AD DS se sincronizan con la hora de un equipo autorizado. Muchos factores pueden afectar a la sincronización de hora en una red. A menudo, los siguientes factores afectan a la precisión de la sincronización en AD DS:  
  
-   Condiciones de red  
  
-   La precisión del reloj de hardware del equipo  
  
-   La cantidad de recursos de CPU y de red disponibles para el servicio de hora de Windows  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de la aplicación dependiente del tiempo.  Sin embargo, las actualizaciones de Windows Server 2016 ahora le permiten implementar una solución de 1 ms precisión en el dominio.  Consulte [Windows 2016 preciso momento](accurate-time.md) y [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) para obtener más información.  
  
Se configuran los equipos que sincronizan su hora menos con frecuencia o no están unidos a un dominio, de forma predeterminada, para sincronizar con time.windows.com.  Por lo tanto, es imposible garantizar la precisión de tiempo en los equipos que tienen intermitente o ninguna conexión de red.  
  
Un bosque de AD DS tiene una jerarquía de sincronización de tiempo predeterminado. El servicio de hora de Windows sincroniza el tiempo entre los equipos dentro de la jerarquía, con los relojes de referencia más precisas en la parte superior. Si se configura más de un origen de hora en un equipo, el tiempo de Windows utiliza algoritmos NTP para seleccionar la mejor fuente de tiempo de los orígenes configurados según la capacidad del equipo para sincronizar con ese origen de hora. El servicio de hora de Windows no admite la sincronización de red de difusión o multidifusión elementos del mismo nivel. Para obtener más información acerca de estas características NTP, consulte RFC 1305 en la base de datos de RFC de IETF.  
  
Todos los equipos que se está ejecutando el servicio de hora de Windows usa el servicio para mantener la hora más precisa. Los equipos que son miembros de un dominio actúan como un vez que el cliente de forma predeterminada, por lo tanto, en la mayoría de los casos no es necesario configurar el servicio de hora de Windows. Sin embargo, el servicio de hora de Windows puede configurarse para que soliciten la hora a un origen de hora de referencia designado y también puede proporcionar tiempo a los clientes.
  
El grado en que se precisa tiempo de un equipo se denomina un satélites. El origen de hora más preciso en una red (por ejemplo, un reloj de hardware) ocupa el nivel más bajo de los satélites o los satélites uno. Se llama a esta fuente horaria precisa un reloj de referencia. Un servidor NTP que adquiere su tiempo directamente desde un reloj de referencia ocupa un satélites que está un nivel más alto que el reloj de la referencia. Los recursos que adquieren la hora desde el servidor NTP son dos pasos de reloj de referencia y por lo tanto, ocupan mayor que el origen de la hora más preciso un satélites que es la segunda y así sucesivamente. Como número de los satélites del equipo aumenta, la hora de su reloj del sistema pueden ser menos preciso. Por lo tanto, el nivel de los satélites de cualquier equipo es un indicador de cercanía ese equipo se sincroniza con el origen de la hora más preciso.  
  
Cuando el Administrador de W32Time recibe ejemplos de tiempo, utiliza algoritmos especiales NTP para determinar cuál de las muestras de tiempo es la más adecuada para su uso. El servicio de hora también usa otro conjunto de algoritmos para determinar cuál de los orígenes del tiempo configurado es el más preciso. Cuando el servicio de hora ha determinado qué muestra de tiempo es mejor, según los criterios mencionados anteriormente, ajusta la velocidad del reloj local para permitir que convergen hacia la hora correcta. Si la diferencia horaria entre el reloj local y el ejemplo de tiempo precisa seleccionado (también denominado el tiempo de sesgo) es demasiado grande para corregir mediante el ajuste de la velocidad del reloj local, en el servicio de hora se establece el reloj local a la hora correcta. Este ajuste de velocidad de reloj o cambio del tiempo de reloj directa se conoce como disciplina de reloj.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Arquitectura del servicio de hora de Windows  
El servicio de hora de Windows consta de los siguientes componentes:  
  
-   administrador de control de servicios  
  
-   Administrador del servicio de hora de Windows  
  
-   Disciplina de reloj  
  
-   Proveedores de hora  
  
En la siguiente ilustración muestra la arquitectura del servicio de hora de Windows.  
  
**Arquitectura del servicio de hora de Windows**  
  
![Hora de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
El Administrador de Control de servicio es responsable de iniciar y detener el servicio de hora de Windows. El Administrador de servicios de tiempo de Windows es responsable de iniciar la acción de los proveedores de hora NTP incluidos con el sistema operativo. El Administrador de servicios de tiempo de Windows controla todas las funciones del servicio de hora de Windows y el uso combinado de todas las muestras de tiempo. Además que proporciona información sobre el estado actual del sistema, como el origen de hora actual o la última vez que el reloj del sistema se actualizó, el Administrador de servicios de tiempo de Windows también es responsable de crear eventos en el registro de eventos.  
  
El proceso de sincronización de hora implica los pasos siguientes:  
  
-   Proveedores de entrada solicitar y recepción muestras de tiempo configurados orígenes de hora NTP.  
  
-   Estos ejemplos de tiempo, a continuación, se pasan a la Windows tiempo Service Manager, que recopila todos los ejemplos y los pasa al subcomponente de la disciplina de reloj.  
  
-   El subcomponente de la disciplina de reloj aplica a los algoritmos NTP que da como resultado de la selección del mejor ejemplo de tiempo.  
  
-   El subcomponente de la disciplina de reloj ajusta la hora del reloj del sistema a la hora más precisa ajustar la velocidad del reloj o cambiar directamente la hora.  
  
Si un equipo se ha designado como un servidor horario, puede enviar el tiempo de sesión en cualquier equipo que solicita la sincronización de hora en cualquier momento durante este proceso.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocolos de tiempo de servicio de hora de Windows  

Protocolos de tiempo determinan cómo estrechamente dos de los equipos se sincronizan los relojes. Un protocolo de tiempo es responsable de determinar la mejor información de hora disponible y converger los relojes para asegurarse de que se mantiene un tiempo consistente en sistemas independientes.  
  
El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para ayudar a sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluye los necesarios para sincronizar los relojes de los algoritmos de disciplina. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se utiliza en algunas versiones de Windows; Sin embargo W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con los equipos que ejecutan servicios de tiempo basadas en SNTP, como Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo de tiempo de red  
Protocolo de tiempo de red (NTP) es el valor predeterminado utilizado por el servicio de hora de Windows en el sistema operativo de protocolo de sincronización horaria. NTP es un protocolo de tiempo de tolerancia y altamente escalable y es el protocolo que se usan con mayor frecuencia para sincronizar los relojes de los equipos mediante una referencia de tiempo designado.  
  
Sincronización de hora NTP tiene lugar durante un período de tiempo e implica a la transferencia de paquetes NTP a través de una red. Paquetes NTP contienen marcas de tiempo que incluyen una muestra de tiempo desde el cliente y el servidor que participan en la sincronización de hora.  
  
NTP se basa en un reloj de referencia para definir la hora más precisa que se usará y sincroniza los relojes de todos en una red a ese reloj de referencia. NTP utiliza la hora Universal coordinada (UTC) como el estándar universal para la hora actual. UTC es independiente de las zonas horarias y permite NTP para usarse en cualquier lugar del mundo, independientemente de la configuración de zona horaria.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
NTP incluye dos algoritmos, un algoritmo de filtrado de reloj y un algoritmo de selección de reloj, para ayudar a determinar el mejor ejemplo de tiempo el servicio de hora de Windows. El algoritmo de filtrado de reloj está diseñado para examinar ejemplos de tiempo que se reciben de los orígenes de hora consultados y determinan las mejores muestras de tiempo de cada origen. El algoritmo de selección de reloj, a continuación, determina el servidor de hora más preciso en la red. A continuación, se pasa esta información para el algoritmo de disciplina de reloj, que utiliza la información recopilada para corregir el reloj local del equipo, mientras la compensación de errores debido a la imprecisión de reloj de latencia y el equipo de red.  
  
Los algoritmos NTP son más precisos en condiciones de cargas de red y del servidor de luz a moderada. Al igual que con cualquier algoritmo que toma el tiempo de tránsito de red en la cuenta, los algoritmos NTP podrían deficiente en condiciones de congestión de la red extreme. Para obtener más información acerca de los algoritmos NTP, consulte RFC 1305 en la base de datos de RFC de IETF.  
  
#### <a name="ntp-time-provider"></a>Proveedor de hora NTP  
El servicio de hora de Windows es un paquete de sincronización de tiempo de finalización que puede admitir una variedad de dispositivos de hardware y los protocolos de tiempo. Para habilitar esta compatibilidad, el servicio usa proveedores de hora conectable. Un proveedor de hora es responsable de cualquier obtener marcas de tiempo precisa (de la red o de hardware) o para proporcionar esas marcas de tiempo a otros equipos a través de la red.  
  
El proveedor NTP es el proveedor de hora estándar incluido con el sistema operativo. El proveedor NTP sigue los estándares especificados por la versión 3 para un cliente y el servidor NTP y puede interactuar con SNTP clientes y servidores para mantener la compatibilidad con Windows 2000 y otros clientes SNTP. El proveedor NTP en el servicio de hora de Windows consta de los elementos siguientes:  
  
-   **Proveedor de salida NtpServer.** Se trata de un servidor horario que responde a las solicitudes de tiempo del cliente en la red.  
  
-   **Proveedor de entrada NtpClient.** Se trata de un vez que el cliente que obtiene la información de tiempo de otro origen, un dispositivo de hardware o un servidor NTP y puede devolver los ejemplos de tiempo son útiles para sincronizar el reloj local.  
  
Aunque las operaciones reales de estas dos proveedores están estrechamente relacionadas, aparecen independientes para el servicio de hora. A partir de Windows 2000 Server, cuando un equipo Windows está conectado a una red, se configura como un cliente NTP. Además, los equipos que ejecutan la hora de Windows de servicio sólo intenta sincronizar la hora con un controlador de dominio o un origen de hora especificado manualmente de forma predeterminada. Estos son los proveedores de hora preferido porque son orígenes automáticamente disponibles y seguros de tiempo.  
  
#### <a name="ntp-security"></a>Seguridad NTP  

Dentro de un bosque de AD DS, el servicio de hora de Windows se basa en las características de seguridad de dominio estándar para exigir la autenticación de los datos de tiempo. La seguridad de paquetes NTP que se envían entre un equipo miembro del dominio y un controlador de dominio local está actuando como un servidor de hora se basa en la autenticación de clave compartida. El servicio de hora de Windows usa la clave de sesión de Kerberos del equipo para crear firmas autenticadas los paquetes de NTP que se envían a través de la red. No se transmiten los paquetes NTP dentro del canal seguro de Net Logon. En su lugar, cuando un equipo solicita el tiempo desde un controlador de dominio en la jerarquía de dominios, el servicio de hora de Windows requiere que se autentique el tiempo. El controlador de dominio, a continuación, devuelve la información necesaria en el formulario de un valor de 64 bits que se ha autenticado con la clave de sesión desde el servicio de Net Logon. Si el paquete NTP devuelto no está firmado con la clave de sesión del equipo o está firmado de forma incorrecta, se rechaza el tiempo. Este tipo de errores de autenticación se registran en el registro de eventos. De este modo, el servicio de hora de Windows proporciona seguridad para los datos NTP en un bosque de AD DS.  
  
Por lo general, los clientes de la hora de Windows obtienen automáticamente la hora exacta para la sincronización de controladores de dominio en el mismo dominio. En un bosque, los controladores de dominio de un dominio secundario sincronizan la hora con los controladores de dominio en sus dominios primarios. Cuando un servidor horario devuelve un paquete NTP autenticado a un cliente que solicita el tiempo, el paquete está firmado por medio de una clave de sesión de Kerberos definida por una cuenta de confianza entre dominios. Cuando un bosque une a un nuevo dominio de AD DS y el servicio de Net Logon administra la clave de sesión, se crea la cuenta de confianza entre dominios. De este modo, el controlador de dominio que está configurado como confiable en el dominio raíz del bosque se convierte en el origen de hora autenticados para todos los controladores de dominio en el elemento primario y secundario de dominios e indirectamente para todos los equipos ubicados en el árbol de dominios.  
  
El servicio de hora de Windows puede configurarse para funcionar entre bosques, pero es importante tener en cuenta que esta configuración no es segura. Por ejemplo, podría ser un servidor NTP disponible en un bosque diferente. Sin embargo, dado que ese equipo está en un bosque diferente, no hay ninguna clave de sesión de Kerberos con la que se va a iniciar sesión y autenticar los paquetes NTP. Para obtener la sincronización de hora precisa desde un equipo en un bosque diferente, el cliente necesita acceso de red a ese equipo y el servicio de hora debe configurarse para usar un origen de hora específico ubicado en el otro bosque. Si un cliente se configura manualmente a la hora de acceso de un servidor NTP fuera de su propia jerarquía de dominios, los paquetes NTP enviados entre el cliente y el servidor de hora no se han autenticado y, por lo tanto, no son seguros. Incluso con la implementación de las confianzas de bosque, el servicio de hora de Windows no es seguro entre bosques. Aunque el canal seguro de Net Logon es el mecanismo de autenticación para el servicio de hora de Windows, no se admite la autenticación entre bosques.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware que son compatibles con el servicio de hora de Windows  
Relojes basada en hardware, como GPS o los relojes de radio se utilizan a menudo como dispositivos de reloj de referencia muy precisos. De forma predeterminada, el proveedor de hora NTP de servicio de hora de Windows no admite la conexión directa de un dispositivo de hardware para un equipo, aunque es posible crear un proveedor de hora independientes basados en software que admita este tipo de conexión. Este tipo de proveedor, junto con el servicio de hora de Windows, puede proporcionar una referencia de hora confiable y estable.  
  
Dispositivos de hardware, como un reloj cesium o un receptor de sistema de posicionamiento Global (GPS), proporcionan una hora actual exacta siguiendo un estándar para obtener una definición precisa del tiempo. Relojes cesium son muy estables y no se ven afectados por factores como la temperatura, presión y humedad, pero también son muy costosos. Un receptor GPS es mucho menos caros de operar y también es un reloj de referencia precisos. Receptores GPS obtienen su hora de satélites que obtienen su tiempo de un reloj cesium. Sin el uso de un proveedor de hora independientes, servidores de hora de Windows pueden adquirir su tiempo mediante la conexión a un servidor NTP externo, que está conectado a un dispositivo de hardware por medio de un teléfono o Internet. Las organizaciones como el Observatorio Naval de Estados Unidos proporcionar servidores NTP que están conectados a los relojes de referencia muy confiable.  
  
Muchos de los destinatarios GPS y otros dispositivos de tiempo pueden funcionar como servidores NTP en una red. Puede configurar su bosque de AD DS para sincronizar la hora de estos dispositivos de hardware externo solo si también actúan como servidores NTP de la red. Para ello, configure el controlador de dominio funciona como el controlador de dominio principal (emulador) en la raíz del bosque para sincronizar con el servidor NTP proporcionado por el dispositivo GPS. Para ello, consulte [configurar el servicio de hora de Windows en el emulador de PDC en el dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo de tiempo de red simple  
El protocolo Simple de tiempo de red (SNTP) es un protocolo de tiempo simplificada que está pensado para servidores y clientes que no requieren el grado de precisión que proporciona el NTP. SNTP, una versión más básica de NTP, es el protocolo de tiempo principal que se usa en Windows 2000. Dado que los formatos de paquetes de red de NTP y SNTP son idénticos, los dos protocolos son interoperables. La diferencia principal entre los dos es que no tienen SNTP la administración de errores y los sistemas de filtrado complejos que proporciona el NTP. Para obtener más información sobre el protocolo Simple de tiempo de red, consulte RFC 1769 en la base de datos de RFC de IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidad de protocolos de tiempo  
El servicio de hora de Windows puede operar en un entorno mixto de equipos que ejecutan Windows 2000, Windows XP y Windows Server 2003, como el protocolo SNTP utilizado en Windows 2000 es interoperable con el protocolo NTP en Windows XP y Windows Server 2003.  
  
El servicio de hora de Windows NT Server 4.0, llamado TimeServ, sincroniza la hora a través de una red de Windows NT 4.0. TimeServ es una característica de complemento disponible como parte de la *Kit de recursos de Microsoft Windows NT 4.0* y no proporciona el grado de confiabilidad de la sincronización de tiempo que requiere Windows Server 2003.  
  
El servicio de hora de Windows puede interoperar con equipos que ejecutan Windows NT 4.0 porque puede sincronizar la hora con equipos que ejecutan Windows 2000 o Windows Server 2003; Sin embargo, un equipo que ejecuta Windows 2000 o Windows Server 2003 no detecta automáticamente los servidores de hora de Windows NT 4.0. Por ejemplo, si el dominio está configurado para sincronizar la hora con el método basado en jerarquía de dominio de sincronización y desea que los equipos de la jerarquía de dominios para sincronizar la hora con un controlador de dominio de Windows NT 4.0, debe configurarlos equipos manualmente para sincronizar con los controladores de dominio de Windows NT 4.0.  
  
Windows NT 4.0 utiliza un mecanismo más sencillo para la sincronización de hora que usa el servicio de hora de Windows. Por lo tanto, para garantizar la sincronización de hora exacta a través de la red, se recomienda que actualice los controladores de dominio de Windows NT 4.0 a Windows 2000 o Windows Server 2003.  
  
## <a name="w2k3tr_times_how_izcr"></a>Hora de Windows Service procesos e interacciones  

El servicio de hora de Windows está diseñado para sincronizar los relojes de los equipos de una red. El proceso de sincronización de tiempo de red, también denominado convergencia de tiempo, se produce a lo largo de una red como equipo cada vez que accede a desde un servidor de tiempo más preciso. Convergencia de tiempo implica un proceso por el que un servidor autoritativo proporciona la hora actual en los equipos cliente en forma de paquetes NTP. La información proporcionada en un paquete indica si un ajuste debe realizarse a la hora del equipo actual, por lo que se sincronice con el servidor más preciso.  
  
Como parte del proceso de convergencia de tiempo, los miembros del dominio intentan sincronizar la hora con cualquier controlador de dominio ubicado en el mismo dominio. Si el equipo es un controlador de dominio, intenta sincronizar con un controlador de dominio autoritativo más.  
  
Los equipos que ejecutan Windows XP Home Edition o equipos que no están unidos a un dominio no intentan sincronizar con la jerarquía de dominios, pero se configuran de forma predeterminada para obtener la hora de time.windows.com.  
  
Para establecer un equipo que ejecuta Windows Server 2003 como autorizada, el equipo debe estar configurado para que sea un origen de hora confiable. De forma predeterminada, el primer controlador de dominio que está instalado en un dominio de Windows Server 2003 se configura automáticamente para que sea un origen de hora confiable. Dado que es el equipo autorizado para el dominio, debe configurarse para sincronizar con un origen de hora externo en lugar de con la jerarquía de dominios. También de forma predeterminada, todos los demás miembros del dominio de Windows Server 2003 se configuran para sincronizar con la jerarquía de dominios.  
  
Después de haber establecido una red de Windows Server 2003, puede configurar el servicio de hora de Windows para usar una de las siguientes opciones para la sincronización:  
  
-   Sincronización de dominio basado en jerarquía  
  
-   Un origen de sincronización especificada manualmente  
  
-   Todos los mecanismos de sincronización disponibles  
  
-   Ninguna sincronización.  
  
En la siguiente sección se detallan cada uno de estos tipos de sincronización.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronización de dominio basado en jerarquía  

Sincronización que se basa en una jerarquía de dominios usa la jerarquía de dominios de AD DS para buscar un origen de confianza con el que se va a sincronizar la hora. En función de la jerarquía de dominios, el servicio de hora de Windows determina la precisión de cada servidor de tiempo. En un bosque de Windows Server 2003, el equipo que contiene el dominio principal (PDC) de controlador operaciones función de maestro emulador, ubicada en el dominio raíz del bosque, contiene la posición de la mejor fuente de tiempo, a menos que se ha configurado otro origen de hora confiable. La ilustración siguiente muestra una ruta de acceso de la sincronización temporal entre equipos en una jerarquía de dominios.  
  
**Sincronización de hora en una jerarquía de AD DS**  
![Hora de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuración de origen de hora confiable  
Un equipo que está configurado para ser un origen de hora confiable se identifica como la raíz del servicio de hora. La raíz del servicio de hora es el servidor autoritativo para el dominio y normalmente está configurada para recibir la hora de un servidor NTP externo o un dispositivo de hardware. Un servidor horario puede configurarse como un origen de hora confiable para optimizar cómo se transfiere la hora en toda la jerarquía de dominios. Si un controlador de dominio está configurado para que sea un origen de hora confiable, servicio de Net Logon anuncia ese controlador de dominio como origen de hora confiable cuando inicia sesión en la red. Al buscan un origen de hora sincronizar con otros controladores de dominio, que elijan un origen confiable en primer lugar si está disponible.  
  
#### <a name="time-source-selection"></a>Selección de origen de hora  
El proceso de selección de origen de hora puede crear dos problemas en una red:  
  
-   Ciclos de sincronización adicional.  
  
-   Aumento del volumen de tráfico de red.  
  
Se produce un ciclo en la red de sincronización al tiempo que permanece coherente entre un grupo de controladores de dominio y el mismo tiempo que se comparte entre ellos continuamente sin una resincronización con otro origen de hora confiable. Algoritmo de selección de origen de hora del servicio de hora de Windows está diseñado para protegerse frente a estos tipos de problemas.  
  
Un equipo utiliza uno de los métodos siguientes para identificar un origen de hora para sincronizar con:  
  
-   Si el equipo no es un miembro de un dominio, se debe configurar para sincronizar con un origen de hora especificado.  
  
-   Si el equipo es un servidor miembro o estación de trabajo dentro de un dominio, de forma predeterminada, a continuación la jerarquía de AD DS y sincroniza su tiempo con un controlador de dominio en su dominio local que se está ejecutando el servicio de hora de Windows.  
  
Si el equipo es un controlador de dominio, permite hasta seis de las consultas para buscar otro controlador de dominio para sincronizar con. Cada consulta está diseñada para identificar un origen de hora con determinados atributos, como un tipo de controlador de dominio, una ubicación determinada, y si es un origen de hora confiable. El origen de hora también debe cumplir las siguientes restricciones:  
  
-   Un origen de hora confiable sólo puede sincronizar con un controlador de dominio en el dominio primario.  
  
-   Un emulador de PDC se puede sincronizar con un origen de hora confiable en su propio dominio o cualquier controlador de dominio en el dominio primario.  
  
Si no puede sincronizar con el tipo de controlador de dominio que está consultando el controlador de dominio, no se realiza la consulta. El controlador de dominio sabe qué tipo de equipo puede obtener la hora desde antes de efectuar la consulta. Por ejemplo, un emulador PDC local no intenta números de consulta tres o seis porque un controlador de dominio no intentará sincronizar consigo misma.  
  
En la tabla siguiente se enumera las consultas que hace que sea un controlador de dominio para buscar un origen de hora y el orden en que se realizan las consultas.  
  
**Consulta de origen de hora del controlador de dominio**  
  
|Número de consultas|Controlador de dominio|Location|Confiabilidad de origen de hora|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de dominio principal|En el sitio|Prefiere confiable origen de hora, pero se puede sincronizar con un origen de hora que no sea confiable si eso es todo lo que está disponible.|  
|2|Controlador de dominio local|En el sitio|Solo se sincroniza con un origen de hora confiable.|  
|3|Emulador PDC local|En el sitio|No es aplicable.<br /><br />Un controlador de dominio no intentará sincronizar consigo misma.|  
|4|Controlador de dominio principal|Fuera del sitio|Prefiere confiable origen de hora, pero se puede sincronizar con un origen de hora que no sea confiable si eso es todo lo que está disponible.|  
|5|Controlador de dominio local|Fuera del sitio|Solo se sincroniza con un origen de hora confiable.|  
|6|Emulador PDC local|Fuera del sitio|No es aplicable.<br /><br />Un controlador de dominio no intentará sincronizar consigo misma.| 
  
**Nota:**  
  
-   Un equipo nunca se sincroniza con sí mismo. Si el equipo al intentar la sincronización es el emulador PDC local, no intente consultas 3 ó 6.  
  
Cada consulta devuelve una lista de controladores de dominio que se puede usar como origen de hora. Hora de Windows asigna cada controlador de dominio es consulta una puntuación basada en la confiabilidad y la ubicación del controlador de dominio. En la tabla siguiente se enumera las puntuaciones asignadas por hora de Windows para cada tipo de controlador de dominio.  
  
**Determinación de puntuación**  
  
|Estado del controlador de dominio|Puntuación|  
|----------------------------|---------|  
|Controlador de dominio ubicado en el mismo sitio|8|  
|Controlador de dominio marcada como un origen de hora confiable|4|  
|Controlador de dominio ubicado en el dominio primario|2|  
|Controlador de dominio que es un emulador PDC|1|  
  
Cuando el servicio de hora de Windows determina que el controlador de dominio que ha identificado con el mejor resultado posible, no se realizan más consultas. Las puntuaciones asignadas por el servicio de hora son acumulativas, lo que significa que un emulador de PDC ubicado en el mismo sitio recibe una puntuación de nueve.  
  
Si la raíz del servicio de hora no está configurada para sincronizarse con un origen externo, el reloj de hardware interno del equipo controla el tiempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronización especificada manualmente  
Sincronización especificada manualmente permite designar un único elemento del mismo nivel o una lista de elementos del mismo nivel desde el que un equipo obtiene la hora. Si el equipo no es un miembro de un dominio, se debe configurar manualmente para sincronizar con un origen de hora especificado. Un equipo que es que un miembro de un dominio se configura de forma predeterminada para sincronizar desde la jerarquía de dominios, sincronización especificada manualmente es muy útil para la raíz del bosque del dominio o para equipos que no están unidos a un dominio. Especificar manualmente un servidor NTP externo para sincronizar con el equipo autorizado para su dominio proporciona horario confiable. Sin embargo, la configuración del equipo autoritativo para el dominio sincronizar con un reloj de hardware es realmente una mejor solución para proporcionar la hora más precisa y segura a su dominio.  
  
Orígenes de hora especificado manualmente no se autentican a menos que se escribe un proveedor de hora específicos para ellos y, por lo tanto, son vulnerables a los atacantes. Además, si un equipo se sincroniza con un recurso especificado manualmente, en lugar de su controlador de dominio de autenticación, los dos equipos estén sincronizada, causando un error de autenticación Kerberos. Esto puede provocar otras acciones que requieren autenticación de red a un error, como impresión o uso compartido de archivos. Si sólo la raíz del bosque está configurada para sincronizar con un origen externo, todos los demás equipos del bosque permanecen sincronizados entre sí, dificultando los ataques de reproducción.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos los mecanismos de sincronización disponibles  

La opción de "todos los mecanismos de sincronización disponibles" es el método de sincronización más valioso para los usuarios en una red. Este método permite la sincronización con la jerarquía de dominios y también puede proporcionar una alternativa origen de hora si no está disponible, según la configuración de la jerarquía de dominios. Si el cliente no puede sincronizar la hora con la jerarquía de dominios, el origen de hora recurre automáticamente al origen de tiempo especificado por el **NtpServer** configuración. Este método de sincronización es más probable proporcionar la hora exacta a los clientes.  

### <a name="stopping-time-synchronization"></a>Sincronización de hora de detención  
Hay algunas situaciones en las que desea impedir que un equipo de la sincronización de su tiempo. Por ejemplo, si un equipo intenta sincronizar desde un origen de hora de Internet o desde otro sitio en una WAN mediante una conexión de acceso telefónico, pueden incurrir en gastos de teléfono costosas. Cuando se deshabilita la sincronización en ese equipo, evitar que el equipo se intenta tener acceso a un origen de hora a través de una conexión de acceso telefónico.  
  
También puede deshabilitar la sincronización para evitar la generación de errores en el registro de eventos. Cada vez que un equipo intenta sincronizar con un origen de hora no está disponible, genera un error en el registro de eventos. Si un origen de hora se realiza fuera de la red para el mantenimiento programado y no piensa volver a configurar el cliente para sincronizar desde otro origen, puede deshabilitar la sincronización en el cliente para evitar que el intento de sincronización, mientras que el servidor de tiempo no está disponible.  
  
Es útil deshabilitar la sincronización en el equipo que se designa como la raíz de la red de la sincronización. Esto indica que el equipo de raíz confíe su reloj local. Si la raíz de la jerarquía de sincronización no está establecida en **NoSync** y si no puede sincronizar con otro origen de hora, los clientes no aceptan el paquete no envía este equipo porque su hora no es de confianza.
  
La única vez que los servidores que confían los clientes incluso si no se han sincronizado con otro origen de hora son aquellos que se han identificado por el cliente como servidores de hora confiable.  
  
### <a name="disabling-the-windows-time-service"></a>Deshabilitar el servicio de hora de Windows  
Se puede deshabilitar completamente el servicio de hora de Windows (W32Time). Si decide implementar un producto de sincronización de hora de terceros que utiliza NTP, debe deshabilitar el servicio de hora de Windows. Esto es porque todos los servidores NTP necesitan tener acceso al puerto de protocolo de datagramas de usuario (UDP) 123 y, siempre y cuando el servicio de hora de Windows se ejecuta en el sistema operativo Windows Server 2003, el puerto 123 permanece reservado por hora de Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Puertos de red usados por el servicio de hora de Windows  
El servicio de hora de Windows se comunica en una red para identificar orígenes de hora confiable, obtener información de tiempo y proporcionar información de tiempo a otros equipos. Esta comunicación realiza como definido por el NTP y SNTP RFC.  
  
**Asignaciones de puertos para el servicio de hora de Windows**  
  
|Nombre del servicio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Vea también  
[Referencia técnica del servicio de hora de Windows](windows-time-service-tech-ref.md)
[herramientas del servicio de hora de Windows y la configuración](Windows-Time-Service-Tools-and-Settings.md)
[902229 de artículo de Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=186066)