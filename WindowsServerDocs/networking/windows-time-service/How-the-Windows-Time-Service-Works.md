---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Cómo funciona el servicio hora de Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: c9ab52229c2a16d6ae6b0b2733c52e53a25cfa44
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2018
---
# <a name="how-the-windows-time-service-works"></a>Cómo funciona el servicio hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**En esta sección**  
  
-   [Arquitectura del servicio de tiempo de Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocolos de tiempo de servicio de tiempo de Windows](#w2k3tr_times_how_ekoc)  
  
-   [Servicio procesos e interacciones de hora de Windows](#w2k3tr_times_how_izcr)  
  
-   [Puertos de red usados por el servicio hora de Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> Este tema explica solo cómo funciona el servicio hora de Windows (W32Time). Para obtener información sobre cómo configurar el servicio hora de Windows, consulta la lista de temas en la sección [dónde encontrar la información de configuración de Windows tiempo servicio](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina servicio de directorio de Active Directory. En Windows Server 2008 y versiones posteriores, el servicio de directorio se llama servicios de dominio de Active Directory (AD DS). El resto de este tema se refiere a AD DS, pero la información también es aplicable a Active Directory.  
  
Aunque el servicio hora de Windows no es una implementación exacta de protocolo de tiempo de red (NTP), usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para garantizar que los relojes de los equipos de una red sean lo más precisos posible. Idealmente, todos los relojes de los equipos en un dominio de AD DS se sincronizan con la hora de un equipo autorizado. Muchos factores pueden afectar la sincronización de hora en una red. A menudo, los siguientes factores afectan a la precisión de la sincronización en AD DS:  
  
-   Condiciones de red  
  
-   La precisión de reloj de hardware del equipo  
  
-   La cantidad de recursos de CPU y de red disponibles para el servicio hora de Windows  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de aplicación sujetos a limitación temporal.  Sin embargo, las actualizaciones de Windows Server 2016 ahora te permiten implementar una solución para 1 ms precisión en su dominio.  Consulta [Windows 2016 preciso tiempo](accurate-time.md) y [límite de soporte para configurar el servicio hora de Windows para entornos de alta precisión](https://go.microsoft.com/fwlink/?LinkID=179459) para obtener más información.  
  
Se configuran los equipos que sincronizan su tiempo con menos frecuencia o no están unidos a un dominio, de manera predeterminada, se sincronizarán con time.windows.com. Por lo tanto, es imposible garantizar la exactitud de tiempo en equipos que tienen intermitente o sin conexiones de red.  
  
Un bosque de AD DS tiene una jerarquía de sincronización de hora predeterminada. El servicio hora de Windows sincroniza el tiempo entre equipos dentro de la jerarquía, con los relojes de referencia más precisas en la parte superior. Si más de un origen de tiempo se configura en un equipo, hora de Windows usa algoritmos NTP para seleccionar el origen de tiempo recomendado de los orígenes configurados en función de la capacidad del equipo para sincronizar con ese origen de tiempo. El servicio hora de Windows no admite la sincronización de la red de difusión o multidifusión sistemas del mismo nivel. Para obtener más información acerca de estas características NTP, consulte RFC 1305 en la base de datos de IETF RFC.  
  
Todos los equipos que se está ejecutando el servicio hora de Windows usa el servicio para mantener el tiempo más preciso. Los equipos que pertenecen a un dominio actúan como un cliente de tiempo de manera predeterminada, por lo tanto, en la mayoría de los casos no es necesario configurar el servicio hora de Windows. Sin embargo, el servicio hora de Windows puede configurarse para solicitar tiempo desde un origen en tiempo de referencia designado y también se puede proporcionar el tiempo a los clientes.
  
El grado de precisión de hora de un equipo se denomina una capa. El origen de tiempo más preciso en una red (por ejemplo, un reloj de hardware) ocupa el nivel más bajo de capa o capa uno. Esta fuente precisa de tiempo se denomina un reloj de referencia. Un servidor NTP que adquiere su tiempo directamente desde un reloj de referencia ocupa una capa que está un nivel más alto que el reloj de referencia. Recursos que adquieren la hora del servidor NTP dos pasos fuera el reloj de referencia, por lo tanto ocupan una capa que dos es mayor que el origen de tiempo más preciso y así sucesivamente. Como el número de capa de un equipo aumenta, la hora del reloj del sistema puede ser menos precisa. Por lo tanto, el nivel de la capa de cualquier equipo es un indicador de cercanía ese equipo se sincroniza con el origen de tiempo más preciso.  
  
Cuando el Administrador de W32Time recibe ejemplos de tiempo, usa algoritmos especiales en NTP para determinar cuál de las muestras de tiempo es la más adecuada para su uso. El servicio hora de también usa otro conjunto de algoritmos para determinar cuál de los orígenes de tiempo configurado es más preciso. Cuando el servicio hora de ha determinado que muestra tiempo es mejor, en función de los criterios descritos anteriormente, ajusta la velocidad del reloj local para permitir que convergen hacia el momento correcto. Si la diferencia de tiempo entre el reloj local y la muestra de tiempo preciso seleccionado (también llamado el tiempo sesgo) es demasiado grande para corregir ajustando la velocidad del reloj local, el servicio hora de establece el reloj local en el momento correcto. Este ajuste de velocidad del reloj o cambio de tiempo de reloj directo se conoce como una enorme disciplina del reloj.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Arquitectura del servicio de tiempo de Windows  
El servicio hora de Windows se compone de los siguientes componentes:  
  
-   Administrador de Control de servicio  
  
-   Administrador de servicios de tiempo de Windows  
  
-   Enorme disciplina de reloj  
  
-   Proveedores de hora  
  
La siguiente figura muestra la arquitectura del servicio hora de Windows.  
  
**Arquitectura del servicio de tiempo de Windows**  
  
![Hora de Windows](media/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)  
  
El Administrador de Control de servicio es responsable de iniciar y detener el servicio hora de Windows. El Administrador de servicios de tiempo de Windows es responsable de iniciar la acción de los proveedores de tiempo NTP incluidos con el sistema operativo. El Administrador de servicios de tiempo de Windows controla todas las funciones del servicio hora de Windows y la combinación de todos los ejemplos de tiempo. Además que proporciona información sobre el estado actual del sistema, como el origen en tiempo actual o la última vez que el reloj del sistema se actualizó, el Administrador de servicios de tiempo de Windows también es responsable de crear eventos en el registro de eventos.  
  
El proceso de sincronización de hora implica los pasos siguientes:  
  
-   Solicitud de proveedores de entrada y recibir ejemplos de tiempo desde orígenes de tiempo NTP configurados.  
  
-   Estas muestras de tiempo, a continuación, se pasan al tiempo servicio Administrador de Windows, que se recopila todas las muestras y los pasa el subcomponente de reloj una enorme disciplina.  
  
-   El subcomponente disciplina de reloj aplica a los algoritmos NTP lo que genera la selección de la muestra de tiempo recomendada.  
  
-   El subcomponente disciplina de reloj ajusta la hora del reloj del sistema a la hora más precisa ajustar la velocidad del reloj o cambiar directamente el tiempo.  
  
Si un equipo se ha designado como un servidor horario, puede enviar el tiempo en cualquier equipo que solicite la sincronización de hora en cualquier punto de este proceso.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocolos de tiempo de servicio de tiempo de Windows  

Los protocolos de tiempo determinan los equipos de la cercanía dos relojes están sincronizados. Un protocolo de tiempo es responsable de determinar la mejor información del tiempo disponible y que converge los relojes para garantizar que se mantenga un tiempo consistente en sistemas independientes.  
  
El servicio hora de Windows usa el protocolo de tiempo de red (NTP) para ayudar a sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluya los algoritmos disciplina necesarios para sincronizar el reloj. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se usa en algunas versiones de Windows; Sin embargo W32Time continúa admitiendo SNTP para habilitar la compatibilidad con equipos que ejecutan servicios basados en SNTP tiempo, como Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo de tiempo de red  
Protocolo de tiempo de red (NTP) es el valor predeterminado usada por el servicio hora de Windows en el sistema operativo de protocolo de sincronización de hora. NTP es un protocolo de tiempo de tolerancia, escalable y el protocolo que se usa con más frecuencia para sincronizar los relojes de los equipos mediante una referencia de tiempo designado.  
  
Sincronización de hora NTP tiene lugar durante un período de tiempo e implica a la transferencia de paquetes NTP a través de una red. Paquetes NTP contienen las marcas de tiempo que incluyen una muestra de tiempo desde el cliente y el servidor que participe en la sincronización de hora.  
  
NTP se basa en un reloj de referencia para definir el tiempo que se usará más preciso y sincroniza los relojes en una red a ese reloj de referencia. NTP usa hora Universal coordinada (UTC) como el estándar universal para la hora actual. UTC es independiente de las zonas horarias y permite NTP para usarse en cualquier lugar del mundo independientemente de la configuración de zona horaria.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
NTP incluye dos algoritmos, un algoritmo de filtrado de reloj y un algoritmo de selección de reloj, para ayudar a determinar la muestra de tiempo recomendada el servicio hora de Windows. El algoritmo de filtrado de reloj está diseñado para ver ejemplos de tiempo que se reciben de orígenes de tiempo consultado y determinan los mejores ejemplos de tiempo de cada fuente de. El algoritmo de selección de reloj, a continuación, determina el servidor de tiempo más preciso de la red. Esta información a continuación, se pasa al algoritmo de disciplina reloj, que utiliza la información recopilada para corregir el reloj del equipo local mientras compensar errores debido a imprecisión de reloj de latencia y el equipo de red.  
  
Los algoritmos NTP son más precisos en condiciones de carga de red y del servidor de luz a moderada. Al igual que con cualquier algoritmo que tarda en cuenta durante el tránsito red, algoritmos NTP realizar mal en condiciones de congestión extremos de red. Para obtener más información acerca de los algoritmos NTP, consulte RFC 1305 en la base de datos de IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Proveedor de hora NTP  
El servicio hora de Windows es un paquete de la sincronización de hora completa que puede admitir una variedad de dispositivos de hardware y los protocolos de tiempo. Para habilitar esta función, el servicio usa proveedores de vez en marcha. Un proveedor de hora es responsable de cualquier obtener precisa las marcas de tiempo (de la red o de hardware) o para proporcionar las marcas de tiempo a otros equipos a través de la red.  
  
El proveedor NTP es el proveedor de hora estándar con el sistema operativo. El proveedor NTP sigue los estándares especificados por NTP versión 3 para un cliente y servidor y puede interactuar con los clientes SNTP y servidores por motivos de compatibilidad con Windows 2000 y otros clientes SNTP. El proveedor NTP en el servicio hora de Windows se compone de las siguientes dos partes:  
  
-   **Proveedor de salida NtpServer.** Este es un servidor horario que responda a solicitudes de tiempo del cliente en la red.  
  
-   **Proveedor de entrada NtpClient.** Se trata de un cliente de tiempo que se obtiene información en tiempo de otro origen, un dispositivo de hardware o un servidor NTP y puede devolver los ejemplos de tiempo que son útiles para sincronizar el reloj local.  
  
Aunque las operaciones reales de estas dos proveedores están estrechamente relacionadas, aparecen independientes para el servicio de hora. A partir de Windows 2000 Server, cuando un equipo de Windows está conectado a una red, se configura como un cliente NTP. Además, el mantenimiento de equipos que ejecutan la hora de Windows solo intento para sincronizar la hora con un controlador de dominio o un origen de tiempo especificado manualmente de manera predeterminada. Estos son los proveedores de hora preferido porque están disponibles automáticamente y seguras orígenes de tiempo.  
  
#### <a name="ntp-security"></a>Seguridad NTP  

Dentro de un bosque de AD DS, el servicio hora de Windows se basa en las características de seguridad de dominio estándar para aplicar la autenticación de los datos de tiempo. La seguridad de los paquetes NTP que se envían entre un equipo miembro del dominio y un controlador de dominio local que actúa como un servidor horario se basa en la autenticación de clave compartida. El servicio hora de Windows usa la clave de sesión de Kerberos del equipo para crear firmas autenticadas en los paquetes NTP que se envían a través de la red. No se transmiten paquetes NTP dentro del canal seguro de Net Logon. Ahora, cuando un equipo solicita la hora de un controlador de dominio en la jerarquía de dominios, el servicio hora de Windows requiere que se autentique el tiempo. El controlador de dominio, a continuación, devuelve la información necesaria en forma de un valor de 64 bits que se ha autenticado con la clave de sesión desde el servicio de Net Logon. Si el paquete NTP devuelto no está firmado con la clave de sesión del equipo o está firmado incorrectamente, se rechaza el tiempo. Todos estos errores de autenticación se registran en el registro de eventos. De este modo, el servicio hora de Windows proporciona seguridad de los datos NTP en un bosque de AD DS.  
  
Por lo general, los clientes de Windows tiempo Obtén automáticamente hora precisa de sincronización desde los controladores de dominio en el mismo dominio. En un bosque, los controladores de dominio de un dominio secundario sincronizan la hora con los controladores de dominio en sus dominios primarios. Cuando un servidor horario devuelve un paquete NTP autenticado a un cliente que solicita el tiempo, el paquete está firmado por medio de una clave de sesión de Kerberos definida por una cuenta de confianza entre dominios. Cuando un bosque une a un dominio de AD DS y el servicio de Net Logon administra la clave de sesión, se crea la cuenta de confianza entre dominios. De este modo, el controlador de dominio que esté configurado como confiable de dominio raíz del bosque se convierte en el origen de tiempo autenticado para todos los controladores de dominio de dominios los primario y secundario e indirectamente para todos los equipos que se encuentra en el árbol de dominio.  
  
El servicio hora de Windows puede configurarse para que funcione entre bosques, pero es importante tener en cuenta que esta configuración no es segura. Por ejemplo, un servidor NTP esté disponible en un bosque diferente. Sin embargo, porque ese equipo está en un bosque diferente, no hay ninguna clave de sesión de Kerberos con los que iniciar sesión y autenticar NTP paquetes. Para obtener la sincronización de hora precisa de un equipo en un bosque diferente, el cliente necesita acceso a la red a ese equipo y el servicio hora de debe configurarse para usar un origen de tiempo específico ubicado en el otro bosque. Si un cliente se configura manualmente a la hora de acceso desde un servidor NTP fuera de su propia jerarquía de dominio, los paquetes NTP enviados entre el cliente y el servidor de tiempo no se autentican y por lo tanto, no son seguros. Incluso con la implementación de confianzas de bosque, el servicio hora de Windows no es seguro entre bosques. Aunque el canal seguro de Net Logon es el mecanismo de autenticación para el servicio hora de Windows, no se admite la autenticación entre bosques.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware que son compatibles con el servicio hora de Windows  
Relojes basado en hardware como GPS o relojes de radio a menudo se usan como dispositivos de reloj de referencia de alta precisión. De manera predeterminada, el proveedor de tiempo NTP de servicio hora de Windows no admite la conexión directa de un dispositivo de hardware en un equipo, aunque es posible crear un proveedor de hora independientes basadas en software admite este tipo de conexión. Este tipo de proveedor, junto con el servicio hora de Windows, puede proporcionar una referencia de hora confiable y estable.  
  
Dispositivos de hardware, como un reloj de cesium o un receptor de sistema de posicionamiento Global (GPS), proporcionan tiempo actual precisa siguiendo un estándar para obtener una definición precisa de tiempo. Relojes cesium son muy estables y no se ven afectados por factores como la temperatura, la presión o humedad, pero también son muy costosos. Un receptor GPS es mucho menos costoso operar y también es un reloj de referencia precisa. Receptores GPS Obtén su tiempo de satélites que obtienen su hora desde un reloj cesium. Sin el uso de un proveedor de tiempo independientes, servidores de tiempo de Windows pueden adquirir su tiempo mediante la conexión a un servidor NTP externo, que está conectado a un dispositivo de hardware por medio de un teléfono o Internet. Las organizaciones como el Observatorio Naval de Estados Unidos ofrecen servidores NTP que están conectados a los relojes de referencia extremadamente confiable.  
  
Muchos receptores GPS y otros dispositivos de tiempo pueden funcionar como servidores NTP en una red. Puedes configurar el bosque de AD DS para sincronizar la hora de estos dispositivos de hardware externo solo si también actúan como servidores NTP en la red. Para ello, configura el controlador de dominio funciona como el controlador de dominio principal emulador (PDC) en la raíz del bosque para sincronizar con el servidor NTP proporcionado por el dispositivo GPS. Para ello, consulta [configurar el servicio hora de Windows en el emulador PDC en el dominio raíz](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo de tiempo de red simple  
El protocolo Simple de tiempo de red (SNTP) es un protocolo de tiempo simplificada que está destinado a servidores y clientes que no requieren el grado de precisión que proporciona NTP. SNTP, una versión más básica de NTP, es el protocolo de tiempo principal que se usa en Windows 2000. Dado que los formatos de paquetes de red de NTP y SNTP son idénticos, los dos protocolos son interoperables. La principal diferencia entre los dos es que SNTP no tiene la administración de errores y sistemas de filtrado complejos que proporciona NTP. Para obtener más información sobre el protocolo Simple de tiempo de red, consulte RFC 1769 en la base de datos de IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidad de protocolo de tiempo  
El servicio hora de Windows puede funcionar en un entorno de equipos que ejecutan Windows 2000, Windows XP y Windows Server 2003, porque el protocolo SNTP usado en Windows 2000 es interoperable con el protocolo NTP en Windows XP y Windows Server 2003.  
  
El servicio hora de Windows NT Server 4.0, llamado TimeServ, sincroniza tiempo en una red de Windows NT 4.0. TimeServ es una característica de complemento disponible como parte de la *Kit de recursos de Microsoft Windows NT 4.0* y no proporciona el grado de confiabilidad de sincronización de tiempo que se requiere Windows Server 2003.  
  
El servicio hora de Windows puede interoperar con equipos que ejecutan Windows NT 4.0, porque puede sincronizar la hora con equipos que ejecutan Windows 2000 o Windows Server 2003. Sin embargo, un equipo que ejecute Windows 2000 o Windows Server 2003 detecta automáticamente los servidores de hora de Windows NT 4.0. Por ejemplo, si tu dominio está configurado para sincronizar la hora mediante el dominio basado en la jerarquía de método de sincronización y desea que los equipos de la jerarquía de dominio para sincronizar la hora con un controlador de dominio de Windows NT 4.0, tienes que configurar esos equipos manualmente para sincronizar con los controladores de dominio de Windows NT 4.0.  
  
Windows NT 4.0 utiliza un mecanismo más simple para la sincronización de tiempo que el servicio hora de Windows que usa. Por lo tanto, para garantizar la sincronización de hora precisa a través de la red, se recomienda que actualices a Windows 2000 o Windows Server 2003 los controladores de dominio de Windows NT 4.0.  
  
## <a name="w2k3tr_times_how_izcr"></a>Servicio procesos e interacciones de hora de Windows  

El servicio hora de Windows está diseñado para sincronizar los relojes de los equipos de una red. El proceso de sincronización de la hora de red, también denominado convergencia de tiempo, se produce en toda la red como cada vez que accede a equipo desde un servidor de tiempo más preciso. Convergencia de tiempo implica un proceso que proporciona la hora actual en los equipos cliente en el formulario de paquetes NTP un servidor autorizado. La información proporcionada dentro de un paquete indica si se necesita un ajuste realizarse para la hora actual del equipo para que se sincroniza con el servidor más preciso.  
  
Como parte del proceso de convergencia de tiempo, los miembros del dominio intentan sincronizar la hora con cualquier controlador de dominio que se encuentra en el mismo dominio. Si el equipo es un controlador de dominio, intenta sincronizar con un controlador de dominio más autorizado.  
  
Equipos que ejecutan Windows XP Home Edition o los equipos que no están unidos a un dominio no intentará sincronizar con la jerarquía de dominios, pero se configuran de forma predeterminada para obtener la hora de time.windows.com.  
  
Para establecer un equipo que ejecute Windows Server 2003 como autorizado, el equipo debe configurarse para ser un origen de hora confiable. De manera predeterminada, el primer controlador de dominio que está instalado en un dominio de Windows Server 2003 se configura automáticamente para ser un origen de hora confiable. Dado que es el equipo autorizado para el dominio, se debe configurarse para sincronizar con una fuente externa, en lugar de con la jerarquía de dominios. También de manera predeterminada, todos los demás miembros de dominio de Windows Server 2003 se configuran para sincronizar con la jerarquía de dominios.  
  
Cuando se haya establecido una red de Windows Server 2003, puedes configurar el servicio hora de Windows para usar una de las siguientes opciones de sincronización:  
  
-   Sincronización de basado en la jerarquía de dominios  
  
-   Un origen de sincronización especificada manualmente  
  
-   Todos los mecanismos de sincronización disponibles  
  
-   Sin sincronización.  
  
Cada uno de estos tipos de sincronización se analiza en la siguiente sección.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronización de basado en la jerarquía de dominios  

Sincronización que se basa en una jerarquía de dominios, usa la jerarquía de dominio de AD DS para encontrar un origen de confianza con el que se va a sincronizar la hora. En función de la jerarquía de dominios, el servicio hora de Windows determina la precisión de cada servidor horario. En un bosque de Windows Server 2003, el equipo que contiene el dominio principal (PDC) emulador operaciones principal función de controlador, ubicada en el dominio raíz contiene la posición del origen de tiempo recomendado, a menos que se ha configurado otra fuente de hora confiable. La figura siguiente muestra una ruta de acceso de sincronización de tiempo entre equipos en una jerarquía de dominio.  
  
**Sincronización de hora en una jerarquía de AD DS**  
  
![Hora de Windows](media/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)  
  
#### <a name="reliable-time-source-configuration"></a>Configuración del origen de hora de confianza  
Un equipo que está configurado para ser un origen de la hora de confianza se identifica como la raíz del servicio de hora. La raíz del servicio de hora es el servidor autorizado para el dominio y normalmente se configura para recuperar la hora de un servidor NTP externo o un dispositivo de hardware. Un servidor horario puede configurarse como un origen de hora confiable para optimizar cómo se transfiere la hora a lo largo de la jerarquía de dominios. Si un controlador de dominio está configurado para ser un origen de hora confiable, servicio de Net Logon anuncia el controlador de dominio como un origen de hora confiable cuando inicia sesión en la red. Al buscan un recurso de hora sincronizar con otros controladores de dominio, elige un origen de confianza primero si está disponible.  
  
#### <a name="time-source-selection"></a>Selección de origen de tiempo  
El proceso de selección del origen de tiempo puede crear dos problemas en una red:  
  
-   Ciclos de sincronización adicional.  
  
-   Mayor volumen de tráfico de red.  
  
Un ciclo de la red de sincronización se produce cuando tiempo permanece coherente entre un grupo de controladores de dominio y el mismo tiempo que se comparte entre ellas continuamente sin una sincronización con otra fuente de hora confiable. Algoritmo de selección de origen de tiempo del servicio hora de Windows está diseñado para protegerse contra estos tipos de problemas.  
  
Un equipo usa uno de los siguientes métodos para identificar una fuente de tiempo para sincronizar con:  
  
-   Si el equipo no es miembro de un dominio, debe estar configurada para sincronizar con un origen de tiempo especificado.  
  
-   Si el equipo es un servidor miembro o estación de trabajo dentro de un dominio, de manera predeterminada, sigue la jerarquía de AD DS y sincroniza su hora con un controlador de dominio en su dominio local que se está ejecutando el servicio hora de Windows.  
  
Si el equipo es un controlador de dominio, es hasta seis consultas para buscar otro controlador de dominio para sincronizar con. Cada consulta está diseñada para identificar una fuente de tiempo con determinados atributos, como un tipo de controlador de dominio, una ubicación en particular, y si no es un recurso de hora confiable. El origen en tiempo también debe cumplir con las siguientes restricciones:  
  
-   Un origen de hora confiable solo puede sincronizarse con un controlador de dominio en el dominio principal.  
  
-   Un emulador PDC puede sincronizar con un origen de hora confiable en su propio dominio o cualquier controlador de dominio en el dominio principal.  
  
Si no puede sincronizar con el tipo de controlador de dominio que está consultando el controlador de dominio, no se realiza la consulta. El controlador de dominio sabe qué tipo de equipo que puede obtener el tiempo que transcurre desde antes de que realice la consulta. Por ejemplo, un emulador PDC local no intenta números de consulta tres o seis porque un controlador de dominio no intentará sincronizar con él mismo.  
  
La siguiente tabla enumera las consultas que hace que un controlador de dominio para encontrar una fuente de tiempo y el orden en que se realizan las consultas.  
  
**Consulta el origen de tiempo del controlador de dominio**  
  
|Número de consulta|Controlador de dominio|Ubicación|Confiabilidad de origen en tiempo de|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de dominio principal|En el sitio|Prefiere una confiable origen en tiempo, pero se puede sincronizar con un origen no confiable tiempo si eso es todo lo que está disponible.|  
|2|Controlador de dominio local|En el sitio|Solo se sincroniza con un origen de hora confiable.|  
|3|Emulador local|En el sitio|No se aplica.<br /><br />Un controlador de dominio no intentará sincronizar con él mismo.|  
|4|Controlador de dominio principal|Sitio de|Prefiere una confiable origen en tiempo, pero se puede sincronizar con un origen no confiable tiempo si eso es todo lo que está disponible.|  
|5|Controlador de dominio local|Sitio de|Solo se sincroniza con un origen de hora confiable.|  
|6|Emulador local|Sitio de|No se aplica.<br /><br />Un controlador de dominio no intentará sincronizar con él mismo.|  
  
**Ten en cuenta**  
  
-   Un equipo nunca se sincroniza con él mismo. Si el equipo intenta sincronización es el emulador PDC local, no se intenta consultas 3 o 6.  
  
Cada consulta devuelve una lista de controladores de dominio que se puede usar como origen de hora. Hora de Windows asigna cada controlador de dominio es consulta una puntuación en función de la confiabilidad y la ubicación del controlador de dominio. La siguiente tabla enumera las puntuaciones asignadas por hora de Windows para cada tipo de controlador de dominio.  
  
**Determinación de puntuación**  
  
|Estado del controlador de dominio|Puntuación|  
|----------------------------|---------|  
|Controlador de dominio que se encuentra en el mismo sitio|8|  
|Controlador de dominio se marca como un origen de hora confiable|4|  
|Controlador de dominio ubicado en el dominio principal|2|  
|Controlador de dominio que es un emulador PDC|1|  
  
Cuando el servicio hora de Windows determina que ha identificado el controlador de dominio con la mejor puntuación posible, no se realizan ninguna más consultas. Las puntuaciones asignadas por el servicio hora son acumulativas, lo que significa que un emulador PDC ubicado en el mismo sitio recibe una puntuación de nueve.  
  
Si la raíz del servicio de hora no está configurada para sincronizar con una fuente externa, el reloj del hardware interno del equipo rige el tiempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronización especificada manualmente  
Sincronización especificada manualmente le permite designar un único sistema del mismo nivel o la lista de sistemas del mismo nivel desde el que un equipo obtiene la hora. Si el equipo no es miembro de un dominio, se debe configurar manualmente para sincronizar con un origen de tiempo especificado. Un equipo que es que un miembro de un dominio está configurado para sincronizar de la jerarquía de dominios de manera predeterminada, la sincronización especificada manualmente es más útil para la raíz del bosque del dominio o para los equipos que no están unidos a un dominio. Especificar manualmente un servidor NTP externo para sincronizar con el equipo autorizado para tu dominio proporciona hora confiable. Sin embargo, al configurar el equipo autorizado para tu dominio para sincronizar con un reloj de hardware es realmente la mejor solución para proporcionar el tiempo más preciso y seguro con el dominio.  
  
No se autentican los orígenes de tiempo especificado manualmente a menos que se especifique un proveedor de tiempo específico para ellos y, por lo tanto, son vulnerables a los atacantes. Además, si un equipo se sincroniza con un origen especificada manualmente, en lugar de su controlador de dominio de autenticación, los dos equipos podrían no sincronizarse, lo que hace que la autenticación Kerberos producirá un error. Esto podría provocar otras acciones que requieren autenticación de red, un error, como impresión o uso compartido de archivos. Si solo la raíz del bosque está configurada para sincronizar con una fuente externa, todos los demás equipos del bosque permanecen sincronizados entre sí, lo dificulta que los ataques de reproducción.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos los mecanismos de sincronización disponibles  

La opción de "todos los mecanismos de sincronización disponibles" es el método de sincronización más valioso para los usuarios en una red. Este método permite la sincronización con la jerarquía de dominios y también puede proporcionar una alternativa origen en tiempo de si la jerarquía de dominio no está disponible, según la configuración. Si el cliente no puede sincronizar la hora con la jerarquía de dominios, el origen en tiempo automáticamente retrocede para el origen de tiempo especificado por el **NtpServer** configuración. Este método de sincronización es más probable que proporcionan tiempo precisa a los clientes.  

### <a name="stopping-time-synchronization"></a>Detener la sincronización de hora  
Existen determinadas situaciones en que quieres impedir que un equipo sincronice la hora. Por ejemplo, si un equipo intenta sincronizar desde un origen de vez en Internet o de otro sitio a través de una WAN mediante una conexión de acceso telefónico, puede verse tarifas telefónicas costosa. Cuando se deshabilita la sincronización en ese equipo, evitar que el equipo intenta acceder a una fuente de tiempo mediante una conexión de acceso telefónico.  
  
También puede deshabilitar la sincronización para evitar la generación de errores en el registro de eventos. Cada vez que un equipo intenta sincronizar con un origen de tiempo que no está disponible, genera un error en el registro de eventos. Si se toma un origen en tiempo de fuera de la red para su mantenimiento periódico y que no vayas a configurar el cliente para sincronizar de otro origen, puede deshabilitar la sincronización en el cliente para impedir que se intente sincronización mientras el servidor horario no está disponible.  
  
Es útil deshabilitar la sincronización en el equipo que está designada como la raíz de la red de sincronización. Esto indica que el equipo de raíz confía en el reloj local. Si la raíz de la jerarquía de sincronización no se establece en **NoSync** y si no puede sincronizar con otro origen de tiempo, los clientes no aceptan el paquete que envía este equipo porque el tiempo no es de confianza.  
  
La única vez que los servidores que son de confianza para los clientes incluso si no se hayan sincronizado con otro origen en tiempo de son los que se han identificado por el cliente como los servidores de hora confiable.  
  
### <a name="disabling-the-windows-time-service"></a>Deshabilitar el servicio hora de Windows  
El servicio hora de Windows (W32Time) puede estar deshabilitado por completo. Si optas por implementar un producto de sincronización de hora de terceros que utiliza NTP, debes deshabilitar el servicio hora de Windows. Esto es porque todos los servidores NTP tienen acceso al puerto de protocolo de datagramas de usuario (UDP) 123 y siempre y cuando el servicio hora de Windows se ejecuta en el sistema operativo Windows Server 2003, el puerto 123 permanece reservado por hora de Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Puertos de red usados por el servicio hora de Windows  
El servicio hora de Windows se comunica en una red para identificar los orígenes de hora confiable, obtener información de tiempo y proporcionar información de tiempo a otros equipos. Lleva a cabo esta comunicación definida por el NTP y SNTP RFC.  
  
**Asignaciones de puerto para el servicio hora de Windows**  
  
|Nombre de servicio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Consulta también  
[Referencia técnica del servicio hora de Windows](https://technet.microsoft.com/library/cc773061.aspx)  
[Herramientas de servicio hora de Windows y configuración](Windows-Time-Service-Tools-and-Settings.md)  
[Artículo de Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)  
  


