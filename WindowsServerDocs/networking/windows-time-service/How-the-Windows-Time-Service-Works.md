---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Funcionamiento del servicio Hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2bf4a887218cd51e9c10954a75bbc1ba2112647f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405142"
---
# <a name="how-the-windows-time-service-works"></a>Funcionamiento del servicio Hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

**En esta sección**  
  
-   [Arquitectura de servicio de hora de Windows](#windows-time-service-architecture)  
  
-   [Protocolos de hora del servicio de hora de Windows](#windows-time-service-time-protocols)  
  
-   [Procesos e interacciones del servicio de hora de Windows](#windows-time-service-processes-and-interactions)  
  
-   [Puertos de red usados por el servicio de hora de Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> En este tema solo se explica cómo funciona el servicio de hora de Windows (W32Time). Para obtener información sobre cómo configurar el servicio de hora de Windows, consulta [Configuración de sistemas para alta precisión](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina Servicio de directorio de Active Directory. En Windows Server 2008 y versiones posteriores, el servicio de directorio se denomina Active Directory Domain Services (AD DS). El resto de este tema hace referencia a AD DS, pero la información también es aplicable a Active Directory.  
  
Aunque el servicio de hora de Windows no es una implementación exacta del Protocolo de tiempo de redes (NTP), usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos de una red sean lo más precisos posible. Idealmente, todos los relojes de los equipos de un dominio de AD DS están sincronizados con la hora de un equipo autoritativo. Muchos factores pueden afectar a la sincronización de la hora en una red. Los siguientes factores suelen afectar a la precisión de la sincronización en AD DS:  
  
-   Las condiciones de la red  
  
-   La precisión del reloj de hardware del equipo  
  
-   La cantidad de recursos de CPU y de red a disposición del servicio de hora de Windows  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no estaba diseñado para satisfacer las necesidades de las aplicaciones sujetas a limitaciones temporales.  Sin embargo, las actualizaciones de Windows Server 2016 ahora permiten implementar una solución con una precisión de 1 ms en el dominio.  Consulte [Hora precisa para Windows Server 2016](accurate-time.md) y [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) para obtener más información.  
  
Los equipos que sincronizan su hora con menos frecuencia o que no están unidos a un dominio están configurados de forma predeterminada para sincronizarse con time.windows.com.  Por lo tanto, no es posible garantizar la exactitud de la hora en los equipos que tienen conexiones de red intermitentes o sin conexión.  
  
Un bosque de AD DS tiene una jerarquía de sincronización de hora predeterminada. El servicio de hora de Windows sincroniza la hora entre los equipos de la jerarquía, donde los relojes de referencia más precisos se encuentran en la parte superior. Si se configura más de un origen de hora en un equipo, la hora de Windows usa algoritmos NTP para seleccionar el mejor origen de la hora entre los orígenes configurados en función de la capacidad del equipo de sincronizarse con ese origen de hora. El servicio de hora de Windows no admite la sincronización de red desde pares de difusión o del mismo nivel. Para más información sobre estas características de NTP, consulta el documento RFC 1305 en la base de datos RFC de IETF.  
  
Cada equipo que ejecuta el servicio de hora de Windows usa el servicio para mantener la hora más precisa. Los equipos que son miembros de un dominio funcionan como clientes de hora de forma predeterminada; por lo tanto, en la mayoría de los casos no es necesario configurar el servicio de hora de Windows. Sin embargo, el servicio de hora de Windows se puede configurar para solicitar la hora desde un origen de la hora de referencia designado, y también puede proporcionar la hora a clientes.
  
El grado de precisión de la hora de un equipo se denomina "estrato". El origen de la hora más preciso en una red (por ejemplo, un reloj de hardware) ocupa el nivel de estrato más bajo o el estrato uno. Este origen de la hora preciso se denomina "reloj de referencia". Un servidor NTP que adquiere su hora directamente de un reloj de referencia ocupa un estrato que es un nivel superior al del reloj de referencia. Los recursos que adquieren la hora del servidor NTP están a dos pasos del reloj de referencia y, por tanto, ocupan un estrato que es dos veces mayor que el origen de la hora más preciso, y así sucesivamente. A medida que aumenta el número de estrato de un equipo, la hora en el reloj del sistema puede ser menos precisa. Por lo tanto, el nivel de estrato de cualquier equipo es un indicador de cuán sincronizado está ese equipo con el origen de la hora más preciso.  
  
Cuando el administrador de W32Time recibe ejemplos de hora, usa algoritmos especiales en NTP para determinar cuál de los ejemplos de hora es el más adecuado para su uso. El servicio de hora también usa otro conjunto de algoritmos para determinar cuál de los orígenes de la hora configurados es el más preciso. Cuando el servicio de hora ha determinado cuál muestra de hora es la mejor, en función de los criterios anteriores, ajusta la frecuencia del reloj local para que pueda converger hacia la hora correcta. Si la diferencia de hora entre el reloj local y la muestra de hora exacta seleccionada (también denominado "sesgo horario") es demasiado grande para corregirla ajustando la frecuencia del reloj local, el servicio de hora establece el reloj local en la hora correcta. Este ajuste de la frecuencia del reloj o el cambio directo de la hora del reloj se conocen como "sincronización de reloj".  
  
## <a name="windows-time-service-architecture"></a>Arquitectura de servicio de hora de Windows  
El servicio de hora de Windows consta de los siguientes componentes:  
  
-   Administrador de control de servicios  
  
-   Administrador del servicio de hora de Windows  
  
-   Sincronización de reloj  
  
-   Proveedores de hora  
  
En la ilustración siguiente se muestra la arquitectura del servicio de hora de Windows.  
  
**Arquitectura de servicio de hora de Windows**  
  
![Hora de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
El administrador de control de servicios es responsable de iniciar y detener el servicio de hora de Windows. El administrador del servicio de hora de Windows es responsable de iniciar la acción de los proveedores de hora de NTP incluidos con el sistema operativo. El administrador del servicio de hora de Windows controla todas las funciones del servicio de hora de Windows y la fusión de todas las muestras de hora. Además de proporcionar información sobre el estado actual del sistema, como el origen de la hora actual o la última vez que se actualizó el reloj del sistema, el administrador del servicio de hora de Windows también es responsable de crear eventos en el registro de eventos.  
  
El proceso de sincronización de la hora conlleva los siguientes pasos:  
  
-   Los proveedores de entrada solicitan y reciben las muestras de hora de los orígenes de la hora NTP configurados.  
  
-   Estos ejemplos de hora se pasan luego al administrador del servicio de hora de Windows, que recopila todas las muestras y las pasa al subcomponente de sincronización de reloj.  
  
-   El subcomponente de sincronización de reloj aplica los algoritmos NTP, lo que da como resultado la selección de la mejor muestra de hora.  
  
-   El subcomponente de sincronización de reloj ajusta la hora del reloj del sistema a la hora más precisa, ya sea ajustando la frecuencia del reloj o cambiando la hora directamente.  
  
Si se ha designado un equipo como servidor de hora, puede enviar la hora a cualquier equipo que solicite la sincronización de la hora en cualquier momento de este proceso.  
  
## <a name="windows-time-service-time-protocols"></a>Protocolos de hora del servicio de hora de Windows  

Los protocolos de hora determinan el grado de sincronización de los relojes de dos equipos. Un protocolo de hora es responsable de determinar la mejor información de hora disponible y de hacer converger los relojes para asegurarse de que se mantenga una hora uniforme en sistemas independientes.  
  
El servicio de hora de Windows usa el Protocolo de tiempo de redes (NTP) para ayudar a sincronizar la hora a través de una red. NTP es un protocolo de hora de Internet que incluye los algoritmos de sincronización necesarios para sincronizar los relojes. NTP es un protocolo de hora más preciso que el Protocolo simple de tiempo de redes (SNTP) que se usa en algunas versiones de Windows; sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con equipos que ejecutan servicios de hora basados en SNTP, como Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo de tiempo de redes  
El Protocolo de tiempo de redes (NTP) es el protocolo de sincronización de hora predeterminado que usa el servicio de hora de Windows en el sistema operativo. NTP es un protocolo de hora altamente escalable y tolerante a errores, y es el protocolo que se usa con mayor frecuencia para sincronizar los relojes de los equipos mediante una referencia de hora designada.  
  
La sincronización de la hora de NTP tiene lugar durante un cierto período e implica la transferencia de paquetes NTP a través de una red. Los paquetes NTP contienen marcas de tiempo que incluyen una muestra de hora tanto del cliente como del servidor que participan en la sincronización de la hora.  
  
NTP se basa en un reloj de referencia para definir la hora más precisa que se va a usar, y sincroniza todos los relojes de una red con ese reloj de referencia. NTP usa la hora universal coordinada (UTC) como estándar universal para la hora actual. La hora UTC es independiente de las zonas horarias y permite usar NTP en cualquier parte del mundo, independientemente de la configuración de la zona horaria.  
  
#### <a name="ntp-algorithms"></a>Algoritmos de NTP  
NTP incluye dos algoritmos, un algoritmo de filtrado de relojes y un algoritmo de selección de reloj, para ayudar al servicio de hora de Windows a determinar la mejor muestra de hora. El algoritmo de filtrado de relojes está diseñado para examinar las muestras de hora que se reciben de los orígenes de hora consultados y determinar las mejores muestras de hora de cada origen. Luego, el algoritmo de selección de reloj determina el servidor de hora más preciso de la red. A continuación, esta información se pasa al algoritmo de sincronización de reloj, que usa la información recopilada para corregir el reloj local del equipo, a la vez que compensa los errores debido a la latencia de la red y a la inexactitud del reloj del equipo.  
  
Los algoritmos de NTP son más precisos en condiciones de cargas de red y de servidor de bajas a intermedias. Como con cualquier algoritmo que tiene en cuenta el tiempo de tránsito por la red, los algoritmos de NTP podrían tener un bajo rendimiento en condiciones de congestión extrema de la red. Para más información sobre los algoritmos de NTP, consulta el documento RFC 1305 en la base de datos RFC de IETF.  
  
#### <a name="ntp-time-provider"></a>Proveedor de hora de NTP  
El servicio de hora de Windows es un paquete completo de sincronización de la hora que puede admitir una variedad de dispositivos de hardware y protocolos de hora. Para habilitar esta compatibilidad, el servicio usa proveedores de hora conectables. Un proveedor de hora es responsable de obtener marcas de tiempo precisas (de la red o del hardware) o de proporcionar dichas marcas de tiempo a otros equipos a través de la red.  
  
El proveedor de NTP es el proveedor de hora estándar incluido con el sistema operativo. El proveedor de NTP sigue los estándares especificados por NTP, versión 3, para un cliente y servidor, y puede interactuar con clientes y servidores SNTP para mantener la compatibilidad con versiones anteriores de Windows 2000 y otros clientes SNTP. El proveedor de NTP en el servicio de hora de Windows consta de las siguientes dos partes:  
  
-   **Proveedor de salida NtpServer.** Se trata de un servidor de hora que responde a las solicitudes de hora de los clientes en la red.  
  
-   **Proveedor de entrada NtpClient.** Se trata de un cliente de hora que obtiene información de hora de otro origen, ya sea un dispositivo de hardware o un servidor NTP, y puede devolver muestras de hora que son útiles para sincronizar el reloj local.  
  
Aunque las operaciones reales de estos dos proveedores están estrechamente relacionadas, se muestran independientes ante el servicio de hora. A partir de Windows 2000 Server, cuando un equipo Windows está conectado a una red, se configura como cliente NTP. Además, los equipos que ejecutan el servicio de hora de Windows solo intentan sincronizar la hora con un controlador de dominio o un origen de hora especificado manualmente de manera predeterminada. Estos son los proveedores de hora preferidos porque son orígenes de hora que están disponibles de forma automática y son seguros.  
  
#### <a name="ntp-security"></a>Seguridad de NTP  

Dentro de un bosque de AD DS, el servicio de hora de Windows se basa en las características de seguridad estándar del dominio para aplicar la autenticación de los datos de hora. La seguridad de los paquetes NTP que se envían entre un equipo miembro del dominio y un controlador de dominio local que funciona como servidor de hora se basa en la autenticación de clave compartida. El servicio de hora de Windows usa la clave de sesión de Kerberos del equipo para crear firmas autenticadas en los paquetes NTP que se envían a través de la red. Los paquetes NTP no se transmiten dentro del canal seguro de Inicio de sesión de red. En su lugar, cuando un equipo solicita la hora a un controlador de dominio en la jerarquía de dominios, el servicio de hora de Windows requiere que se autentique la hora. Luego, el controlador de dominio devuelve la información necesaria en forma de un valor de 64 bits que se ha autenticado con la clave de sesión del servicio Inicio de sesión de red. Si el paquete NTP devuelto no está firmado con la clave de sesión del equipo o tiene una firma incorrecta, se rechaza la hora. Todos estos errores de autenticación se registran en el Registro de eventos. De este modo, el servicio de hora de Windows proporciona seguridad para los datos de NTP en un bosque de AD DS.  
  
Por lo general, los clientes de hora de Windows obtienen automáticamente la hora precisa para la sincronización desde controladores de dominio en el mismo dominio. En un bosque, los controladores de dominio de un dominio secundario sincronizan la hora con los controladores de dominio de sus dominios primarios. Cuando un servidor de hora devuelve un paquete NTP autenticado a un cliente que solicita la hora, el paquete se firma por medio de una clave de sesión de Kerberos definida por una cuenta de confianza entre dominios. La cuenta de confianza entre dominios se crea cuando un nuevo dominio de AD DS se une a un bosque, y el servicio Inicio de sesión de red administra la clave de sesión. De este modo, el controlador de dominio que está configurado como de confianza en el dominio raíz del bosque se convierte en el origen de la hora autenticado para todos los controladores de dominio, tanto de los dominios primarios como secundarios, e indirectamente en todos los equipos ubicados en el árbol de dominios.  
  
El servicio de hora de Windows se puede configurar para que funcione entre bosques, pero es importante tener en cuenta que esta configuración no es segura. Por ejemplo, un servidor NTP podría estar disponible en otro bosque. Sin embargo, dado que ese equipo está en otro bosque, no hay ninguna clave de sesión de Kerberos con la cual firmar y autenticar los paquetes NTP. Para obtener una sincronización de hora precisa de un equipo en otro bosque, el cliente necesita acceso de red a ese equipo, y el servicio de hora debe estar configurado para usar un origen de la hora específico ubicado en el otro bosque. Si un cliente se configura manualmente para acceder a la hora desde un servidor NTP fuera de su propia jerarquía de dominios, los paquetes NTP enviados entre el cliente y el servidor de hora no se autentican y, por lo tanto, no son seguros. Incluso con la implementación de confianzas de bosque, el servicio de hora de Windows no es seguro entre bosques. Aunque el canal seguro de Inicio de sesión de red es el mecanismo de autenticación para el servicio de hora de Windows, no se admite la autenticación entre bosques.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware admitidos por el servicio de hora de Windows  
Los relojes basados en hardware, como GPS o relojes de radio, suelen usarse como dispositivos de reloj de referencia muy precisos. De manera predeterminada, el proveedor de hora NTP del servicio de hora de Windows no admite la conexión directa de un dispositivo de hardware a un equipo, aunque es posible crear un proveedor de hora independiente basado en software que admita este tipo de conexión. Este tipo de proveedor, junto con el servicio de hora de Windows, puede proporcionar una referencia de hora estable y confiable.  
  
Los dispositivos de hardware, como un reloj de cesio o un receptor del sistema de posicionamiento global (GPS), proporcionan la hora actual precisa siguiendo un estándar para obtener una definición precisa de la hora. Los relojes de cesio son extremadamente estables y no se ven afectados por factores como la temperatura, la presión ni la humedad, pero también son muy costosos. Un receptor de GPS funciona de manera mucho más económica y también es un reloj de referencia preciso. Los receptores de GPS obtienen la hora a partir de satélites que obtienen su hora a partir de un reloj de cesio. Si no se usa un proveedor de hora independiente, los servidores de hora de Windows pueden adquirir la hora mediante la conexión a un servidor NTP externo, que se conecta a un dispositivo de hardware por medio de un teléfono o Internet. Las organizaciones como el Observatorio Naval de Estados Unidos proporcionan servidores NTP que están conectados a relojes de referencia extremadamente confiables.  
  
Muchos receptores de GPS y otros dispositivos de hora pueden funcionar como servidores NTP en una red. Puedes configurar el bosque de AD DS para que sincronice la hora a partir de estos dispositivos de hardware externos solo si también funcionan como servidores NTP en la red. Para ello, configura el controlador de dominio que funciona como emulador del controlador de dominio principal (PDC) en la raíz del bosque para que se sincronice con el servidor NTP proporcionado por el dispositivo GPS. Para ello, consulta [Configuración del servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo simple de tiempo de redes  
El Protocolo simple de tiempo de redes (SNTP) es un protocolo de hora simplificado diseñado para servidores y clientes que no requieren el grado de precisión que proporciona NTP. SNTP, una versión más rudimentaria de NTP, es el protocolo de hora principal que se usa en Windows 2000. Dado que los formatos de los paquetes de red de SNTP y NTP son idénticos, los dos protocolos son interoperables. La principal diferencia entre los dos es que SNTP no tiene los sistemas de administración de errores y de filtrado complejo que proporciona NTP. Para más información sobre el Protocolo simple de tiempo de redes, consulta el documento RFC 1769 en la base de datos RFC de IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidad de protocolos de hora  
El servicio de hora de Windows puede funcionar en un entorno mixto de equipos que ejecutan Windows 2000, Windows XP y Windows Server 2003, ya que el protocolo SNTP usado en Windows 2000 es interoperable con el protocolo NTP en Windows XP y Windows Server 2003.  
  
El servicio de hora de Windows NT Server 4.0, llamado TimeServ, sincroniza la hora a través de una red de Windows NT 4.0. TimeServ es una característica complementaria disponible como parte del *Kit de recursos de Microsoft Windows NT 4.0* y no proporciona el grado de confiabilidad de la sincronización de la hora que requiere Windows Server 2003.  
  
El servicio de hora de Windows puede interoperar con equipos que ejecutan Windows NT 4.0 porque pueden sincronizar la hora con equipos que ejecutan Windows 2000 o Windows Server 2003; sin embargo, un equipo que ejecuta Windows 2000 o Windows Server 2003 no detecta automáticamente los servidores de hora de Windows NT 4.0. Por ejemplo, si el dominio está configurado para sincronizar la hora mediante el método de sincronización basado en la jerarquía de dominios, y quieres que los equipos de la jerarquía de dominios sincronicen la hora con un controlador de dominio de Windows NT 4.0, tendrás que configurar esos equipos manualmente para que se sincronicen con los controladores de dominio de Windows NT 4.0.  
  
Windows NT 4.0 usa un mecanismo más sencillo para la sincronización de la hora que el que usa el servicio de hora de Windows. Por lo tanto, para garantizar una precisa sincronización de la hora en la red, se recomienda actualizar los controladores de dominio de Windows NT 4.0 a Windows 2000 o Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Procesos e interacciones del servicio de hora de Windows  

El servicio de hora de Windows está diseñado para sincronizar los relojes de los equipos de una red. El proceso de sincronización de la hora de la red, también denominado "convergencia de la hora", se produce a lo largo de una red a medida que cada equipo accede a la hora a partir de un servidor de hora más preciso. La convergencia de la hora implica un proceso por el cual un servidor autoritativo proporciona la hora actual a los equipos cliente en forma de paquetes NTP. La información proporcionada en un paquete indica si es necesario realizar un ajuste en la hora actual del reloj del equipo para que esté sincronizado con el servidor más preciso.  
  
Como parte del proceso de convergencia de la hora, los miembros del dominio intentan sincronizar la hora con cualquier controlador de dominio ubicado en el mismo dominio. Si el equipo es un controlador de dominio, intenta sincronizarse con un controlador de dominio con mayor autoridad.  
  
Los equipos que ejecutan Windows XP Home Edition o los equipos que no están unidos a un dominio no intentan sincronizarse con la jerarquía de dominios, sino que están configurados de forma predeterminada para obtener la hora de time.windows.com.  
  
Para establecer un equipo que ejecute Windows Server 2003 como autoritativo, el equipo debe estar configurado para ser un origen de la hora confiable. De manera predeterminada, el primer controlador de dominio que se instala en un dominio de Windows Server 2003 se configura automáticamente para que sea un origen de la hora confiable. Dado que se trata del equipo autoritativo para el dominio, debe configurarse para que se sincronice con un origen de la hora externo, en lugar de hacerlo con la jerarquía de dominios. También de manera predeterminada, todos los demás miembros de dominio de Windows Server 2003 están configurados para sincronizarse con la jerarquía de dominios.  
  
Después de establecer una red de Windows Server 2003, puedes configurar el servicio de hora de Windows para que use una de las siguientes opciones para la sincronización:  
  
-   La sincronización basada en la jerarquía de dominios  
  
-   Un origen de sincronización especificado manualmente  
  
-   Todos los mecanismos de sincronización disponibles  
  
-   Sin sincronización  
  
Cada uno de estos tipos de sincronización se analiza en las siguientes secciones.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronización basada en la jerarquía de dominios  

La sincronización que se basa en una jerarquía de dominios usa la jerarquía de dominios de AD DS para buscar un origen confiable con el que sincronizar la hora. En función de la jerarquía de dominios, el servicio de hora de Windows determina la precisión de cada servidor de hora. En un bosque de Windows Server 2003, el equipo que contiene el rol principal de operaciones del emulador del controlador de dominio principal (PDC), ubicado en el dominio raíz del bosque, contiene la posición del mejor origen de la hora, a menos que se haya configurado otro origen de la hora confiable. En la ilustración siguiente se muestra una ruta para la sincronización de la hora entre equipos en una jerarquía de dominios.  
  
**Sincronización de la hora en una jerarquía de AD DS**  
![Hora de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuración de origen de la hora de confianza  
Un equipo configurado para ser un origen de la hora de confianza se identifica como la raíz del servicio de hora. La raíz del servicio de hora es el servidor autoritativo para el dominio y normalmente está configurado para recuperar la hora a partir de un servidor NTP o dispositivo de hardware externos. Se puede configurar un servidor de hora como origen de la hora de confianza para optimizar la manera en que la hora se transfiere a través de la jerarquía de dominios. Si un controlador de dominio está configurado para ser un origen de la hora de confianza, el servicio Inicio de sesión de red anuncia ese controlador de dominio como origen de la hora de confianza cuando inicia sesión en la red. Cuando otros controladores de dominio buscan un origen de la hora con el cual sincronizarse, eligen primero un origen de confianza si hay alguno disponible.  
  
#### <a name="time-source-selection"></a>Selección del origen de la hora  
El proceso de selección del origen de la hora puede generar dos problemas en una red:  
  
-   Ciclos de sincronización adicionales.  
  
-   Mayor volumen de tráfico en la red.  
  
Un ciclo en la red de sincronización se produce cuando la hora permanece constante entre un grupo de controladores de dominio, y comparten la misma hora entre ellos de manera continua sin necesidad de volver a sincronizar con otro origen de la hora de confianza. El algoritmo de selección del origen de la hora del servicio de hora de Windows está diseñado para protegerse frente a estos tipos de problemas.  
  
Un equipo usa uno de los métodos siguientes para identificar un origen de la hora con el cual sincronizarse:  
  
-   Si el equipo no es miembro de un dominio, debe configurarse para que se sincronice con un origen de la hora especificado.  
  
-   Si el equipo es un servidor miembro o una estación de trabajo dentro de un dominio, de manera predeterminada sigue la jerarquía de AD DS y sincroniza la hora con un controlador de dominio en su dominio local que esté ejecutando actualmente el servicio de hora de Windows.  
  
Si el equipo es un controlador de dominio, hace hasta seis consultas para encontrar otro controlador de dominio con el cual sincronizarse. Cada consulta está diseñada para identificar un origen de la hora con determinados atributos, como un tipo de controlador de dominio, una ubicación determinada y si es un origen de la hora de confianza o no. El origen de la hora también debe cumplir las restricciones siguientes:  
  
-   Un origen la de hora de confianza solo puede sincronizarse con un controlador de dominio en el dominio primario.  
  
-   Un emulador de PDC se puede sincronizar con un origen de la hora de confianza en su propio dominio o en cualquier controlador de dominio del dominio primario.  
  
Si el controlador de dominio no se puede sincronizar con el tipo de controlador de dominio que está consultando, no se hace la consulta. El controlador de dominio sabe de qué tipo de equipo puede obtener la hora antes de hacer la consulta. Por ejemplo, un emulador de PDC local no intenta consultar los números tres o seis porque un controlador de dominio no intenta sincronizarse consigo mismo.  
  
En la tabla siguiente se enumeran las consultas que hace un controlador de dominio para buscar un origen de la hora y el orden en el que se hacen las consultas.  
  
**Consultas del controlador de dominio por orígenes de la hora**  
  
|Número de consulta|Controlador de dominio|Ubicación|Confiabilidad del origen de la hora|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de dominio primario|En el sitio|Prefiere un origen de la hora de confianza, pero puede sincronizarse con un origen de hora la que no sea de confianza si es lo único que está disponible.|  
|2|Controlador de dominio local|En el sitio|Solo se sincroniza con un origen de la hora de confianza.|  
|3|Emulador de PDC local|En el sitio|No corresponde.<br /><br />Un controlador de dominio no intenta sincronizarse consigo mismo.|  
|4|Controlador de dominio primario|Fuera del sitio|Prefiere un origen de la hora de confianza, pero puede sincronizarse con un origen de hora la que no sea de confianza si es lo único que está disponible.|  
|5|Controlador de dominio local|Fuera del sitio|Solo se sincroniza con un origen de la hora de confianza.|  
|6|Emulador de PDC local|Fuera del sitio|No corresponde.<br /><br />Un controlador de dominio no intenta sincronizarse consigo mismo.| 
  
**Note**  
  
-   Un equipo nunca se sincroniza consigo mismo. Si el equipo que intenta la sincronización es el emulador de PDC local, no intenta las consultas 3 ni 6.  
  
Cada consulta devuelve una lista de controladores de dominio que se pueden usar como origen de la hora. Hora de Windows asigna a cada controlador de dominio que recibe consultas una puntuación en función de la confiabilidad y la ubicación del controlador de dominio. En la tabla siguiente se enumeran las puntuaciones asignadas por Hora de Windows a cada tipo de controlador de dominio.  
  
**Determinación de la puntuación**  
  
|Estado del controlador de dominio|Puntuación|  
|----------------------------|---------|  
|Controlador de dominio ubicado en el mismo sitio|8|  
|Controlador de dominio marcado como origen de la hora de confianza|4|  
|Controlador de dominio ubicado en el dominio primario|2|  
|Controlador de dominio que es un emulador de PDC|1|  
  
Cuando el servicio de hora de Windows determina que ha identificado el controlador de dominio con la mejor puntuación posible, no se hacen más consultas. Las puntuaciones que asigna el servicio de hora son acumulativas; es decir, un emulador de PDC ubicado en el mismo sitio recibe una puntuación de nueve.  
  
Si la raíz del servicio de hora no está configurada para sincronizarse con un origen externo, el reloj de hardware interno del equipo rige la hora.  
  
### <a name="manually-specified-synchronization"></a>Sincronización especificada manualmente  
La sincronización especificada manualmente te permite designar un único homólogo o una lista de homólogos de los que un equipo obtiene la hora. Si el equipo no es miembro de un dominio, debe configurarse manualmente para que se sincronice con un origen de la hora especificado. Un equipo que es miembro de un dominio está configurado de manera predeterminada para sincronizarse a partir de la jerarquía de dominios, la sincronización especificada manualmente es el método más útil para la raíz del bosque del dominio o para los equipos que no están unidos a un dominio. La especificación manual de un servidor NTP externo para sincronizar con el equipo autoritativo del dominio proporciona una hora confiable. Sin embargo, la configuración del equipo autoritativo para que tu dominio se sincronice con un reloj de hardware en verdad es una mejor solución para proporcionar la hora más precisa y segura para tu dominio.  
  
Los orígenes de la hora especificados manualmente no se autentican a menos que se les indique un proveedor de hora específico y, por tanto, son vulnerables a los atacantes. Además, si un equipo se sincroniza con un origen especificado manualmente en lugar de hacerlo con su controlador de dominio de autenticación, es posible que los dos equipos no estén sincronizados, lo que provocaría un error en la autenticación de Kerberos. Esto podría provocar errores en otras acciones que requieran la autenticación de la red, como la impresión o el uso compartido de archivos. Si solo la raíz del bosque está configurada para sincronizarse con un origen externo, todos los demás equipos del bosque permanecen sincronizados entre sí, lo que dificulta los ataques de reinyección.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos los mecanismos de sincronización disponibles  

La opción "todos los mecanismos de sincronización disponibles" es el método de sincronización más valioso para los usuarios de una red. Este método permite la sincronización con la jerarquía de dominios y también puede proporcionar un origen de la hora alternativo si la jerarquía de dominios deja de estar disponible, según la configuración. Si el cliente no puede sincronizar la hora con la jerarquía de dominios, el origen de la hora recurre automáticamente al origen de la hora especificado por la configuración de **NtpServer**. Es más probable que este método de sincronización proporcione una hora precisa a los clientes.  

### <a name="stopping-time-synchronization"></a>Detención de la sincronización de la hora  
Existen ciertas situaciones en las que querrás que un equipo deje de sincronizar la hora. Por ejemplo, si un equipo intenta sincronizarse desde un origen de la hora en Internet o desde otro sitio a través de una WAN por medio de una conexión de acceso telefónico, puede suponer costosos cargos telefónicos. Cuando deshabilitas la sincronización en ese equipo, se impides que el equipo intente acceder a un origen de la hora a través de una conexión de acceso telefónico.  
  
También puedes deshabilitar la sincronización para evitar la generación de errores en el Registro de eventos. Cada vez que un equipo intenta sincronizarse con un origen de la hora que no está disponible, genera un error en el Registro de eventos. Si un origen de la hora se desconecta de la red para un mantenimiento programado y no tienes intención de reconfigurar el cliente para que se sincronice a partir de otro origen, puedes deshabilitar la sincronización en el cliente para evitar que intente la sincronización mientras el servidor de hora no esté disponible.  
  
Resulta útil deshabilitar la sincronización en el equipo designado como raíz de la red de sincronización. Esto indica que el equipo raíz confía en su reloj local. Si la raíz de la jerarquía de sincronización no está establecida en **NoSync** y, si no se puede sincronizar con otro origen de la hora, los clientes no aceptan el paquete que envía este equipo porque no es de confianza.
  
Los únicos servidores de hora en los que los clientes confían incluso si no se han sincronizado con otro origen de la hora son los identificados por el cliente como servidores de hora de confianza.  
  
### <a name="disabling-the-windows-time-service"></a>Deshabilitación del servicio de hora de Windows  
El servicio de hora de Windows (W32Time) se puede deshabilitar por completo. Si decides implementar un producto de sincronización de hora de un tercero que use NTP, tienes que deshabilitar el servicio de hora de Windows. Esto se debe a que todos los servidores NTP necesitan acceso al puerto 123 del Protocolo de datagramas de usuario (UDP) y, siempre y cuando el servicio de hora de Windows se esté ejecutando en el sistema operativo Windows Server 2003, el puerto 123 permanece reservado por Hora de Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Puertos de red usados por el servicio de hora de Windows  
El servicio de hora de Windows se comunica en una red para identificar los orígenes de la hora de confianza, para obtener información de hora y proporcionar información de hora a otros equipos. Establece esta comunicación tal y como se define en las RFC de NTP y SNTP.  
  
**Asignaciones de puertos para el servicio de hora de Windows**  
  
|Nombre de servicio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/A|  
|SNTP|123|N/A|  
  
## <a name="see-also"></a>Consulte también  
[Referencia técnica del servicio de hora de Windows](windows-time-service-tech-ref.md)
[Configuración y herramientas del servicio Hora de Windows](Windows-Time-Service-Tools-and-Settings.md)
[Artículo 902229 de Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=186066)