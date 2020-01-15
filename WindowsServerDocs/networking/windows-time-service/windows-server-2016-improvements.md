---
title: Mejoras en Windows Server 2016
description: Windows Server 2016 ha mejorado los algoritmos que usa para corregir el tiempo y la condición de que el reloj local se sincronice con la hora UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 37e37e33b5d8dd571f8519aaa48251856503578d
ms.sourcegitcommit: 10331ff4f74bac50e208ba8ec8a63d10cfa768cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75953091"
---
# <a name="windows-server-2016-improvements"></a>Mejoras en Windows Server 2016

En este artículo se incluye información sobre las mejoras realizadas en Windows Server 2016.

## <a name="windows-time-service-and-ntp"></a>Servicio de hora de Windows y NTP

Windows Server 2016 ha mejorado los algoritmos que usa para corregir el tiempo y la condición de que el reloj local se sincronice con la hora UTC. NTP usa 4 valores para calcular el desplazamiento de tiempo en función de las marcas de tiempo de la solicitud/respuesta del cliente y la solicitud/respuesta del servidor. Sin embargo, las redes tienen ruido y puede haber picos en los datos de NTP debido a la congestión de la red y otros factores que afectan a la latencia de la red. Los algoritmos de Windows 2016 calculan el promedio de este ruido mediante una serie de técnicas diferentes, lo que da como resultado un reloj estable y exacto. Además, el origen que usamos para una hora precisa hace referencia a una API mejorada que nos proporciona una mejor solución. Con estas mejoras, podremos lograr una precisión de 1 MS con respecto a la hora UTC en un dominio.

## <a name="hyper-v"></a>Hyper-V

Windows 2016 ha mejorado el servicio TimeSync de Hyper-V. Entre las mejoras se incluye el tiempo inicial más preciso en el inicio de la máquina virtual o la corrección de la latencia de interrupción para los ejemplos proporcionados a W32Time. Esta mejora nos permite mantener el 10μs del host con un RMS (la media raíz cuadrada, que indica la varianza), de 50 μs, incluso en un equipo con una carga del 75%. Para obtener más información, consulte [arquitectura de Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> La carga se creó con la prueba comparativa de Prime95 mediante un perfil equilibrado.

Además, el nivel de estrato que el host notifica al invitado es más transparente. Anteriormente, el host presentaba una capa fija de 2, independientemente de su precisión. Con los cambios en Windows Server 2016, el host informa de un estrato uno mayor que el estrato del host, lo que da como resultado un mejor tiempo para los invitados virtuales. La estrato del host está determinada por W32Time a través de medios normales según su hora de origen. Los invitados de Windows 2016 Unidos a un dominio encontrarán el reloj más preciso, en lugar de tener como valor predeterminado el host. Por esta razón, se recomienda deshabilitar manualmente la configuración del proveedor de hora de Hyper-V para los equipos que participan en un dominio en Windows 2012R2 y versiones anteriores.

## <a name="monitoring"></a>Supervisión
Se han agregado los contadores del monitor de rendimiento. Estos permiten la línea de base, la supervisión y la solución de problemas de precisión temporal. Estos contadores incluyen:

|Contador|Descripción|
|----- | ----- |
|Desplazamiento de tiempo calculado| El desplazamiento de tiempo absoluto entre el reloj del sistema y el origen de hora elegido, calculado por el servicio W32Time en microsegundos. Cuando hay disponible una nueva muestra válida, el tiempo calculado se actualiza con el desplazamiento de tiempo indicado por el ejemplo. Este es el desplazamiento de tiempo real del reloj local. W32time inicia la corrección del reloj con este desplazamiento y actualiza el tiempo calculado entre las muestras con el desplazamiento de tiempo restante que se debe aplicar al reloj local. Se puede realizar un seguimiento de la precisión del reloj mediante este contador de rendimiento con un intervalo de sondeo bajo (p. ej., 256 segundos o menos) y buscando el valor del contador en un valor menor que el límite de precisión del reloj deseado.|
|Ajuste de la frecuencia del reloj| El ajuste de la frecuencia del reloj absoluta realizado en el reloj del sistema local por W32Time en partes por mil millones. Este contador ayuda a visualizar las acciones que realiza W32time.|
|Retraso de ida y vuelta NTP| Retraso de ida y vuelta más reciente producido por el cliente NTP al recibir una respuesta del servidor en microsegundos. Este es el tiempo transcurrido en el cliente NTP entre la transmisión de una solicitud al |El servidor NTP y la recepción de una respuesta válida desde el servidor. Este contador ayuda a caracterizar los retrasos que experimenta el cliente NTP. Los intercambios mayores o variables pueden agregar ruido a los cálculos de tiempo de NTP, lo que a su vez puede afectar a la precisión de la sincronización de tiempo a través de NTP.|
|Recuento de orígenes de cliente NTP| Número activo de orígenes de tiempo NTP utilizados por el cliente NTP. Se trata de un recuento de direcciones IP distintas y activas de los servidores de tiempo que responden a las solicitudes de este cliente. Este número puede ser mayor o menor que los elementos del mismo nivel configurados, en función de la resolución de DNS de los nombres del mismo nivel y del alcance actual.|
|Solicitudes entrantes del servidor NTP| Número de solicitudes recibidas por el servidor NTP (solicitudes por segundo).|
|Respuestas salientes del servidor NTP| Número de solicitudes respondidas por el servidor NTP (respuestas/seg.).|

Los tres primeros contadores son escenarios de destino para solucionar problemas de precisión. En las prácticas recomendadas, la precisión del tiempo de solución de problemas y la sección NTP, en procedimientos recomendados, tiene más detalles.
Los tres últimos contadores cubren los escenarios de servidor NTP y son útiles cuando se determina la carga y se establece una línea de su rendimiento actual.

## <a name="configuration-updates-per-environment"></a>Actualizaciones de configuración por entorno
A continuación se describen los cambios en la configuración predeterminada entre Windows 2016 y las versiones anteriores para cada rol. La configuración de Windows Server 2016 y de la actualización de aniversario de Windows 10 (compilación 14393) ahora es única, que es el motivo por el que se muestran como columnas independientes.

|Rol|Configuración|Windows Server 2016|10 de Windows|R2 de Windows 2012 Server<br>Windows Server 2008 R2<br>10 de Windows|
|---|---|---|---|---|
|**Independiente/nano Server**||||
| |*Servidor horario*|time.windows.com|N/A|time.windows.com|
| |*Frecuencia de sondeo*|64-1024 segundos|N/A|Una vez a la semana|
| |*Frecuencia de actualización del reloj*|Una vez por segundo|N/A|Una vez a la hora|
|**Cliente independiente**||||
| |*Servidor horario*|N/A|time.windows.com|time.windows.com|
| |*Frecuencia de sondeo*|N/A|Una vez al día|Una vez a la semana|
| |*Frecuencia de actualización del reloj*|N/A|Una vez al día|Una vez a la semana|
|**Controlador de dominio**||||
| |*Servidor horario*|PDC/GTIMESERV|N/A|PDC/GTIMESERV|
| |*Frecuencia de sondeo*|64-1024 segundos|N/A|1024-32768 segundos|
| |*Frecuencia de actualización del reloj*|Una vez al día|N/A|Una vez a la semana|
|**Servidor miembro de dominio**||||
| |*Servidor horario*|DC|N/A|DC|
| |*Frecuencia de sondeo*|64-1024 segundos|N/A|1024-32768 segundos|
| |*Frecuencia de actualización del reloj*|Una vez por segundo|N/A|Una vez cada 5 minutos|
|**Cliente miembro de dominio**||||
| |*Servidor horario*|N/A|DC|DC|
| |*Frecuencia de sondeo*|N/A|1204-32768 segundos|1024-32768 segundos|
| |*Frecuencia de actualización del reloj*|N/A|Una vez cada 5 minutos|Una vez cada 5 minutos|
|**Invitado de Hyper-V**||||
| |*Servidor horario*|Elige la mejor opción en función del estrato del host y el servidor de tiempo|Elige la mejor opción en función del estrato del host y el servidor de tiempo|El valor predeterminado es host|
| |*Frecuencia de sondeo*|Según el rol anterior|Según el rol anterior|Según el rol anterior|
| |*Frecuencia de actualización del reloj*|Según el rol anterior|Según el rol anterior|Según el rol anterior|

>[!NOTE]
>Para Linux en Hyper-V, consulte la sección permitir que Linux use el tiempo de host de Hyper-V a continuación.

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto del aumento del sondeo y la frecuencia de actualización del reloj

Con el fin de proporcionar un tiempo más preciso, los valores predeterminados para las frecuencias de sondeo y las actualizaciones del reloj aumentan, lo que nos permite realizar pequeños ajustes con mayor frecuencia. Esto provocará más tráfico UDP/NTP; sin embargo, estos paquetes son pequeños, por lo que debería haber muy poco o ningún impacto en los vínculos de banda ancha. Sin embargo, la ventaja es que el tiempo debe ser mejor en una variedad más amplia de hardware y entornos.

En el caso de los dispositivos con respaldo de batería, el aumento de la frecuencia de sondeo puede producir problemas. Los dispositivos de batería no almacenan el tiempo mientras están desactivados. Cuando se reanudan, puede que requieran correcciones frecuentes en el reloj. El aumento de la frecuencia de sondeo hará que el reloj se vuelve inestable y también pueda usar más energía. Microsoft recomienda no cambiar la configuración predeterminada del cliente.

Los controladores de dominio deben verse afectados mínimamente, incluso con el efecto multiplicado del aumento de las actualizaciones de los clientes NTP en un dominio de AD. NTP tiene un consumo de recursos mucho más pequeño en comparación con otros protocolos y un impacto marginal. Es más probable que alcance los límites de otras funciones de dominio antes de verse afectado por la mayor configuración de Windows Server 2016. Active Directory usa NTP seguro, que tiende a sincronizar el tiempo con menos precisión que el NTP simple, pero hemos comprobado que se escalará verticalmente a los clientes dos estrato fuera del PDC.

Como plan conservador, debe reservar 100 solicitudes NTP por segundo por núcleo. Por ejemplo, un dominio formado por 4 controladores de dominio con 4 núcleos cada uno, debería poder atender 1600 solicitudes NTP por segundo. Si tiene 10 000 clientes configurados para sincronizar la hora una vez cada 64 segundos y las solicitudes se reciben uniformemente a lo largo del tiempo, verá 10000/64 o aproximadamente 160 solicitudes por segundo, distribuidas en todos los controladores de dominios. Esto se encuentra fácilmente dentro de las solicitudes de 1600 NTP por segundo basadas en este ejemplo. Se trata de recomendaciones de planeación conservadora y, por supuesto, tienen una gran dependencia en la red, velocidades y cargas de procesador, por lo que siempre se trata de una línea de base y pruebas en los entornos.

También es importante tener en cuenta que si los controladores de dominio se ejecutan con una carga de CPU considerable, mayor del 40%, se agregará el ruido a las respuestas de NTP y afectará a la precisión del tiempo en el dominio. De nuevo, debe probar en su entorno para comprender los resultados reales.

## <a name="time-accuracy-measurements"></a>Medidas de precisión temporal
### <a name="methodology"></a>Metodología
Para medir la precisión temporal de Windows Server 2016, usamos una variedad de herramientas, métodos y entornos. Puede usar estas técnicas para medir y ajustar el entorno y determinar si los resultados de precisión satisfacen sus necesidades. 

Nuestro reloj de origen de dominio constaba de dos servidores NTP de alta precisión con hardware GPS. También usamos una máquina de prueba de referencia independiente para las medidas, que también tenían hardware GPS de alta precisión instalado de otro fabricante. En el caso de algunas de las pruebas, necesitará un origen de reloj preciso y confiable para usarlo como referencia además del origen del reloj del dominio.

Usamos cuatro métodos diferentes para medir la precisión con las máquinas físicas y virtuales. Varios métodos proporcionan medios independientes para validar los resultados.

1. Mida el reloj local, que está condicionado por W32tm, en la máquina de prueba de referencia que tiene hardware GPS independiente. 
2. Mida los pings NTP desde el servidor NTP a los clientes que usen W32tm "Stripchart"
3. Mida los pings NTP desde el cliente al servidor NTP mediante W32tm "Stripchart".
4. Mida los resultados de Hyper-V del host al invitado mediante el contador de marca de tiempo (TSC). Este contador se comparte entre ambas particiones y la hora del sistema en ambas particiones. Calculamos la diferencia entre el tiempo de host y el tiempo de cliente en la máquina virtual. A continuación, usamos el reloj de TSC para interpolar la hora del host desde el invitado, ya que las medidas no se realizan al mismo tiempo. Además, usamos el retraso y la latencia del factor de reloj TSV en la API.

W32tm está integrado, pero las otras herramientas que usamos durante las pruebas están disponibles para el repositorio de Microsoft en GitHub como código abierto para las pruebas y el uso. El WIKI del repositorio tiene más información que describe cómo usar las herramientas para realizar mediciones.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Los resultados de pruebas que se muestran a continuación son un subconjunto de las medidas realizadas en uno de los entornos de prueba. Ilustran la precisión que se mantiene al principio de la jerarquía de tiempo y el cliente de dominio secundario al final de la jerarquía de tiempo. Se compara con las mismas máquinas en una topología basada en 2012 para realizar la comparación.

### <a name="topology"></a>Topología
Para la comparación, hemos probado una topología basada en Windows Server 2012R2 y Windows Server 2016. Ambas topologías constan de dos equipos host de Hyper-V físicos que hacen referencia a un equipo con Windows Server 2016 con hardware de reloj de GPS instalado. Cada host ejecuta 3 invitados de Windows Unidos a un dominio, que se organizan según la siguiente topología. Las líneas representan la jerarquía de tiempo y el protocolo o transporte que se utiliza.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Información general sobre los resultados gráficos
Los dos gráficos siguientes representan la precisión temporal para dos miembros específicos de un dominio en función de la topología anterior. Cada gráfico muestra los resultados 2012R2 y 2016 de Windows Server, que muestran las mejoras visualmente. La precisión era Measure desde con, en la máquina invitada en comparación con el host. Los datos gráficos representan un subconjunto del conjunto completo de pruebas que hemos realizado y muestran los escenarios de las mejores mayúsculas y minúsculas. 

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Rendimiento del PDC del dominio raíz
El PDC raíz se sincroniza con el host de Hyper-V (mediante VMIC), que es un Windows Server 2016 con hardware GPS que se ha demostrado que es preciso y estable. Se trata de un requisito crítico para una precisión de 1 MS, que se muestra como el área sombreada de color verde.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Rendimiento del cliente del dominio secundario
El cliente de dominio secundario se adjunta a un PDC de dominio secundario que se comunica con el PDC raíz. El tiempo de ti también está dentro del requisito de 1 ms.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>Prueba de larga distancia
En el gráfico siguiente se compara un salto de red virtual con 6 saltos de red físicos con Windows Server 2016. Dos gráficos están superpuestos entre sí con transparencia para mostrar los datos superpuestos. Aumentar los saltos de red significa una latencia mayor y desviaciones de tiempo mayores. El gráfico se amplía y, por lo tanto, los límites de 1 MS, representados por el área verde, son más grandes. Como puede ver, el tiempo sigue siendo de 1 MS con varios saltos. Se desplaza negativamente, lo que muestra una asimetría de red. Por supuesto, cada red es diferente y las mediciones dependen de una gran variedad de factores ambientales.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>Prácticas recomendadas para timekeeping precisos
### <a name="solid-source-clock"></a>Reloj de origen sólido
La hora de los equipos es tan buena como el reloj de origen con el que se sincroniza. Para lograr una precisión de 1 MS, necesitará hardware GPS o un dispositivo de tiempo en la red al que haga referencia como el reloj de origen principal. Si se usa el valor predeterminado de time.windows.com, es posible que no proporcione un origen de hora estable y local. Además, a medida que se aleja del reloj de origen, la red afecta a la precisión. Es necesario tener un reloj de origen maestro en cada centro de datos para obtener la mejor precisión.

### <a name="hardware-gps-options"></a>Opciones de GPS de hardware
Hay varias soluciones de hardware que pueden ofrecer un tiempo preciso. En general, las soluciones de hoy en día se basan en antenas GPS. También hay soluciones de radio y de acceso telefónico con líneas dedicadas. Se conectan a la red como un dispositivo o se conectan a un equipo, por ejemplo, las ventanas de instancia a través de un dispositivo PCIe o USB. Las distintas opciones proporcionarán diferentes niveles de precisión y, como siempre, los resultados dependen de su entorno. Las variables que afectan a la precisión incluyen la disponibilidad del GPS, la estabilidad y la carga de red y el hardware del equipo. Estos son algunos factores importantes a la hora de elegir un reloj de origen, como se indica, es un requisito de tiempo estable y preciso.

### <a name="domain-and-synchronizing-time"></a>Tiempo de sincronización y dominio
Los miembros del dominio usan la jerarquía de dominios para determinar qué máquina usan como origen para sincronizar la hora. Cada miembro de dominio encontrará otro equipo con el que sincronizar y lo guardará como origen del reloj. Cada tipo de miembro de dominio sigue un conjunto diferente de reglas para buscar un origen de reloj para la sincronización de hora. El PDC de la raíz del bosque es el origen del reloj predeterminado para todos los dominios. A continuación se muestran diferentes roles y una descripción de alto nivel sobre cómo encuentran un origen:


- **Controlador de dominio con rol de PDC** : este equipo es el origen de la hora autorizado para un dominio. Tendrá el tiempo más preciso disponible en el dominio y debe sincronizarse con un controlador de dominio en el dominio primario, excepto en los casos en los que el rol GTIMESERV está habilitado.
- **Cualquier otro controlador de dominio** : este equipo actuará como origen de hora para los clientes y los servidores miembro del dominio. Un controlador de dominio puede sincronizarse con el PDC de su propio dominio o con cualquier controlador de dominio en su dominio primario.
- **Clientes/servidores miembro** : este equipo puede sincronizarse con cualquier DC o PDC de su propio dominio, o con un DC o PDC en el dominio primario.

En función de los candidatos disponibles, se usa un sistema de puntuación para encontrar el origen de la mejor hora. Este sistema tiene en cuenta la confiabilidad del origen de hora y su ubicación relativa. Esto sucede cuando se inicia el servicio. Si necesita tener un control más preciso de cómo se sincroniza el tiempo, puede agregar servidores de hora correctos en ubicaciones específicas o agregar redundancia. Para obtener más información, consulte la sección [Especificación de un servicio de hora local confiable mediante GTIMESERV.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Entornos de sistema operativo mixtos (Win2012R2 y Win2008R2)
Aunque se requiere un entorno de dominio de Windows Server 2016 puro para obtener la mejor precisión, todavía hay ventajas en un entorno mixto. La implementación de Windows Server 2016 Hyper-V en un dominio de Windows 2012 beneficiará a los invitados debido a las mejoras que hemos mencionado anteriormente, pero solo si los invitados también son Windows Server 2016. Un PDC de Windows Server 2016 podrá ofrecer un tiempo más preciso debido a los algoritmos mejorados, por lo que será un origen más estable. Dado que es posible que el reemplazo de su PDC no sea una opción, puede Agregar un controlador de dominio de Windows Server 2016 con el conjunto de rollo de GTIMESERV, que sería una actualización de precisión para su dominio. Un controlador de dominio de Windows Server 2016 puede ofrecer mejor tiempo para los clientes de tiempo de bajada, pero solo es tan bueno como su hora NTP de origen.

También como se indicó anteriormente, las frecuencias de sondeo y actualización del reloj se han modificado con Windows Server 2016. Estos se pueden cambiar manualmente en los controladores de DC de nivel inferior o aplicarse a través de la Directiva de grupo. Aunque no hemos probado estas configuraciones, deben comportarse bien en Win2008R2 y Win2012R2 y ofrecer algunas ventajas.

Las versiones anteriores a Windows Server 2016 tenían varios problemas para mantener el tiempo de conservación preciso, lo que dio lugar a que la hora del sistema se desplazara inmediatamente después de que se realizara un ajuste. Por este motivo, la obtención de muestras de tiempo de un origen NTP preciso con frecuencia y el acondicionamiento del reloj local con los datos conduce a un desplazamiento más reducido en los relojes del sistema en el período de muestreo, lo que da lugar a un mejor tiempo de mantenimiento en las versiones del sistema operativo de nivel inferior. La mejor precisión observada era de aproximadamente 5 ms cuando un cliente NTP de Windows Server 2012R2, configurado con la configuración de alta precisión, sincronizó su tiempo desde un servidor NTP de Windows 2016 preciso.

En algunos escenarios que implican controladores de dominio invitados, los ejemplos de TimeSync de Hyper-V pueden interrumpir la sincronización de la hora del dominio. Esto ya no debe ser un problema para los invitados del servidor 2016 que se ejecutan en hosts de Hyper-V del servidor 2016.

Para deshabilitar el servicio TimeSync de Hyper-V para proporcionar ejemplos a W32Time, establezca la siguiente clave del registro invitado:

 HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider "Enabled" = dword: 00000000

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>Permitir que Linux use la hora del host de Hyper-V
En el caso de los invitados de Linux que se ejecutan en Hyper-V, los clientes se configuran normalmente para usar el demonio NTP para la sincronización de hora con los servidores NTP. Si la distribución de Linux es compatible con el protocolo de la versión 4 de TimeSync y el invitado de Linux tiene habilitado el servicio de integración de TimeSync, se sincronizará con la hora del host. Esto podría dar lugar a un tiempo de conservación incoherente si ambos métodos están habilitados.

Para sincronizar exclusivamente con la hora del host, se recomienda deshabilitar la sincronización de hora de NTP:

- Deshabilitación de los servidores NTP en el archivo NTP. conf
- o deshabilitar el demonio NTP

En esta configuración, el parámetro de servidor horario es este host. Su frecuencia de sondeo es de 5 segundos y la frecuencia de actualización del reloj también es de 5 segundos.

Para sincronizar exclusivamente a través de NTP, se recomienda deshabilitar el servicio de integración de TimeSync en el invitado.

> [!NOTE]
> Nota: la compatibilidad con un tiempo preciso con invitados de Linux requiere una característica que solo se admite en los kernels de Linux de nivel superior más recientes y no es algo que esté ampliamente disponible en todos los distribuciones de Linux. Para obtener más información sobre las distribuciones de soporte técnico, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) .

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>Especificar un servicio de hora confiable local mediante GTIMESERV
Puede especificar uno o varios controladores de dominio como relojes de origen precisos mediante el uso de las marcas GTIMESERV, Good Time Server. Por ejemplo, los controladores de dominio específicos equipados con hardware GPS se pueden marcar como GTIMESERV. Esto garantizará que el dominio hace referencia a un reloj basado en el hardware GPS.

> [!NOTE]
> Puede encontrar más información sobre las marcas de dominio en la [documentación del protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV es otro marcador de servicios de dominio relacionado que indica si un equipo está actualmente autorizado, lo que puede cambiar si un DC pierde la conexión. Un controlador de dominio en este estado devolverá "capa desconocida" cuando se realicen consultas a través de NTP. Después de probar varias veces, el controlador de dominio registrará el evento de servicio de hora de eventos del sistema 36.

Si desea configurar un controlador de dominio como GTIMESERV, puede configurarlo manualmente con el siguiente comando. En este caso, el controlador de dominio usa otras máquinas como el reloj maestro. Podría ser un dispositivo o un equipo dedicado.

 W32tm/config/manualpeerlist: "master_clock1, 0x8 master_clock2, 0x8"/syncfromflags:/Reliable manual: Yes/Update

> [!NOTE]
> Para obtener más información, consulte [configuración del servicio de hora de Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si el controlador de dominio tiene instalado el hardware GPS, debe seguir estos pasos para deshabilitar el cliente NTP y habilitar el servidor NTP.

Para empezar, deshabilite el cliente NTP y habilite el servidor NTP con estos cambios en la clave del registro.

 REG Add HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient/v habilitado/t REG_DWORD/d 0/f

 REG Add HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer/v habilitado/t REG_DWORD/d 1/f

A continuación, reinicie el servicio de hora de Windows

 net stop W32Time & & net start W32Time

Por último, indica que esta máquina tiene un origen de hora confiable mediante.
  
 W32tm/config/Reliable: Yes/Update

Para comprobar que los cambios se han realizado correctamente, puede ejecutar los siguientes comandos que afectan a los resultados que se muestran a continuación. 

 W32tm/Query/configuración

Valor|Configuración esperada|
----- | ----- |
AnnounceFlags| 5 (local)|
NtpServer |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (local)|
Habilitado |1 (local)|
Resolvi| (Local)|

 W32tm/Query/status/verbose

Valor| Configuración esperada|
----- | ----- |
Sincronizados| 1 (referencia principal: sincronizada por el reloj de radio)|
ReferenceId| 0x4C4F434C (nombre de origen: "LOCAL")|
Origen| Reloj CMOS local|
Desplazamiento de fase| 0.0000000 s|
Rol de servidor| 576 (servicio de hora confiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 en plataformas virtuales de terceros
Cuando se virtualiza Windows, el hipervisor es responsable de proporcionar tiempo de forma predeterminada. Pero los miembros Unidos a un dominio deben Sychronized con el controlador de dominio para que Active Directory funcionen correctamente. Es mejor deshabilitar la virtualización de cualquier momento entre el invitado y el host de cualquier plataforma virtual de terceros.

#### <a name="discovering-the-hierarchy"></a>Detección de la jerarquía
Dado que la cadena de jerarquía de tiempo para el origen del reloj maestro es dinámica en un dominio y se negocia, deberá consultar el estado de una máquina determinada para comprender el origen de hora y la cadena al reloj de origen principal. Esto puede ayudar a diagnosticar problemas de sincronización de la hora.

Dado que desea solucionar problemas de un cliente específico; el primer paso es comprender su origen de hora mediante este comando W32tm.

 W32tm/Query/status

Los resultados muestran el origen entre otras cosas. El origen indica con quién sincroniza la hora en el dominio. Este es el primer paso de la jerarquía de tiempo de esta máquina.
A continuación, use la entrada de origen anterior y use el parámetro/StripChart para buscar el siguiente origen de tiempo de la cadena.

 W32tm/Stripchart/Computer: MySourceEntry/packetinfo/Samples: 1

También resulta útil el siguiente comando muestra una lista de cada controlador de dominio que puede encontrar en el dominio especificado e imprime un resultado que le permite determinar cada asociado. Este comando incluirá los equipos configurados manualmente.
 
 W32tm/monitor/Domain: my_domain

Mediante la lista, puede realizar un seguimiento de los resultados a través del dominio y comprender la jerarquía, así como el desplazamiento de tiempo en cada paso. Al localizar el punto en el que el desplazamiento de tiempo es significativamente peor, puede identificar la raíz de la hora incorrecta. Desde allí, puede intentar comprender por qué la hora es incorrecta activando el registro W32tm.

#### <a name="using-group-policy"></a>Uso de la directiva de grupo
Puede usar directiva de grupo para lograr una precisión más estricta, por ejemplo, asignando a los clientes que usen servidores NTP específicos o controlen cómo se configuran los sistemas operativos de nivel inferior cuando están virtualizados. A continuación se muestra una lista de posibles escenarios y configuraciones de directiva de grupo relevantes:

**Dominios virtualizados** : con el fin de controlar los controladores de dominio virtualizados en Windows 2012R2 para que sincronicen la hora con su dominio, en lugar de con el host de Hyper-V, puede deshabilitar esta entrada del registro.  En el caso de PDC, no desea deshabilitar la entrada, ya que el host de Hyper-V proporcionará el origen de hora más estable. La entrada del registro requiere que se reinicie el servicio W32Time una vez que se haya cambiado.

 [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled" = dword: 00000000

**Cargas de precisión** : para cargas de trabajo sensibles a la precisión temporal, puede configurar grupos de máquinas para establecer los servidores ntp y cualquier configuración de hora relacionada, como el sondeo y la frecuencia de actualización del reloj. Normalmente, esto lo controla el dominio, pero para más control puede dirigirse a equipos específicos para que apunten directamente al reloj maestro.

Configuración de directiva de grupo| Valor nuevo|
----- | ----- |
NtpServer| ClockMasterName, 0x8|
MinPollInterval| de 6 a 64 segundos|
MaxPollInterval| 6|
UpdateInterval| 100: una vez por segundo|
EventLogFlags| 3-registro de hora especial|

> [!NOTE]
> La configuración de NtpServer y EventLogFlags se encuentra en System\Windows Time Service\Time Providers Using the Windows NTP Client Settings. Los otros 3 se encuentran en servicio de hora de System\Windows con las opciones de configuración global.

**Cargas remotas con distinción de precisión** remota: para los sistemas de dominios de rama para la instancia comercial y el sector de crédito de pago (PCI), Windows usa la información del sitio actual y el ubicador de DC para encontrar un controlador de dominio local, a menos que haya configurado un origen de hora NTP manual. Este entorno requiere 1 segundo de precisión, que usa una convergencia más rápida a la hora correcta. Esta opción permite al servicio W32Time pasar el reloj hacia atrás. Si esto es aceptable y cumple sus requisitos, puede crear la Directiva siguiente.  Al igual que con cualquier entorno, asegúrese de probar y realizar una línea de base de la red. 

Configuración de directiva de grupo| Valor nuevo|
----- | ----- |
MaxAllowedPhaseOffset| 1, si es superior a en segundo, establezca el reloj en la hora correcta.|

El valor MaxAllowedPhaseOffset se encuentra en System\Windows Time Service con las opciones de configuración global.

> [!NOTE]
> Para obtener más información sobre la Directiva de grupo y las entradas relacionadas, consulte el artículo herramientas y configuración del [servicio de hora de Windows](windows-time-service-tools-and-settings.md) en TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Consideraciones sobre IaaS de Azure y Windows

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina virtual de Azure: Active Directory Domain Services
Si la máquina virtual de Azure que ejecuta Active Directory Domain Services forma parte de un bosque de Active Directory local existente, se debe deshabilitar TimeSync (VMIC). Esto sirve para permitir que todos los controladores de dispositivo del bosque, tanto físicos como virtuales, usen una única jerarquía de sincronización. Consulte las notas del producto de procedimientos recomendados ["ejecutar controladores de dominio en Hyper-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina virtual de Azure: equipo unido a un dominio
Si hospeda una máquina que está unida a un dominio en un bosque de Active Directory existente, virtual o física, el procedimiento recomendado es deshabilitar TimeSync para el invitado y asegurarse de que W32Time está configurado para sincronizarse con su controlador de dominio mediante la configuración del tiempo para Tipo = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina virtual de Azure: equipo de grupo de trabajo independiente
Si la máquina virtual de Azure no está unida a un dominio, ni tampoco es un controlador de dominio, se recomienda mantener la configuración de tiempo predeterminada y hacer que la máquina virtual se sincronice con el host.

## <a name="windows-application-requiring-accurate-time"></a>Aplicación para Windows que requiere un tiempo preciso
### <a name="time-stamp-api"></a>API de marca de tiempo
Los programas que requieren mayor precisión con respecto a la hora UTC, y no el paso del tiempo, deben usar la [API de GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx). Esto garantiza que la aplicación obtenga la hora del sistema, que es condicionada por el servicio de hora de Windows.

### <a name="udp-performance"></a>Rendimiento de UDP
Si tiene una aplicación que usa la comunicación UDP para las transacciones y es importante minimizar la latencia, hay algunas entradas del registro relacionadas que puede usar para configurar un intervalo de puertos que se van a excluir del puerto del motor de filtrado de base. Esto mejorará la latencia y aumentará el rendimiento. Sin embargo, los cambios en el registro deben limitarse a los administradores experimentados. Además, esto evita que los puertos no estén protegidos por el firewall. Para obtener más información, vea la referencia del artículo siguiente.

En Windows Server 2012 y Windows Server 2008, primero deberá instalar una revisión. Puede hacer referencia a este artículo de KB: [pérdida de datagramas al ejecutar una aplicación de receptor de multidifusión en Windows 8 y en Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Actualizar controladores de red
Algunos proveedores de red tienen actualizaciones de controladores que mejoran el rendimiento con respecto a la latencia del controlador y el almacenamiento en búfer de paquetes UDP. Póngase en contacto con su proveedor de red para ver si hay actualizaciones que le ayuden con el rendimiento de UDP.

## <a name="logging-for-auditing-purposes"></a>Registro para fines de auditoría
Para cumplir las normativas de seguimiento de tiempo, puede archivar manualmente los registros W32tm, los registros de eventos y la información del monitor de rendimiento. Más adelante, se puede usar la información archivada para atestiguar el cumplimiento en un momento específico del pasado. Los siguientes factores se usan para indicar la precisión.

1. Precisión del reloj mediante el contador del monitor de rendimiento de desplazamiento de tiempo calculado. Esto muestra el reloj con la precisión deseada.
2. Origen del reloj que busca "respuesta del mismo nivel desde" en los registros W32tm.  Después del texto del mensaje se encuentra la dirección IP o VMIC, que describe el origen de la hora y el siguiente en la cadena de relojes de referencia que se va a validar.
3. Estado de la condición del reloj mediante los registros W32tm para validar que se está produciendo la "disciplina ClockDispl: \*sesgo\*tiempo\*". Esto indica que W32tm está activo en ese momento.

### <a name="event-logging"></a>Registro de eventos
Para obtener la historia completa, también necesitará información de registro de eventos. Al recopilar el registro de eventos del sistema y filtrar por Time-Server, Microsoft-Windows-kernel-boot, Microsoft-Windows-kernel-general, es posible que pueda detectar si hay otras influencias que han cambiado el tiempo, por ejemplo, terceros. Estos registros pueden ser necesarios para descartar interferencias externas. La Directiva de grupo puede afectar a los registros de eventos que se escriben en el registro. Vea la sección anterior sobre el uso de directiva de grupo para obtener más detalles.

### <a name="W32Logging"></a>Registro de depuración de W32time
Para habilitar W32tm con fines de auditoría, el siguiente comando habilita el registro que muestra las actualizaciones periódicas del reloj e indica el reloj de origen. Reinicie el servicio para habilitar el nuevo registro. 

Para obtener más información, vea [Cómo activar el registro de depuración en el servicio de hora de Windows](https://support.microsoft.com/kb/816043).

 W32tm/Debug/enable/File: C:\Windows\Temp\w32time-test.log/size: 10000000/entries: 0-73103107110

### <a name="performance-monitor"></a>Performance Monitor
El servicio de hora de Windows de Windows Server 2016 expone contadores de rendimiento que se pueden usar para recopilar el registro de auditoría. Estos se pueden registrar localmente o de forma remota. Puede registrar los contadores de desplazamiento de tiempo de equipo y retraso de recorrido de ida y vuelta. Y, al igual que cualquier contador de rendimiento, puede supervisarlos de forma remota y crear alertas mediante System Center Operations Manager. Por ejemplo, puede usar una alerta para avisarle cuando el desplazamiento de tiempo se desplace de la precisión deseada. El [módulo de administración de System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tiene más información.

### <a name="windows-traceability-example"></a>Ejemplo de rastreabilidad de Windows
En los archivos de registro W32tm, querrá validar dos fragmentos de información. La primera es una indicación de que el archivo de registro es actualmente el reloj de condición. Esto demuestra que el reloj estaba condicionado por el servicio de hora de Windows en el momento conflictivo.

 151802 20:18:32.9821765 s-ClockDispln disciplina: *sesgo*Time *-PhCRR: 223 CR: 156250 UI: 100 phcT: 65 KPhO: 14307 151802 20:18:33.9898460 s-ClockDispln disciplina: *sesgo*Time *-PhCRR: 1 CR: 156250 UI: 100 phcT: 64 KPhO: 41 151802 20:18:44.1090410 s-ClockDispln disciplina: *sesgo*Time *-PHCRR: 1 CR: 156250 UI: 100 phcT: 65 KPhO: 38

El punto principal es que los mensajes se ven precedidos por la disciplina de ClockDispln, que es la prueba que W32Time está interactuando con el reloj del sistema.
 
A continuación, debe buscar el último informe en el registro antes del tiempo conflictivo que informa del equipo de origen que se está usando actualmente como el reloj de referencia. Puede ser una dirección IP, un nombre de equipo o el proveedor VMIC, lo que indica que se está sincronizando con el host para Hyper-V. En el ejemplo siguiente se proporciona una dirección IPv4 de 10.197.216.105.

 151802 20:18:54.6531515 s-respuesta de Peer 10.197.216.105, 0x8 (NTP. m | 0x8 | 0.0.0.0:123-> 10.197.216.105:123), OFS: + 00.0012218 s

Ahora que ha validado el primer sistema en la cadena de tiempo de referencia, debe investigar el archivo de registro en el origen de la hora de referencia y repetir los mismos pasos. Esto continúa hasta que llegue a un reloj físico, como GPS o un origen de tiempo conocido como NIST. Si el reloj de referencia es hardware GPS, es posible que también se requieran los registros de la fabricación.

## <a name="network-considerations"></a>Consideraciones de red
Los algoritmos de protocolo NTP dependen de la simetría de la red. A medida que aumenta el número de saltos de red, aumenta la probabilidad de asimetría. Allí, es difícil predecir qué tipos de precisión se verán en los entornos específicos. 

El monitor de rendimiento y los nuevos contadores de hora de Windows en Windows Server 2016 se pueden usar para evaluar la precisión de los entornos y crear líneas de base. Además, puede solucionar problemas para determinar el desplazamiento actual de cualquier equipo de la red.

Hay dos estándares generales para el tiempo preciso a través de la red. PTP ([Protocolo de tiempo de precisión-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tiene requisitos más estrictos en la infraestructura de red, pero a menudo puede proporcionar una precisión de subsegundos. NTP ([Protocolo de tiempo de red: RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona en una amplia variedad de redes y entornos, lo que facilita su administración. 

Windows admite NTP simple (RFC2030) de forma predeterminada para las máquinas que no están unidas a un dominio. En el caso de las máquinas Unidas a un dominio, usamos un NTP seguro denominado [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), que aprovecha los secretos negociados del dominio, que proporcionan una ventaja de administración sobre el NTP autenticado descrito en RFC1305 y RFC5905.  

Los protocolos de dominio y no Unidos a un dominio requieren el puerto UDP 123. Para obtener más información acerca de los procedimientos recomendados de NTP, consulte las prácticas recomendadas del [Protocolo de tiempo de red procedimientos actuales de IETF draft](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Reloj de hardware confiable (RTC)
Windows no realiza el paso de la hora, a menos que se superen determinados límites, pero en su lugar se disciplina el reloj. Esto significa que W32tm ajusta la frecuencia del reloj a intervalos regulares, con la configuración de frecuencia de actualización del reloj, que tiene como valor predeterminado una vez por segundo con Windows Server 2016. Si el reloj está detrás, acelera la frecuencia y, si está por atrás, ralentiza la frecuencia. Sin embargo, durante ese tiempo entre los ajustes de frecuencia del reloj, el reloj del hardware está en el control. Si hay un problema con el firmware o el reloj del hardware, la hora del equipo puede ser menos precisa.

Este es otro motivo que debe probar y línea de base en su entorno. Si el contador de rendimiento "desplazamiento de tiempo calculado" no se estabiliza a la precisión de destino, puede que desee comprobar que el firmware esté actualizado. Como otra prueba, puede ver si el hardware duplicado reproduce el mismo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solución de problemas de precisión temporal y NTP
Puede usar la sección detectar la jerarquía anterior para comprender el origen de la hora inexacta. Observando el desplazamiento de tiempo, busque el punto de la jerarquía en el que el tiempo diverge el máximo de su origen NTP. Una vez que comprenda la jerarquía, querrá probar y comprender por qué esa fuente de hora determinada no recibe un tiempo preciso. 

Centrándose en el sistema con un tiempo divergente, puede usar estas herramientas para recopilar más información que le ayude a determinar el problema y encontrar una solución. La siguiente referencia UpstreamClockSource es el reloj descubierto mediante "W32tm/config/status".


- Registros de eventos del sistema
- Habilitar el registro mediante: W32tm logs-W32tm/Debug/enable/File: C:\Windows\Temp\w32time-test.log/size: 10000000/entries: 0-300
- clave del registro de w32Time HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\W32Time
- Seguimientos de red local
- Contadores de rendimiento (de la máquina local o UpstreamClockSource)
- W32tm/Stripchart/Computer: UpstreamClockSource
- PING UpstreamClockSource para comprender la latencia y el número de saltos al origen
- Tracert UpstreamClockSource

Problema| Síntomas| Resolución|
----- | ----- | ----- |
| El reloj de TSC local no es estable.| Mediante Perfmon-equipo físico: sincronizar el reloj del reloj, pero sigue viendo que cada 1-2 minutos de varios 100us. | Actualizar firmware o validar hardware diferente no muestra el mismo problema.|
| Latencia de red| W32tm Stripchart muestra un RoundTripDelay de más de 10 ms. La variación en el retraso produce ruido de hasta 1/2 del tiempo de ida y vuelta, por ejemplo, un retraso que solo está en una dirección.<br><br>UpstreamClockSource es varios saltos, tal y como se indica mediante PING. El TTL debe estar cerca de 128.<br><br>Usar tracert para encontrar la latencia en cada salto.  | Busque un origen de reloj más cercano para la hora. Una solución consiste en instalar un reloj de origen en el mismo segmento o seleccionar manualmente el reloj de origen que está más cerca geográficamente. Para un escenario de dominio, agregue una máquina con el rol GTimeServ. | 
No se puede alcanzar de forma confiable el origen NTP| W32tm/Stripchart devuelve intermitentemente "se agotó el tiempo de espera de la solicitud" |El origen NTP no responde|
El origen NTP no responde| Compruebe los contadores de Perfmon para el recuento de orígenes de cliente NTP, las solicitudes entrantes del servidor NTP, las respuestas salientes del servidor NTP y determine el uso en comparación con las líneas base.| Mediante los contadores de rendimiento del servidor, determine si la carga ha cambiado en referencia a las líneas base.<br><br>¿Hay problemas de congestión de la red?|
El controlador de dominio no usa el reloj más preciso| Cambios en la topología o en el reloj de hora principal agregado recientemente.| W32tm/Resync/rediscover|
Los relojes de los clientes están desplazados| Evento 36 de servicio de hora en el registro de eventos del sistema y/o texto del archivo de registro que describe: "recuento de origen de tiempo de cliente NTP" que va de 1 a 0|Solucione el problema del origen ascendente y comprenda si se está ejecutando en problemas de rendimiento.|

### <a name="baselining-time"></a>Hora de la línea de tiempo
La línea de base es importante, por lo que primero puede comprender el rendimiento y la precisión de la red y compararlo con la línea de base en el futuro cuando se produzcan problemas. Querrá basar el PDC raíz o cualquier equipo marcado con el GTIMESRV. También se recomienda realizar una línea base del PDC en cada bosque. Por último, elija los controladores de dispositivos críticos o las máquinas que tengan características interesantes, como las cargas de distancia o alta y las líneas base.

También es útil para Windows Server 2016 vs 2012 R2 de línea de base, pero solo tiene W32tm/Stripchart como una herramienta que puede usar para comparar, ya que Windows Server 2012R2 no tiene contadores de rendimiento. Debe elegir dos máquinas con las mismas características, o actualizar una máquina y comparar los resultados después de la actualización. En el anexo de medidas de tiempo de Windows se ofrece más información sobre cómo realizar mediciones detalladas entre 2016 y 2012.

Con todos los contadores de rendimiento de W32Time, recopile datos durante al menos una semana. Esto garantizará que tiene suficiente una referencia para la cuenta de varias en la red a lo largo del tiempo y lo suficiente para que la duración sea estable.

### <a name="ntp-server-redundancy"></a>Redundancia del servidor NTP
Para la configuración manual del servidor NTP utilizada con máquinas no Unidas a un dominio o con el PDC, tener más de un servidor es una buena medida de redundancia en caso de disponibilidad. También podría proporcionar una mejor precisión, suponiendo que todos los orígenes son precisos y estables. Sin embargo, si la topología no está bien diseñada o los orígenes de hora no son estables, la precisión resultante podría ser peor, por lo que se recomienda tener cuidado. El límite de servidores horarios admitidos W32Time puede hacer referencia manualmente a 10. 

## <a name="leap-seconds"></a>Segundos bisiestos
El período de rotación de la tierra varía con el tiempo, debido a eventos climáticos y geológicos. Normalmente, la variación es aproximadamente un segundo cada dos años. Siempre que la variación de la hora atómica crezca demasiado, se inserta una corrección de un segundo (arriba o abajo), denominada segundo salto. Esto se hace de manera que la diferencia nunca supere los 0,9 segundos. Esta corrección se anuncia seis meses antes de la corrección real. Antes de Windows Server 2016, el servicio de hora de Microsoft no era consciente de los segundos bisiestos, pero confiaba en el servicio de hora externo para encargarse de ello. Con la mayor precisión de tiempo de Windows Server 2016, Microsoft está trabajando en una solución más adecuada para el segundo problema de LEAP.

## <a name="secure-time-seeding"></a>Propagación de tiempo seguro
W32time en el servidor 2016 incluye la característica de propagación de tiempo seguro. Esta característica determina la hora actual aproximada de las conexiones SSL salientes. Este valor de tiempo se usa para supervisar el reloj del sistema local y corregir cualquier error bruto. Puede leer más sobre la característica en [esta entrada de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/). En las implementaciones con un origen de hora confiable y equipos bien supervisados que incluyen la supervisión de desplazamientos de tiempo, puede optar por no usar la característica de propagación de tiempo seguro y confiar en la infraestructura existente. 

Puede deshabilitar la característica con estos pasos:

1. Establezca el valor de configuración del registro UtilizeSSLTimeData en 0 en un equipo específico:

    REG Add HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\w32time\Config/v UtilizeSslTimeData/t REG_DWORD/d 0/f

2. Si no puede reiniciar el equipo inmediatamente debido a alguna razón, puede notificar al servicio W32time sobre la actualización de la configuración. Esto detiene la supervisión y la aplicación de la hora en función de los datos recopilados de las conexiones SSL.

    W32tm. exe/config/Update

3. Al reiniciar el equipo, la configuración se hace efectiva de inmediato y también se detiene la recopilación de datos de tiempo de las conexiones SSL. Esta última parte tiene una sobrecarga muy pequeña y no debe ser un problema de rendimiento.

4. Para aplicar esta configuración en un dominio completo, establezca el valor de UtilizeSSLTimeData en la configuración de la Directiva de grupo W32time en 0 y publique el valor. Cuando un cliente directiva de grupo selecciona la configuración, se notifica al servicio W32time y se detiene la supervisión y el cumplimiento de la hora mediante datos de tiempo SSL. La recopilación de datos de tiempo SSL se detendrá cuando se reinicie cada máquina. Si el dominio tiene equipos portátiles o tabletas delgados portátiles y otros dispositivos, puede que desee excluir tales máquinas de este cambio de directiva. Estos dispositivos se dirigirán finalmente a la carga de la batería y necesitarán la característica de propagación de tiempo seguro para arrancar su tiempo.
