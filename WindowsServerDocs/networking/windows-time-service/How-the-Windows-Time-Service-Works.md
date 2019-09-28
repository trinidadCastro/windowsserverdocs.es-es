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
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405142"
---
# <a name="how-the-windows-time-service-works"></a>Funcionamiento del servicio Hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

**En esta sección**  
  
-   [Arquitectura de servicio de hora de Windows](#windows-time-service-architecture)  
  
-   [Protocolos de Tiempo de servicio de hora de Windows](#windows-time-service-time-protocols)  
  
-   [Interacciones y procesos del servicio de hora de Windows](#windows-time-service-processes-and-interactions)  
  
-   [Puertos de red usados por el servicio de hora de Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> En este tema solo se explica cómo funciona el servicio de hora de Windows (W32Time). Para obtener información acerca de cómo configurar el servicio de hora de Windows, consulte [configuración de sistemas para una precisión alta](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina Active Directory servicio de directorio. En Windows Server 2008 y versiones posteriores, el servicio de directorio se denomina Active Directory Domain Services (AD DS). El resto de este tema hace referencia a AD DS, pero la información también es aplicable a Active Directory.  
  
Aunque el servicio de hora de Windows no es una implementación exacta del Protocolo de tiempo de red (NTP), utiliza el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos de una red sean lo más precisos posible. Idealmente, todos los relojes de los equipos de un dominio de AD DS se sincronizan con la hora de un equipo autoritativo. Muchos factores pueden afectar a la sincronización de la hora en una red. Los siguientes factores suelen afectar a la precisión de la sincronización en AD DS:  
  
-   Condiciones de red  
  
-   La precisión del reloj de hardware del equipo  
  
-   La cantidad de recursos de CPU y de red disponibles para el servicio de hora de Windows  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no estaba diseñado para satisfacer las necesidades de la aplicación que depende del tiempo.  Sin embargo, las actualizaciones de Windows Server 2016 permiten ahora implementar una solución de una precisión de 1 ms en el dominio.  Consulte el límite de [tiempo](accurate-time.md) y [soporte técnico de Windows 2016 para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) para obtener más información.  
  
Los equipos que sincronizan su hora con menos frecuencia o que no están Unidos a un dominio se configuran de forma predeterminada para sincronizarse con time.windows.com.  Por lo tanto, no es posible garantizar la exactitud del tiempo en los equipos que tienen conexiones de red intermitentes o sin conexión.  
  
Un bosque de AD DS tiene una jerarquía de sincronización de hora predeterminada. El servicio de hora de Windows sincroniza el tiempo entre los equipos de la jerarquía, con los relojes de referencia más precisos en la parte superior. Si se configura más de un origen de hora en un equipo, la hora de Windows utiliza algoritmos NTP para seleccionar el origen de mejor hora de los orígenes configurados en función de la capacidad del equipo de sincronizarse con ese origen de hora. El servicio de hora de Windows no admite la sincronización de red desde equipos del mismo nivel de difusión o multidifusión. Para obtener más información acerca de estas características de NTP, consulte RFC 1305 en la base de datos de RFC de IETF.  
  
Cada equipo que ejecuta el servicio de hora de Windows usa el servicio para mantener el tiempo más preciso. Los equipos que son miembros de un dominio actúan como cliente de hora de forma predeterminada, por lo tanto, en la mayoría de los casos no es necesario configurar el servicio de hora de Windows. Sin embargo, el servicio de hora de Windows se puede configurar para solicitar la hora desde un origen de hora de referencia designado y también puede proporcionar tiempo a los clientes.
  
El grado de precisión de la hora de un equipo se denomina estrato. El origen de hora más preciso en una red (por ejemplo, un reloj de hardware) ocupa el nivel de estrato más bajo o estrato uno. Este origen de hora preciso se denomina reloj de referencia. Un servidor NTP que adquiere su tiempo directamente desde un reloj de referencia ocupa una capa que es un nivel superior al del reloj de referencia. Los recursos que adquieren tiempo desde el servidor NTP son dos pasos del reloj de referencia y, por tanto, ocupan una capa que es dos veces mayor que el origen de hora más preciso, y así sucesivamente. A medida que aumenta el número de estrato de un equipo, el tiempo en el reloj del sistema puede ser menos preciso. Por lo tanto, el nivel de estrato de cualquier equipo es un indicador de cuánto se sincroniza ese equipo con el origen de hora más preciso.  
  
Cuando el administrador de W32Time recibe ejemplos de tiempo, usa algoritmos especiales en NTP para determinar cuál de los ejemplos de tiempo es el más adecuado para su uso. El servicio de hora también utiliza otro conjunto de algoritmos para determinar cuál de los orígenes de hora configurados es el más preciso. Cuando el servicio de hora ha determinado qué tiempo muestra mejor el ejemplo, en función de los criterios anteriores, ajusta la velocidad del reloj local para que pueda converger hacia la hora correcta. Si la diferencia de tiempo entre el reloj local y el ejemplo de hora exacta seleccionada (también denominado sesgo horario) es demasiado grande para corregirlo ajustando la tasa de reloj local, el servicio de hora establece el reloj local en la hora correcta. Este ajuste de la tasa de reloj o el cambio de tiempo de reloj directo se conoce como disciplina de reloj.  
  
## <a name="windows-time-service-architecture"></a>Arquitectura de servicio de hora de Windows  
El servicio de hora de Windows consta de los siguientes componentes:  
  
-   administrador de control de servicios  
  
-   Service Manager de hora de Windows  
  
-   Disciplina del reloj  
  
-   Proveedores de hora  
  
En la siguiente ilustración se muestra la arquitectura del servicio de hora de Windows.  
  
**Arquitectura de servicio de hora de Windows**  
  
![Hora de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
El administrador de control de servicios es responsable de iniciar y detener el servicio de hora de Windows. La Service Manager de hora de Windows es responsable de iniciar la acción de los proveedores de hora NTP incluidos con el sistema operativo. La hora de Windows Service Manager controla todas las funciones del servicio de hora de Windows y la combinación de todos los ejemplos de tiempo. Además de proporcionar información sobre el estado actual del sistema, como el origen de hora actual o la última vez que se actualizó el reloj del sistema, el Service Manager de hora de Windows también es responsable de crear eventos en el registro de eventos.  
  
El proceso de sincronización de hora conlleva los siguientes pasos:  
  
-   Los proveedores de entrada solicitan y reciben ejemplos de tiempo de los orígenes de hora NTP configurados.  
  
-   Estos ejemplos de tiempo se pasan a la hora de Windows Service Manager, que recopila todos los ejemplos y los pasa al subcomponente de la disciplina del reloj.  
  
-   El subcomponente de la disciplina del reloj aplica los algoritmos NTP, lo que da como resultado la selección del ejemplo de mejor hora.  
  
-   El subcomponente de la disciplina del reloj ajusta la hora del reloj del sistema a la hora más precisa, ya sea ajustando la velocidad del reloj o cambiando directamente la hora.  
  
Si un equipo se ha designado como servidor horario, puede enviar la hora a cualquier equipo que solicite la sincronización de hora en cualquier momento de este proceso.  
  
## <a name="windows-time-service-time-protocols"></a>Protocolos de Tiempo de servicio de hora de Windows  

Los protocolos de hora determinan el grado de sincronización de los relojes de dos equipos. Un protocolo de tiempo es responsable de determinar la mejor información de hora disponible y de converger los relojes para asegurarse de que se mantiene un tiempo coherente en sistemas independientes.  
  
El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para ayudar a sincronizar el tiempo a través de una red. NTP es un protocolo de hora de Internet que incluye los algoritmos de disciplina necesarios para sincronizar los relojes. NTP es un protocolo de hora más preciso que el Protocolo simple de tiempo de redes (SNTP) que se usa en algunas versiones de Windows. sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con los equipos que ejecutan servicios de hora basados en SNTP, como Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo de tiempo de red  
El protocolo de tiempo de red (NTP) es el protocolo de sincronización de hora predeterminado que usa el servicio de hora de Windows en el sistema operativo. NTP es un protocolo de tiempo altamente escalable tolerante a errores y es el protocolo que se usa con más frecuencia para sincronizar los relojes de los equipos mediante una referencia de hora designada.  
  
La sincronización de la hora de NTP tiene lugar durante un período de tiempo e implica la transferencia de paquetes NTP a través de una red. Los paquetes NTP contienen marcas de tiempo que incluyen una muestra de tiempo del cliente y el servidor que participa en la sincronización.  
  
NTP se basa en un reloj de referencia para definir el tiempo más preciso que se va a usar y sincroniza todos los relojes de una red con ese reloj de referencia. NTP usa la hora universal coordinada (UTC) como el estándar universal para la hora actual. La hora UTC es independiente de las zonas horarias y permite el uso de NTP en cualquier parte del mundo, independientemente de la configuración de la zona horaria.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
NTP incluye dos algoritmos, un algoritmo de filtrado de reloj y un algoritmo de selección de reloj, para ayudar al servicio de hora de Windows a determinar el ejemplo de mejor tiempo. El algoritmo de filtrado de reloj está diseñado para examinar los ejemplos de tiempo que se reciben de los orígenes de hora consultados y determinar las mejores muestras de tiempo de cada origen. A continuación, el algoritmo de selección de reloj determina el servidor horario más preciso de la red. A continuación, esta información se pasa al algoritmo de disciplina del reloj, que usa la información recopilada para corregir el reloj local del equipo, al tiempo que se compensan los errores debido a la latencia de la red y a la inexactitud del reloj del equipo.  
  
Los algoritmos NTP son más precisos en condiciones de carga de red y de servidor de poca importancia. Como con cualquier algoritmo que tenga en cuenta el tiempo de tránsito de la red, los algoritmos NTP podrían tener un rendimiento bajo en condiciones de congestión de la red extrema. Para obtener más información acerca de los algoritmos NTP, consulte RFC 1305 en la base de datos RFC de IETF.  
  
#### <a name="ntp-time-provider"></a>Proveedor de hora NTP  
El servicio de hora de Windows es un paquete de sincronización de tiempo completo que puede admitir una variedad de dispositivos de hardware y protocolos de hora. Para habilitar esta compatibilidad, el servicio utiliza proveedores de hora conectables. Un proveedor de hora es responsable de obtener marcas de tiempo precisas (de la red o del hardware) o de proporcionar dichas marcas de tiempo a otros equipos a través de la red.  
  
El proveedor NTP es el proveedor de hora estándar incluido en el sistema operativo. El proveedor NTP sigue los estándares especificados por la versión 3 de NTP para un cliente y un servidor, y puede interactuar con clientes y servidores SNTP para mantener la compatibilidad con versiones anteriores de Windows 2000 y otros clientes SNTP. El proveedor NTP en el servicio de hora de Windows consta de las dos partes siguientes:  
  
-   **Proveedor de salida NtpServer.** Se trata de un servidor horario que responde a las solicitudes de tiempo de cliente en la red.  
  
-   **Proveedor de entrada NtpClient.** Se trata de un cliente de hora que obtiene información de hora de otro origen, ya sea un dispositivo de hardware o un servidor NTP, y puede devolver ejemplos de tiempo que son útiles para sincronizar el reloj local.  
  
Aunque las operaciones reales de estos dos proveedores están estrechamente relacionadas, parecen independientes del servicio de hora. A partir de Windows 2000 Server, cuando un equipo Windows está conectado a una red, se configura como un cliente NTP. Además, los equipos que ejecutan el servicio de hora de Windows solo intentan sincronizar la hora con un controlador de dominio o un origen de hora especificado manualmente de forma predeterminada. Estos son los proveedores de hora preferidos porque están disponibles de forma automática y seguros.  
  
#### <a name="ntp-security"></a>Seguridad de NTP  

Dentro de un bosque de AD DS, el servicio de hora de Windows se basa en las características de seguridad de dominio estándar para aplicar la autenticación de datos de hora. La seguridad de los paquetes NTP que se envían entre un equipo miembro del dominio y un controlador de dominio local que actúa como servidor horario se basa en la autenticación de clave compartida. El servicio de hora de Windows usa la clave de sesión Kerberos del equipo para crear firmas autenticadas en los paquetes NTP que se envían a través de la red. Los paquetes NTP no se transmiten dentro del canal seguro de inicio de sesión de red. En su lugar, cuando un equipo solicita la hora de un controlador de dominio en la jerarquía de dominios, el servicio de hora de Windows requiere que se autentique la hora. A continuación, el controlador de dominio devuelve la información necesaria en forma de un valor de 64 bits que se ha autenticado con la clave de sesión del servicio de inicio de sesión de red. Si el paquete NTP devuelto no está firmado con la clave de sesión del equipo o está firmado incorrectamente, se rechaza la hora. Todos estos errores de autenticación se registran en el registro de eventos. De esta manera, el servicio de hora de Windows proporciona seguridad para los datos NTP en un bosque de AD DS.  
  
Por lo general, los clientes de hora de Windows obtienen automáticamente el tiempo preciso para la sincronización desde los controladores de dominio del mismo dominio. En un bosque, los controladores de dominio de un dominio secundario sincronizan la hora con los controladores de dominio de sus dominios primarios. Cuando un servidor horario devuelve un paquete NTP autenticado a un cliente que solicita la hora, el paquete se firma por medio de una clave de sesión de Kerberos definida por una cuenta de confianza entre dominios. La cuenta de confianza entre dominios se crea cuando un dominio de AD DS nuevo se une a un bosque y el servicio de inicio de sesión de red administra la clave de sesión. De esta manera, el controlador de dominio que está configurado como confiable en el dominio raíz del bosque se convierte en el origen de la hora autenticada para todos los controladores de dominio de los dominios primarios y secundarios, e indirectamente en todos los equipos ubicados en el árbol de dominios.  
  
El servicio de hora de Windows se puede configurar para que funcione entre bosques, pero es importante tener en cuenta que esta configuración no es segura. Por ejemplo, un servidor NTP podría estar disponible en un bosque diferente. Sin embargo, dado que ese equipo está en un bosque diferente, no hay ninguna clave de sesión de Kerberos con la que firmar y autenticar los paquetes NTP. Para obtener una sincronización de hora precisa de un equipo en un bosque diferente, el cliente necesita acceso de red a ese equipo y el servicio de hora debe estar configurado para usar un origen de hora específico ubicado en el otro bosque. Si un cliente se configura manualmente para tener acceso a la hora desde un servidor NTP fuera de su propia jerarquía de dominios, los paquetes NTP enviados entre el cliente y el servidor horario no se autentican y, por lo tanto, no son seguros. Incluso con la implementación de confianzas de bosque, el servicio de hora de Windows no es seguro entre bosques. Aunque el canal seguro de inicio de sesión de red es el mecanismo de autenticación para el servicio de hora de Windows, no se admite la autenticación entre bosques.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware admitidos por el servicio de hora de Windows  
Los relojes basados en hardware como GPS o relojes de radio suelen usarse como dispositivos de reloj de referencia muy precisos. De forma predeterminada, el proveedor de hora NTP del servicio de hora de Windows no admite la conexión directa de un dispositivo de hardware a un equipo, aunque es posible crear un proveedor de hora independiente basado en software que admita este tipo de conexión. Este tipo de proveedor, junto con el servicio de hora de Windows, puede proporcionar una referencia de tiempo estable y confiable.  
  
Los dispositivos de hardware, como un reloj de Cesium o un receptor del sistema de posicionamiento global (GPS), proporcionan el tiempo actual preciso siguiendo un estándar para obtener una definición de tiempo precisa. Los relojes de Cesium son extremadamente estables y no se ven afectados por factores como la temperatura, la presión o la humedad, sino que también son muy costosos. Un receptor de GPS es mucho más económico de operar y también es un reloj de referencia preciso. Los receptores de GPS obtienen su tiempo de los satélites que obtienen su tiempo desde un reloj de Cesium. Sin el uso de un proveedor de hora independiente, los servidores de hora de Windows pueden adquirir su tiempo mediante la conexión a un servidor NTP externo, que se conecta a un dispositivo de hardware por medio de un teléfono o Internet. Las organizaciones como el Estados Unidos Observatorio Naval proporcionan servidores NTP que están conectados a relojes de referencia extremadamente confiables.  
  
Muchos receptores de GPS y otros dispositivos de tiempo pueden funcionar como servidores NTP en una red. Puede configurar el bosque de AD DS para sincronizar la hora de estos dispositivos de hardware externo sólo si también actúan como servidores NTP en la red. Para ello, configure el controlador de dominio que funciona como emulador del controlador de dominio principal (PDC) en la raíz del bosque para sincronizar con el servidor NTP proporcionado por el dispositivo GPS. Para ello, vea [configurar el servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo simple de tiempo de red  
El Protocolo simple de tiempo de redes (SNTP) es un protocolo de tiempo simplificado diseñado para servidores y clientes que no requieren el grado de precisión que proporciona NTP. SNTP, una versión más rudimentaria de NTP, es el protocolo de hora principal que se usa en Windows 2000. Dado que los formatos de paquete de red de SNTP y NTP son idénticos, los dos protocolos son interoperables. La principal diferencia entre los dos es que SNTP no tiene los sistemas de filtrado complejos y de administración de errores que proporciona NTP. Para obtener más información acerca del Protocolo simple de tiempo de red, consulte RFC 1769 en la base de datos de RFC de IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidad del protocolo Time  
El servicio de hora de Windows puede funcionar en un entorno mixto de equipos que ejecutan Windows 2000, Windows XP y Windows Server 2003, ya que el protocolo SNTP usado en Windows 2000 es interoperable con el protocolo NTP en Windows XP y Windows Server 2003.  
  
El servicio de hora de Windows NT Server 4,0, llamado TimeServ, sincroniza la hora a través de una red Windows NT 4,0. TimeServ es una característica de complemento disponible como parte del kit de *recursos de Microsoft Windows NT 4,0* y no proporciona el grado de confiabilidad de la sincronización de tiempo que requiere Windows Server 2003.  
  
El servicio de hora de Windows puede interoperar con equipos que ejecutan Windows NT 4,0 porque pueden sincronizar la hora con equipos que ejecutan Windows 2000 o Windows Server 2003; sin embargo, un equipo que ejecuta Windows 2000 o Windows Server 2003 no detecta automáticamente los servidores de hora de Windows NT 4,0. Por ejemplo, si el dominio está configurado para sincronizar el tiempo mediante el método de sincronización basado en la jerarquía de dominios y desea que los equipos de la jerarquía de dominios sincronicen la hora con un controlador de dominio de Windows NT 4,0, tendrá que configurarlos. equipos manualmente para sincronizar con los controladores de dominio de Windows NT 4,0.  
  
Windows NT 4,0 usa un mecanismo más sencillo para la sincronización de hora que el servicio de hora de Windows. Por lo tanto, para garantizar una sincronización de hora precisa en la red, se recomienda actualizar los controladores de dominio de Windows NT 4,0 a Windows 2000 o Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Interacciones y procesos del servicio de hora de Windows  

El servicio de hora de Windows está diseñado para sincronizar los relojes de los equipos de una red. El proceso de sincronización de la hora de la red, también denominado convergencia de hora, se produce a lo largo de una red, ya que cada equipo accede a la hora desde un servidor horario más preciso. La convergencia de hora implica un proceso por el que un servidor autoritativo proporciona la hora actual a los equipos cliente en forma de paquetes NTP. La información proporcionada en un paquete indica si es necesario realizar un ajuste en la hora actual del reloj del equipo para que se sincronice con el servidor más preciso.  
  
Como parte del proceso de convergencia de tiempo, los miembros del dominio intentan sincronizar la hora con cualquier controlador de dominio ubicado en el mismo dominio. Si el equipo es un controlador de dominio, intenta sincronizarlo con un controlador de dominio más autoritativo.  
  
Los equipos que ejecutan Windows XP Home Edition o equipos que no están Unidos a un dominio no intentan sincronizarse con la jerarquía de dominios, pero se configuran de forma predeterminada para obtener la hora de time.windows.com.  
  
Para establecer un equipo que ejecute Windows Server 2003 como autoritativo, el equipo debe estar configurado para ser un origen de hora confiable. De forma predeterminada, el primer controlador de dominio que está instalado en un dominio de Windows Server 2003 se configura automáticamente para que sea un origen de hora confiable. Dado que es el equipo autoritativo para el dominio, debe configurarse para que se sincronice con un origen de hora externo, en lugar de con la jerarquía de dominios. Además, de forma predeterminada, todos los demás miembros de dominio de Windows Server 2003 están configurados para sincronizarse con la jerarquía de dominios.  
  
Después de establecer una red de Windows Server 2003, puede configurar el servicio de hora de Windows para que use una de las siguientes opciones para la sincronización:  
  
-   Sincronización basada en jerarquía de dominios  
  
-   Un origen de sincronización especificado manualmente  
  
-   Todos los mecanismos de sincronización disponibles  
  
-   Sin sincronización.  
  
Cada uno de estos tipos de sincronización se describe en la sección siguiente.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronización basada en jerarquía de dominios  

La sincronización basada en una jerarquía de dominios usa el AD DS jerarquía de dominios para buscar una fuente confiable con la que sincronizar la hora. En función de la jerarquía de dominios, el servicio de hora de Windows determina la precisión de cada servidor horario. En un bosque de Windows Server 2003, el equipo que contiene la función de maestro de operaciones del emulador del controlador de dominio principal (PDC), ubicado en el dominio raíz del bosque, contiene la posición del mejor origen de hora, a menos que se haya configurado otro origen de hora confiable. En la ilustración siguiente se muestra una ruta de acceso de sincronización de hora entre equipos en una jerarquía de dominios.  
  
**Sincronización de hora en una jerarquía de AD DS**  
![Windows Time @ no__t-1
  
#### <a name="reliable-time-source-configuration"></a>Configuración de origen de tiempo de confianza  
Un equipo configurado para ser un origen de hora confiable se identifica como la raíz del servicio de hora. La raíz del servicio de hora es el servidor autoritativo para el dominio y se configura normalmente para recuperar la hora de un servidor NTP externo o un dispositivo de hardware. Un servidor horario puede configurarse como un origen de hora confiable para optimizar el tiempo que se transfiere a través de la jerarquía de dominios. Si un controlador de dominio está configurado para ser un origen de hora confiable, el servicio Inicio de sesión de red anuncia ese controlador de dominio como un origen de hora confiable cuando inicia sesión en la red. Cuando otros controladores de dominio buscan un origen de hora con el que sincronizar, eligen primero una fuente confiable si hay alguna disponible.  
  
#### <a name="time-source-selection"></a>Selección de origen de hora  
El proceso de selección de origen de tiempo puede crear dos problemas en una red:  
  
-   Ciclos de sincronización adicionales.  
  
-   Aumento del volumen en el tráfico de red.  
  
Un ciclo en la red de sincronización se produce cuando el tiempo permanece coherente entre un grupo de controladores de dominio y se comparte la misma hora entre ellos continuamente sin necesidad de resincronizar con otro origen de hora confiable. El algoritmo de selección de origen de hora del servicio de hora de Windows está diseñado para protegerse frente a estos tipos de problemas.  
  
Un equipo usa uno de los métodos siguientes para identificar un origen de hora con el que sincronizar:  
  
-   Si el equipo no es miembro de un dominio, debe configurarse para que se sincronice con un origen de hora especificado.  
  
-   Si el equipo es un servidor miembro o una estación de trabajo dentro de un dominio, de forma predeterminada sigue el AD DS jerarquía y sincroniza su hora con un controlador de dominio en su dominio local que está ejecutando actualmente el servicio de hora de Windows.  
  
Si el equipo es un controlador de dominio, realiza hasta seis consultas para encontrar otro controlador de dominio con el que sincronizar. Cada consulta está diseñada para identificar un origen de hora con determinados atributos, como un tipo de controlador de dominio, una ubicación determinada y si es o no un origen de hora confiable. El origen de la hora también debe cumplir las restricciones siguientes:  
  
-   Un origen de hora confiable solo puede sincronizarse con un controlador de dominio en el dominio primario.  
  
-   Un emulador de PDC se puede sincronizar con un origen de hora confiable en su propio dominio o en cualquier controlador de dominio del dominio primario.  
  
Si el controlador de dominio no se puede sincronizar con el tipo de controlador de dominio que está consultando, no se realiza la consulta. El controlador de dominio sabe a qué tipo de equipo puede obtener tiempo antes de que realice la consulta. Por ejemplo, un emulador de PDC local no intenta consultar los números tres o seis porque un controlador de dominio no intenta sincronizarse con sí mismo.  
  
En la tabla siguiente se enumeran las consultas que realiza un controlador de dominio para buscar un origen de hora y el orden en el que se realizan las consultas.  
  
**Consultas de origen de tiempo del controlador de dominio**  
  
|Número de consulta|Controlador de dominio|Location|Confiabilidad del origen de tiempo|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de dominio primario|En el sitio|Prefiere un origen de hora confiable, pero puede sincronizarse con un origen de hora no confiable si es todo lo que está disponible.|  
|2|Controlador de dominio local|En el sitio|Solo se sincroniza con un origen de hora confiable.|  
|3|Emulador de PDC local|En el sitio|No se aplica.<br /><br />Un controlador de dominio no intenta sincronizarse con sí mismo.|  
|4|Controlador de dominio primario|Fuera de sitio|Prefiere un origen de hora confiable, pero puede sincronizarse con un origen de hora no confiable si es todo lo que está disponible.|  
|5|Controlador de dominio local|Fuera de sitio|Solo se sincroniza con un origen de hora confiable.|  
|6|Emulador de PDC local|Fuera de sitio|No se aplica.<br /><br />Un controlador de dominio no intenta sincronizarse con sí mismo.| 
  
**Nota:**  
  
-   Un equipo nunca se sincroniza con sí mismo. Si el equipo que intenta la sincronización es el emulador PDC local, no intenta las consultas 3 o 6.  
  
Cada consulta devuelve una lista de controladores de dominio que se pueden usar como origen de hora. La hora de Windows asigna a cada controlador de dominio que se consulta una puntuación en función de la confiabilidad y la ubicación del controlador de dominio. En la tabla siguiente se enumeran las puntuaciones asignadas por la hora de Windows a cada tipo de controlador de dominio.  
  
**Determinación de puntuación**  
  
|Estado del controlador de dominio|Puntuación|  
|----------------------------|---------|  
|Controlador de dominio ubicado en el mismo sitio|8|  
|Controlador de dominio marcado como origen de hora confiable|4|  
|Controlador de dominio ubicado en el dominio primario|2|  
|Controlador de dominio que es un emulador de PDC|1|  
  
Cuando el servicio de hora de Windows determina que ha identificado el controlador de dominio con la mejor puntuación posible, no se realizan más consultas. Las puntuaciones asignadas por el servicio de hora son acumulativas, lo que significa que un emulador de PDC ubicado en el mismo sitio recibe una puntuación de nueve.  
  
Si la raíz del servicio de hora no está configurada para sincronizarse con un origen externo, el reloj de hardware interno del equipo rige el tiempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronización especificada manualmente  
La sincronización especificada manualmente le permite designar un solo elemento del mismo nivel o lista de elementos del mismo nivel de los que un equipo obtiene la hora. Si el equipo no es miembro de un dominio, debe configurarse manualmente para sincronizarse con un origen de hora especificado. Un equipo que es miembro de un dominio está configurado de forma predeterminada para sincronizar desde la jerarquía de dominios, la sincronización especificada manualmente es muy útil para la raíz del bosque del dominio o para los equipos que no están Unidos a un dominio. La especificación manual de un servidor NTP externo para sincronizar con el equipo autoritativo del dominio proporciona una hora confiable. Sin embargo, la configuración del equipo autoritativo para su dominio para sincronizar con un reloj de hardware es realmente una solución mejor para proporcionar el tiempo más preciso y seguro para su dominio.  
  
Los orígenes de hora especificados manualmente no se autentican a menos que se escriba un proveedor de hora específico para ellos y, por tanto, son vulnerables a los atacantes. Además, si un equipo se sincroniza con un origen especificado manualmente en lugar de su controlador de dominio de autenticación, es posible que los dos equipos no estén sincronizados, lo que provocaría un error en la autenticación Kerberos. Esto podría provocar errores en otras acciones que requieran la autenticación de red, como la impresión o el uso compartido de archivos. Si solo la raíz del bosque está configurada para sincronizarse con un origen externo, el resto de equipos del bosque permanecen sincronizados entre sí, lo que dificulta los ataques de reproducción.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos los mecanismos de sincronización disponibles  

La opción "todos los mecanismos de sincronización disponibles" es el método de sincronización más valioso para los usuarios de una red. Este método permite la sincronización con la jerarquía de dominios y también puede proporcionar un origen de tiempo alternativo si la jerarquía de dominios deja de estar disponible, dependiendo de la configuración. Si el cliente no puede sincronizar la hora con la jerarquía de dominios, el origen de hora recurre automáticamente al origen de hora especificado por el valor **NtpServer** . Es más probable que este método de sincronización proporcione un tiempo preciso a los clientes.  

### <a name="stopping-time-synchronization"></a>Detención de la sincronización de hora  
Existen ciertas situaciones en las que deseará impedir que un equipo Sincronice su tiempo. Por ejemplo, si un equipo intenta sincronizar desde un origen de hora en Internet o desde otro sitio a través de una WAN por medio de una conexión de acceso telefónico, puede suponer costosas cargos por teléfono. Cuando se deshabilita la sincronización en ese equipo, se impide que el equipo intente obtener acceso a un origen de hora a través de una conexión de acceso telefónico.  
  
También puede deshabilitar la sincronización para evitar la generación de errores en el registro de eventos. Cada vez que un equipo intenta sincronizarse con un origen de hora que no está disponible, genera un error en el registro de eventos. Si un origen de hora se desconecta de la red para el mantenimiento programado y no tiene intención de volver a configurar el cliente para sincronizar desde otro origen, puede deshabilitar la sincronización en el cliente para evitar que intente la sincronización mientras el servidor horario no está disponible.  
  
Resulta útil deshabilitar la sincronización en el equipo que se designa como raíz de la red de sincronización. Esto indica que el equipo raíz confía en su reloj local. Si la raíz de la jerarquía de sincronización no está establecida en **nosync** y no puede sincronizarse con otro origen de tiempo, los clientes no aceptan el paquete que este equipo envía porque no es de confianza.
  
Los únicos servidores horarios en los que los clientes confían incluso si no se han sincronizado con otro origen de hora son los identificados por el cliente como servidores de hora confiables.  
  
### <a name="disabling-the-windows-time-service"></a>Deshabilitar el servicio de hora de Windows  
El servicio de hora de Windows (W32Time) se puede deshabilitar completamente. Si decide implementar un producto de sincronización de hora de terceros que utiliza NTP, debe deshabilitar el servicio de hora de Windows. Esto se debe a que todos los servidores NTP necesitan tener acceso al puerto 123 del Protocolo de datagramas de usuario (UDP) y, siempre y cuando el servicio de hora de Windows se esté ejecutando en el sistema operativo Windows Server 2003, el puerto 123 permanece reservado por la hora de Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Puertos de red usados por el servicio de hora de Windows  
El servicio de hora de Windows se comunica en una red para identificar los orígenes de hora confiables, obtener información de hora y proporcionar información de tiempo a otros equipos. Realiza esta comunicación tal y como se define en las RFC de NTP y SNTP.  
  
**Asignaciones de puerto para el servicio de hora de Windows**  
  
|Nombre del servicio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Vea también  
[Referencia técnica del servicio de hora de windows](windows-time-service-tech-ref.md)
[herramientas y configuración del servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md)
 artículo de[Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)