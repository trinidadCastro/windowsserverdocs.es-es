---
title: Mejoras en la precisión temporal para Windows Server 2016
description: Windows Server 2016 ha mejorado los algoritmos que usa para corregir la hora y programar el reloj local para que se sincronice con la hora UTC.
author: dahavey
ms.author: dahavey
ms.date: 10/17/2018
ms.topic: article
ms.openlocfilehash: 0dd8c4c9714f50ba3eee45efdb714e66a8916dd6
ms.sourcegitcommit: 2ede79efbadd109099bb6fdb744796adde123922
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98923667"
---
# <a name="time-accuracy-improvements-for-windows-server-2016"></a>Mejoras en la precisión temporal para Windows Server 2016

Windows Server 2016 ha mejorado los algoritmos que usa para corregir la hora y programar el reloj local para que se sincronice con la hora UTC. NTP usa 4 valores para calcular el desfase de hora en función de las marcas de tiempo de la solicitud o respuesta del cliente y del servidor. Sin embargo, las redes tienen ruido y puede haber picos en los datos de NTP debido a la congestión de la red y otros factores que afectan a la latencia de la red. Los algoritmos de Windows 2016 calculan la media de este ruido mediante una serie de técnicas diferentes, lo que da como resultado un reloj estable y exacto. Además, el origen que usamos para una hora precisa hace referencia a una API mejorada que nos proporciona una mejor resolución. Con estas mejoras, podemos lograr una precisión de 1 ms con respecto a la hora UTC de un dominio.

## <a name="hyper-v"></a>Hyper-V

Windows 2016 ha mejorado el servicio TimeSync de Hyper-V. Entre las mejoras se incluye una hora inicial más precisa en el inicio o la restauración de la máquina virtual y la corrección de la latencia de interrupción para las muestras proporcionadas a W32Time. Esta mejora nos permite permanecer dentro de los 10 μs del host con una media cuadrática (RMS, que indica la varianza) de 50 μs, incluso en una máquina con una carga del 75 %. Para obtener más información, consulta [Arquitectura de Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> La carga se creó con el banco de pruebas Prime95 mediante un perfil equilibrado.

Además, el nivel de capa que el host notifica al invitado es más transparente. Anteriormente, el host presentaba un valor de capa fijo de 2, independientemente de su precisión. Con los cambios en Windows Server 2016, el host informa de una capa mayor que la del host, lo que da como resultado una hora más precisa para los invitados virtuales. W32Time determina la capa del host con medios normales según su hora de origen. Los invitados de Windows 2016 unidos a un dominio encontrarán el reloj más preciso, en lugar de definir como valor predeterminado el host. Por este motivo, recomendamos deshabilitar manualmente la configuración del proveedor de hora de Hyper-V para las máquinas que participan en un dominio de Windows 2012 R2 y versiones anteriores.

## <a name="monitoring"></a>Supervisión

Se han agregado contadores del monitor de rendimiento. Permiten establecer la precisión horaria como base de referencia, así como supervisar dicha precisión y solucionar sus problemas. Estos contadores incluyen:

|Contador|Descripción|
|----- | ----- |
|Desfase de hora calculado| Desfase de hora absoluto entre el reloj del sistema y el origen de la hora elegido, calculado por el servicio W32Time en microsegundos. Cuando hay una nueva muestra válida disponible, la hora calculada se actualiza con el desfase de hora que se indica en el ejemplo. Este es el desfase de hora del reloj local. W32Time inicia la corrección del reloj con este desfase y actualiza la hora calculada entre las muestras con el desfase de hora restante que se debe aplicar al reloj local. Se puede realizar un seguimiento de la precisión del reloj mediante este contador de rendimiento con un intervalo de sondeo bajo (p. ej., 256 segundos o menos) y buscando un valor del contador menor que el límite de precisión del reloj deseado.|
|Ajuste de frecuencia del reloj| Ajuste de frecuencia del reloj absoluto que realizó W32Time en el reloj del sistema local (en partes por mil millones). Este contador ayuda a visualizar las acciones que realiza W32Time.|
|Retraso del recorrido de ida y vuelta de NTP| Retraso del recorrido de ida y vuelta más reciente que experimentó el cliente NTP al recibir una respuesta del servidor en microsegundos. Es el tiempo transcurrido en el cliente NTP entre la transmisión de una solicitud al servidor NTP y la recepción de una respuesta válida desde el servidor. Este contador permite caracterizar los retrasos que experimenta el cliente NTP. Los recorridos de ida y vuelta mayores o variables pueden agregar ruido a los cálculos de tiempo de NTP, lo que a su vez puede afectar a la precisión de la sincronización de la hora a través de NTP.|
|Recuento de orígenes de cliente NTP| Número activo de orígenes de la hora NTP que usa el cliente NTP. Se trata de un recuento de direcciones IP distintas y activas de los servidores horarios que responden a las solicitudes de este cliente. Este número puede ser mayor o menor que el de los elementos del mismo nivel configurados, en función de la resolución de DNS de los nombres del mismo nivel y de la capacidad de alcance actual.|
|Solicitudes entrantes del servidor NTP| Número de solicitudes recibidas por el servidor NTP (solicitudes por segundo).|
|Respuestas salientes del servidor NTP| Número de solicitudes respondidas por el servidor NTP (solicitudes por segundo).|

Los tres primeros contadores corresponden a escenarios de solución de problemas con la precisión. En la sección Solución de problemas con la precisión horaria y NTP que hay a continuación del apartado Procedimientos recomendados, se incluyen más detalles.
Los tres últimos contadores cubren los escenarios de servidor NTP y resultan útiles cuando se determina la carga y se establece una base de referencia para tu rendimiento actual.

## <a name="configuration-updates-per-environment"></a>Actualizaciones de configuración por entorno

A continuación, se describen los cambios en la configuración predeterminada entre Windows 2016 y las versiones anteriores para cada rol. La configuración de Windows Server 2016 y la Actualización de aniversario de Windows 10 (compilación 14393) ahora son únicas y, por este motivo, se muestran como columnas independientes.

|Rol|Valor|Windows Server 2016|Windows 10|Windows Server 2012 R2<br>Windows Server 2008 R2<br>Windows 10|
|---|---|---|---|---|
|**Independiente/Nano Server**||||
| |Servidor horario|time.windows.com|N/D|time.windows.com|
| |Frecuencia de sondeo|64 - 1024 segundos|N/D|Una vez a la semana|
| |Frecuencia de actualización del reloj|Una vez por segundo|N/D|Una vez a la hora|
|**Cliente independiente**||||
| |*Servidor horario*|N/D|time.windows.com|time.windows.com|
| |*Frecuencia de sondeo*|N/D|Una vez al día|Una vez a la semana|
| |*Frecuencia de actualización del reloj*|N/D|Una vez al día|Una vez a la hora|
|**Controlador de dominio**||||
| |*Servidor horario*|PDC/GTIMESERV|N/D|PDC/GTIMESERV|
| |*Frecuencia de sondeo*|64 -1024 segundos|N/D|1024 - 32 768 segundos|
| |*Frecuencia de actualización del reloj*|Una vez por segundo|N/D|Una vez a la hora|
|**Servidor miembro de dominio**||||
| |Servidor horario|DC|N/D|DC|
| |Frecuencia de sondeo|64 -1024 segundos|N/D|1024 - 32 768 segundos|
| |Frecuencia de actualización del reloj|Una vez por segundo|N/D|Una vez cada 5 minutos|
|**Cliente miembro de dominio**||||
| |Servidor horario|N/D|DC|DC|
| |Frecuencia de sondeo|N/D|1204 - 32 768 segundos|1024 - 32 768 segundos|
| |Frecuencia de actualización del reloj|N/D|Una vez cada 5 minutos|Una vez cada 5 minutos|
|**Invitado de Hyper-V**||||
| |Servidor horario|Elige la mejor opción en función de la capa del host y el servidor horario|Elige la mejor opción en función de la capa del host y el servidor horario|El valor predeterminado es Host.|
| |Frecuencia de sondeo|Según el rol anterior|Según el rol anterior|Según el rol anterior|
| |Frecuencia de actualización del reloj|Según el rol anterior|Según el rol anterior|Según el rol anterior|

>[!NOTE]
>Para Linux en Hyper-V, consulta la sección Permiso de Linux para usar la hora del host de Hyper-V que hay a continuación.

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto del aumento del sondeo y la frecuencia de actualización del reloj

Con el fin de proporcionar horas más precisas, los valores predeterminados de las frecuencias de sondeo y las actualizaciones del reloj aumentan, lo que nos permite realizar pequeños ajustes con mayor frecuencia. Esto generará más tráfico UDP/NTP. Sin embargo, estos paquetes son pequeños, por lo que el impacto debería ser muy reducido o nulo respecto a los vínculos de la banda ancha. Aún así, la ventaja es que esa hora debe ser mejor en una variedad más amplia de hardware y entornos.

En el caso de los dispositivos que funcionan con batería, el aumento de la frecuencia de sondeo puede producir problemas. Los dispositivos con batería no almacenan la hora cuando están desactivados. Por este motivo, cuando se reanudan, es posible que requieran correcciones frecuentes en el reloj. El aumento de la frecuencia de sondeo hará que el reloj se vuelva inestable y también puede que use más energía. Microsoft no recomienda cambiar la configuración predeterminada del cliente.

Los controladores de dominio deberían verse afectados mínimamente, incluso con el efecto multiplicado del aumento de las actualizaciones de los clientes NTP en un dominio de AD. NTP tiene un consumo de recursos mucho más pequeño en comparación con otros protocolos y un impacto marginal. Es más probable que alcances los límites de otras funciones de dominio antes de verte afectado por el aumento de actualizaciones de Windows Server 2016. Active Directory usa NTP seguro, que tiende a sincronizar la hora con menos precisión que NTP simple, pero hemos verificado que se escalará verticalmente a los clientes que se encuentren a dos capas del controlador de dominio principal.

Como plan conservador, debes reservar 100 solicitudes NTP por segundo por núcleo. Por ejemplo, en un dominio formado por 4 controladores de dominio con 4 núcleos cada uno, deberías poder atender 1600 solicitudes NTP por segundo. Si tienes 10 000 clientes configurados para sincronizar la hora una vez cada 64 segundos y las solicitudes se reciben uniformemente a lo largo del tiempo, verás 10 000/64 o aproximadamente 160 solicitudes por segundo, distribuidas en todos los controladores de dominios. Esto corresponde fácilmente a las solicitudes de 1600 NTP por segundo basadas en este ejemplo. Estas recomendaciones de planificación son conservadoras y, por supuesto, tienen una gran dependencia en la red y las velocidades y cargas del procesador, por lo que, como siempre, establece tus entornos como base de referencia y pruébalos.

También es importante tener en cuenta que, si los controladores de dominio se ejecutan con una carga de CPU considerable (mayor del 40 %), se agregará ruido a las respuestas de NTP, lo que afectará a la precisión horaria en el dominio. De nuevo, debes probar tu entorno para comprender los resultados reales.

## <a name="time-accuracy-measurements"></a>Medidas de la precisión horaria

### <a name="methodology"></a>Metodología

Para medir la precisión horaria de Windows Server 2016, usamos distintas herramientas, métodos y entornos. Puedes usar estas técnicas para medir y ajustar el entorno y determinar si los resultados de precisión satisfacen tus necesidades.

Nuestro reloj de origen de dominio constaba de dos servidores NTP de alta precisión con hardware GPS. También usamos una máquina de prueba de referencia independiente para las medidas, que también tenían hardware GPS de alta precisión instalado de otro fabricante. En el caso de algunas de las pruebas, necesitarás un origen de reloj preciso y confiable para usarlo como referencia, además del origen del reloj del dominio.

Usamos cuatro métodos diferentes para medir la precisión con las máquinas físicas y virtuales. El uso de varios métodos proporcionaron medios independientes para validar los resultados.

1. Mide el reloj local, que está condicionado por W32tm, en comparación con la máquina de prueba de referencia que tiene hardware GPS independiente.

2. Mide los pings NTP del servidor NTP a los clientes mediante la herramienta W32tm "stripchart".

3. Mide los pings NTP del cliente al servidor NTP mediante la herramienta W32tm "stripchart".

4. Mide los resultados de Hyper-V del host al invitado mediante el contador de marca de tiempo (TSC). Este contador se comparte entre ambas particiones, y la hora del sistema en ambas particiones. Calculamos la diferencia entre la hora del host y la del cliente en la máquina virtual. A continuación, usamos el reloj de TSC para interpolar la hora del host desde el invitado, ya que las medidas no se realizan al mismo tiempo. Además, usamos los retrasos y la latencia del factor de reloj TSV en la API.

W32tm está integrado, pero las otras herramientas que usamos durante las pruebas están disponibles para el repositorio de Microsoft en GitHub como código abierto para las pruebas y el uso. Para obtener más información sobre cómo usar las herramientas para realizar medidas, consulte la [wiki de herramientas de calibración de la hora de Windows](https://github.com/Microsoft/Windows-Time-Calibration-Tools).

Los resultados de las pruebas que se muestran a continuación son un subconjunto de las medidas que realizamos en uno de los entornos de prueba. Ilustran la precisión que se mantiene al principio de la jerarquía de tiempo y el cliente de dominio secundario del final de la jerarquía de tiempo. Esto se compara con las mismas máquinas en una topología basada en 2012.

### <a name="topology"></a>Topología

Para la comparación, probamos una topología basada en Windows Server 2012 R2 y Windows Server 2016. Ambas topologías están compuestas por dos equipos host de Hyper-V físicos que hacen referencia a una máquina Windows Server 2016 con hardware de reloj de GPS instalado. Cada host ejecuta 3 invitados de Windows unidos a un dominio, que se organizan según la siguiente topología. Las líneas representan la jerarquía de tiempo y el protocolo o transporte que se utiliza.

![Diagrama de la topología de horas de Windows con un solo cliente de dominio secundario ejecutándose en el primer host de Hyper-V.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Diagrama de la topología de horas de Windows con dos clientes de dominio secundario; uno se ejecuta en el primer host de Hyper-V y el otro, en el segundo host de Hyper-V.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Información general sobre los resultados de los gráficos

Los dos gráficos siguientes representan la precisión horaria de dos miembros específicos de un dominio basado en la topología anterior. Cada gráfico muestra los resultados de Windows Server 2012 R2 y 2016, los cuales demuestran las mejoras visualmente. La precisión se mide desde la máquina invitada en comparación con el host. Los datos de los gráficos representan un subconjunto del conjunto completo de pruebas que hemos realizado y muestran los escenarios mejores y peores.

![Diagrama de la topología de horas de Windows con el servidor PDC del dominio raíz y los servidores de cliente de dominio secundario en el primer host de Hyper-V resaltados.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Rendimiento de PDC del dominio raíz

El controlador de dominio principal raíz se sincroniza con el host de Hyper-V (mediante VMIC), que es una instancia de Windows Server 2016 con hardware GPS que se ha demostrado que es precisa y estable. Se trata de un requisito fundamental para lograr una precisión de 1 ms, que se refleja con el área sombreada de color verde.

![Diagrama que representa el dominio raíz.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Rendimiento del cliente del dominio secundario

El cliente del dominio secundario se adjunta a un controlador de dominio principal del dominio secundario que se comunica con el controlador de dominio principal raíz. El tiempo también está dentro del requisito de 1 ms.

![Diagrama que representa el cliente de dominio secundario.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>Prueba de larga distancia

En el gráfico siguiente se compara un salto de red virtual con seis saltos de red físicos con Windows Server 2016. Dos gráficos se superponen entre sí con transparencia para mostrar los datos superpuestos. El aumento de los saltos de red significa que la latencia es mayor y que hay desviaciones de más tiempo. El gráfico se amplía y, por lo tanto, los límites de 1 ms, que se representan en el área verde, son más grandes. Como puedes ver, el tiempo sigue siendo de 1 ms con varios saltos. Se desplaza negativamente, lo que muestra una asimetría de la red. Por supuesto, cada red es diferente y las mediciones dependen de una gran variedad de factores ambientales.

![Diagrama que representa la prueba de larga distancia.](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>Procedimientos recomendados para la precisión de la hora

### <a name="solid-source-clock"></a>Reloj de origen sólido

Una hora de las máquinas solo es tan precisa como el reloj de origen con el que se sincroniza. Para lograr una precisión de 1 ms, necesitarás hardware GPS o un dispositivo de hora en la red al que hagas referencia como el reloj de origen maestro. Es posible que el uso del valor predeterminado time.windows.com no proporcione un origen de hora estable y local. Además, a medida que se aleja del reloj de origen, la red afecta a la precisión. Es necesario tener un reloj de origen maestro en cada centro de datos para obtener la mejor precisión.

### <a name="hardware-gps-options"></a>Opciones de GPS de hardware

Existen varias soluciones de hardware que pueden ofrecer una hora precisa. En general, las soluciones de hoy en día se basan en antenas GPS. También hay soluciones de radio y módem de acceso telefónico con líneas dedicadas. Se conectan a la red como un dispositivo, o a un equipo, por ejemplo, Windows a través de un dispositivo PCIe o USB. Las distintas opciones proporcionarán diferentes niveles de precisión y, como siempre, los resultados dependerán de tu entorno. Las variables que afectan a la precisión incluyen la disponibilidad del GPS, la estabilidad y la carga de red y el hardware del equipo. Todos estos factores resultan importantes a la hora de elegir un reloj de origen, que, como indicamos, es un requisito para lograr una hora estable y precisa.

### <a name="domain-and-synchronizing-time"></a>Hora de sincronización y dominio

Los miembros del dominio usan la jerarquía de dominios para determinar qué máquina usan como origen para sincronizar la hora. Cada miembro del dominio buscará otra máquina con la que realizar la sincronización y para guardarla como origen del reloj. Cada tipo de miembro de dominio sigue un conjunto diferente de reglas para buscar un origen de reloj para la sincronización de hora. El controlador de dominio principal de la raíz del bosque es el origen del reloj predeterminado para todos los dominios. A continuación, se muestran diferentes roles y una descripción de alto nivel sobre cómo buscan un origen:

- **Controlador de dominio con el rol PDC**: esta máquina es el origen de la hora autorizado para un dominio. Tendrá la hora más precisa disponible en el dominio, y debe sincronizarse con un controlador de dominio en el dominio primario, excepto en los casos en los que el rol GTIMESERV esté habilitado.
- **Cualquier otro controlador de dominio**: esta máquina actuará como origen de la hora para los clientes y los servidores miembro del dominio. Un controlador de dominio puede sincronizarse con el controlador de dominio principal de su propio dominio o con cualquier controlador de dominio de su dominio primario.
- **Clientes/servidores miembro**: esta máquina puede sincronizarse con cualquier controlador de dominio o controlador de dominio principal de su propio dominio, o con uno del dominio primario.

En función de los candidatos disponibles, se usa un sistema de puntuación para buscar el origen con la mejor hora. Este sistema tiene en cuenta la confiabilidad del origen de la hora y su ubicación relativa. Esto tiene lugar cuando se inicia el servicio de la hora. Si necesitas tener un control más preciso de cómo se sincroniza la hora, puedes agregar servidores horarios que funcionen correctamente en ubicaciones específicas o agregar redundancia. Para obtener más información, consulta la sección Especificación de un servicio de hora local confiable mediante GTIMESERV.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Entornos de sistema operativo mixtos (Win2012R2 y Win2008R2)

Aunque se requiere un entorno de dominio puro de Windows Server 2016 para obtener la mejor precisión, todavía hay ventajas en un entorno mixto. La implementación de Hyper-V de Windows Server 2016 en un dominio de Windows 2012 beneficiará a los invitados debido a las mejoras que hemos mencionado anteriormente, pero solo si los invitados también son de Windows Server 2016. Un controlador de dominio principal de Windows Server 2016 podrá ofrecer una hora más precisa debido a los algoritmos mejorados, por lo que será un origen más estable. Dado que es posible que el reemplazo de tu controlador de dominio principal no sea una opción, puedes agregar un controlador de dominio de Windows Server 2016 con el conjunto de implementación de GTIMESERV, lo que proporcionaría una actualización de precisión para su dominio. Un controlador de dominio de Windows Server 2016 puede ofrecer una hora mejorada para los clientes de hora de bajada, pero solo es tan preciso como su hora NTP de origen.

Así mismo, como se indicó anteriormente, las frecuencias de sondeo y actualización del reloj se han modificado con Windows Server 2016. Pueden cambiarse manualmente en los controladores de dominio de nivel inferior o aplicarse a través de la directiva de grupo. Aunque no hemos probado estas configuraciones, deberían ofrecer un buen comportamiento en Win2008R2 y Win2012R2, así como algunas ventajas.

Las versiones anteriores a Windows Server 2016 presentaban varios problemas para mantener la precisión de la hora, lo que provocó que la hora del sistema experimentara una deriva inmediatamente después de que se realizara un ajuste. Por este motivo, la obtención de muestras de hora de un origen NTP preciso con frecuencia y el acondicionamiento del reloj local con los datos da lugar a una deriva más reducida de los relojes del sistema en el período de muestreo interno, lo que produce un tiempo de mantenimiento mejorado en las versiones del sistema operativo de nivel inferior. La mejor precisión observada era de aproximadamente 5 ms cuando un cliente NTP de Windows Server 2012R2, establecido con la configuración de alta precisión, sincronizó su hora desde un servidor NTP de Windows 2016 preciso.

En algunos escenarios que implican controladores de dominio invitados, las muestras de TimeSync de Hyper-V pueden interrumpir la sincronización de la hora del dominio. Esto ya no debe resultar un problema para los invitados de Server 2016 que se ejecutan en hosts de Hyper-V de Server 2016.

Para deshabilitar el servicio TimeSync de Hyper-V a fin de que no proporcione muestras a W32Time, establezca la siguiente clave del Registro de invitado: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider
 "Enabled"=dword:00000000`.

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>Permiso de Linux para usar la hora del host de Hyper-V

En el caso de los invitados de Linux que se ejecutan en Hyper-V, normalmente los clientes se configuran para usar el demonio NTP para la sincronización de hora con los servidores NTP. Si la distribución de Linux admite el protocolo de la versión 4 de TimeSync y el invitado de Linux tiene habilitado el servicio de integración de TimeSync, se sincronizará con la hora del host. Esto puede dar lugar a una precisión de hora incoherente si ambos métodos están habilitados.

Para realizar la sincronización exclusivamente con la hora del host, te recomendamos que deshabilites la sincronización de hora de NTP de una de las siguientes maneras:

- Deshabilitación de los servidores NTP en el archivo ntp.conf

- Deshabilitación del demonio NTP

En esta configuración, el parámetro de servidor horario es este host. Su frecuencia de sondeo es de 5 segundos y la frecuencia de actualización del reloj también es de 5 segundos.

Para realizar la sincronización exclusivamente a través de NTP, te recomendamos que deshabilites el servicio de integración de TimeSync en el invitado.

> [!NOTE]
> Nota: La compatibilidad de la hora precisa con los invitados de Linux requiere una característica que solo se admite en los kernels de Linux ascendentes más recientes y esto no es algo que esté ampliamente disponible en todas las distribuciones de Linux. Consulta [Máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](../../virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md) para obtener más información sobre las distribuciones de soporte técnico.

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>Especificación de un servicio de hora confiable local mediante GTIMESERV

Puedes especificar uno o varios controladores de dominio como relojes de origen precisos mediante el uso de las marcas, el servidor horario preciso y GTIMESERV. Por ejemplo, los controladores de dominio específicos equipados con hardware GPS se pueden marcar como GTIMESERV. Esto garantizará que el dominio hace referencia a un reloj basado en hardware GPS.

> [!NOTE]
> Puedes encontrar más información sobre las marcas de dominio en la [documentación del protocolo MS-ADTS](/openspecs/windows_protocols/ms-winerrata/fe563333-6e4f-4198-9bf5-741a523cd0d7).

TIMESERV es otra marca de servicios de dominio relacionada que indica si una máquina está autorizada actualmente; esto puede cambiar si un controlador de dominio pierde la conexión. Un controlador de dominio en este estado devolverá "capa desconocida" cuando se le realicen consultas a través de NTP. Después de realizar varios intentos, el controlador de dominio registrará el evento Time-Service  36 de eventos del sistema.

Si quieres configurar un controlador de dominio como GTIMESERV, puedes configurarlo manualmente con el siguiente comando. En este caso, el controlador de dominio usa otras máquinas como el reloj maestro. Podría ser un dispositivo o una máquina dedicada.

```
w32tm /config /manualpeerlist:"master_clock1,0x8 master_clock2,0x8" /syncfromflags:manual /reliable:yes /update
```

> [!NOTE]
> Para obtener más información, consulta [Configuración del servicio de hora de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191(v=ws.10)).

Si el controlador de dominio tiene instalado hardware GPS, debes seguir estos pasos para deshabilitar el cliente NTP y habilitar el servidor NTP.

Para empezar, deshabilita el cliente NTP y habilita el servidor NTP con estos cambios en la clave del Registro.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f
```

A continuación, usa el comando siguiente para reiniciar el servicio de hora de Windows:

```
net stop w32time && net start w32time
```

Por último, indica que esta máquina tiene un origen de hora confiable mediante el comando siguiente:

```
w32tm /config /reliable:yes /update
```

Para comprobar que los cambios se han realizado correctamente, puedes ejecutar los siguientes comandos que afectan a los resultados que se muestran a continuación.

```
w32tm /query /configuration

Value|Expected Setting|
----- | ----- |
AnnounceFlags| 5 (Local)|
NtpServer |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 (Local)|
NtpClient| (Local)|

w32tm /query /status /verbose

Value| Expected Setting|
----- | ----- |
Stratum| 1 (primary reference - syncd by radio clock)|
ReferenceId| 0x4C4F434C (source name: "LOCAL")|
Source| Local CMOS Clock|
Phase Offset| 0.0000000s|
Server Role| 576 (Reliable Time Service)|
```

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 en plataformas virtuales de terceros

Cuando Windows se virtualiza, el hipervisor es responsable de proporcionar la hora de forma predeterminada. Sin embargo, los miembros unidos a un dominio deben sincronizarse con el controlador de dominio para que Active Directory funcione correctamente. Lo ideal es deshabilitar la virtualización en cualquier momento entre el invitado y el host de cualquier plataforma virtual de terceros.

#### <a name="discovering-the-hierarchy"></a>Detección de la jerarquía

Dado que la cadena de jerarquía de tiempo para el origen del reloj maestro es dinámica en un dominio y se negocia, deberás consultar el estado de una máquina determinada para comprender el origen de la hora y la cadena en el reloj de origen maestro. Esto puede ayudarte a diagnosticar problemas de sincronización de la hora.

Dado que te interesa solucionar problemas de un cliente específico, el primer paso consiste en comprender su origen de la hora mediante este comando de W32tm:

```
w32tm /query /status
```

Los resultados muestran el origen entre otras cosas. El origen indica con qué sincronizas la hora en el dominio. Este es el primer paso de la jerarquía de tiempo de esta máquina.
A continuación, usa la entrada de origen anterior y el parámetro/StripChart para buscar el siguiente origen de la hora de la cadena.

```
w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1
```

También resulta útil el siguiente comando, que muestra una lista de cada controlador de dominio que puedes encontrar en el dominio especificado y genera un resultado que te permite determinar cada asociado. Este comando incluirá las máquinas configuradas manualmente.

 w32tm /monitor /domain:my_domain

Mediante la lista, puedes realizar un seguimiento de los resultados a través del dominio y comprender la jerarquía, así como el desfase de hora en cada paso. Al localizar el punto en el que el desfase de hora es significativamente peor, puedes identificar la raíz de la hora incorrecta. Desde allí, puedes intentar comprender por qué la hora es incorrecta mediante la activación del registro W32tm.

#### <a name="using-group-policy"></a>Uso de la directiva de grupo
Puedes usar la directiva de grupo para lograr una precisión más estricta, por ejemplo, al asignar a clientes para que usen servidores NTP específicos o controlen cómo se configuran los sistemas operativos de nivel inferior cuando están virtualizados.
A continuación, se muestra una lista de posibles escenarios y opciones de configuración pertinentes para la directiva de grupo:

**Dominios virtualizados**: para controlar los controladores de dominio virtualizados en Windows 2012R2 para que sincronicen la hora con su dominio, en lugar de con el host de Hyper-V, puedes deshabilitar esta entrada del registro.  En el caso del controlador de dominio principal, no te recomendamos que deshabilites la entrada, ya que el host de Hyper-V proporcionará el origen de la hora más estable. La entrada del registro requiere que reinicies el servicio W32Time una vez que se haya cambiado.

 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled"=dword:00000000

**Cargas que exigen precisión**: para las cargas de trabajo que requieren precisión horaria, puedes configurar grupos de máquinas para establecer los servidores NTP y cualquier configuración de hora relacionada, como el sondeo y la frecuencia de actualización del reloj. Normalmente, esto lo controla el dominio, pero, para obtener un mayor control, puedes dirigirte a máquinas específicas para que apunten directamente al reloj maestro.

Configuración de directiva de grupo| Valor nuevo|
----- | ----- |
NtpServer| ClockMasterName,0x8|
MinPollInterval| 6 – 64 segundos|
MaxPollInterval| 6|
UpdateInterval| 100 (una vez por segundo)|
EventLogFlags| 3 (registro de hora especial)|

> [!NOTE]
> Los valores de configuración NtpServer y EventLogFlags se encuentran en System\Windows Time Service\Time Providers using the Configure Windows NTP Client settings. Los otros tres se encuentran en System\Windows Time Service using the Global Configuration settings.

**Cargas remotas que requieren precisión**: para los sistemas de los dominios de ramas, por ejemplo, el sector de crédito de pago (PCI) y minorista, Windows utiliza la información del sitio actual y el localizador del controlador de dominio para buscar un controlador de dominio local, a menos que haya un origen de la hora NTP manual. Este entorno requiere un segundo de precisión, de modo que usa una convergencia más rápida a la hora correcta. Esta opción permite al servicio W32Time retrasar el reloj. Si esto es aceptable y cumples tus requisitos, puedes crear la directiva siguiente.  Al igual que con cualquier entorno, asegúrate de probar la red y establecerla como base de referencia.

Configuración de directiva de grupo| Valor nuevo|
----- | ----- |
MaxAllowedPhaseOffset| 1, si es superior a en segundo, establece el reloj en la hora correcta.|

La configuración MaxAllowedPhaseOffset se encuentra en System\Windows Time Service using the Global Configuration settings.

> [!NOTE]
> Para obtener más información sobre la directiva de grupo y las entradas relacionadas, consulta [Configuración y herramientas del servicio de hora de Windows](windows-time-service-tools-and-settings.md) y el artículo Configuración en TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Consideraciones sobre IaaS de Azure y Windows

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina virtual de Azure: Servicios de dominio de Active Directory
Si la máquina virtual de Azure que ejecuta Active Directory Domain Services forma parte de un bosque de Active Directory local existente, se debe deshabilitar TimeSync (VMIC). De este modo, todos los controladores de dominio del bosque, tanto físicos como virtuales, pueden usar una jerarquía de sincronización de una sola hora. Consulta las notas del producto con procedimientos recomendados ["Ejecución de controladores de dominio en Hyper-V"](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)).

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina virtual de Azure: Máquina unida a un dominio
Si hospedas una máquina que está unida a un dominio en un bosque de Active Directory existente, virtual o física, el procedimiento recomendado es deshabilitar TimeSync para el invitado y asegurarse de que W32Time está configurado para sincronizarse con su controlador de dominio mediante la configuración de la hora para el tipo NTP5.

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina virtual de Azure: Máquina de grupo de trabajo independiente
Si la máquina virtual de Azure no está unida a ningún dominio, ni tampoco es un controlador de dominio, te recomendamos que mantengas la configuración de hora predeterminada y hagas que la máquina virtual se sincronice con el host.

## <a name="windows-application-requiring-accurate-time"></a>Aplicación para Windows que requiere una hora precisa
### <a name="time-stamp-api"></a>API de marca de tiempo
Los programas que requieren mayor precisión con respecto a la hora UTC, y no el paso del tiempo, deben usar la [API de GetSystemTimePreciseAsFileTime](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getsystemtimepreciseasfiletime). De este modo, se garantiza que la aplicación obtiene la hora del sistema, que está condicionada por el servicio de hora de Windows.

### <a name="udp-performance"></a>Rendimiento de UDP
Si tienes una aplicación que usa la comunicación UDP para las transacciones y es importante minimizar la latencia, hay algunas entradas del registro relacionadas que puedes usar para configurar un intervalo de puertos que se van a excluir del puerto del Motor de filtrado de base. De este modo, se mejorará la latencia y aumentará el rendimiento. Sin embargo, los cambios en el registro deben limitarse a los administradores experimentados. Además, esta solución alternativa evita que los puertos estén protegidos por el firewall. Para obtener más información, consulta la referencia del artículo siguiente.

En Windows Server 2012 y Windows Server 2008, primero deberás instalar una revisión. Puedes hacer referencia a este artículo de Knowledge Base: [Pérdida de datagramas al ejecutar una aplicación de receptor de multidifusión en Windows 8 y Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Actualización de los controladores de red
Algunos proveedores de red tienen actualizaciones de controladores que mejoran el rendimiento con respecto a la latencia del controlador y el almacenamiento en búfer de paquetes UDP. Ponte en contacto con tu proveedor de red para ver si hay actualizaciones que te ayuden con el rendimiento de UDP.

## <a name="logging-for-auditing-purposes"></a>Registro para fines de auditoría
Para cumplir las normativas de seguimiento de la hora, puedes archivar manualmente los registros W32tm, los registros de eventos y la información del monitor de rendimiento. Más adelante, la información archivada se puede usar para atestiguar el cumplimiento en un momento específico del pasado. Los siguientes factores se usan para indicar la precisión.

1. Precisión del reloj mediante el contador del monitor de rendimiento Desfase de hora calculado: muestra el reloj con la precisión deseada.
2. Origen del reloj que busca el "origen de la respuesta homóloga" en los registros W32tm.  Después del texto del mensaje, hay la dirección IP o VMIC, que describe el origen de la hora y el siguiente elemento de la cadena de relojes de referencia que se validará.
3. Estado de la condición del reloj que usa los registros W32tm para validar que se produce la "disciplina ClockDispl: \*SKEW\*TIME\*". Indica que W32tm está activo en ese momento.

### <a name="event-logging"></a>Registro de eventos
Para obtener la historia completa, también necesitarás información de registro de eventos. Al recopilar el registro de eventos del sistema y filtrar por Time-Server, Microsoft-Windows-Kernel-Boot y Microsoft-Windows-Kernel-General, es posible que puedas detectar si hay otros factores que han causado el cambio de la hora, por ejemplo, otros terceros. Estos registros pueden ser necesarios para descartar interferencias externas. La directiva de grupo puede afectar a los registros de eventos que se escriben en el registro. Consulta la sección anterior sobre el uso de la directiva de grupo para obtener más detalles.

### <a name="w32time-debug-logging"></a><a name="W32Logging"></a>Registro de depuración W32time
Para habilitar W32tm con fines de auditoría, el siguiente comando habilita el registro que muestra las actualizaciones periódicas del reloj e indica el reloj de origen. Reinicia el servicio para habilitar el nuevo registro.

Para obtener más información, consulta [Cómo activar el registro de depuración en el servicio de hora de Windows](https://support.microsoft.com/kb/816043).

 w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Monitor de rendimiento
El servicio de hora de Windows de Windows Server 2016 expone contadores de rendimiento que se pueden usar para recopilar el registro para la auditoría. Estos se pueden registrar localmente o de forma remota. Puedes registrar los contadores del desfase de hora del equipo y del retraso de recorrido de ida y vuelta.
Y, al igual que cualquier contador de rendimiento, puedes supervisarlos de forma remota y crear alertas mediante System Center Operations Manager. Por ejemplo, puedes usar una alerta para que te avise cuando el desfase de hora experimenta una deriva de la precisión deseada. El [Módulo de administración de System Center](https://www.microsoft.com/download/details.aspx?id=44231) contiene más información.

### <a name="windows-traceability-example"></a>Ejemplo de rastreabilidad de Windows
En los archivos de registro W32tm, te recomendamos que valides dos fragmentos de información. El primero es una indicación de que el archivo de registro actualmente es el reloj de condición. Esto demuestra que el reloj estaba condicionado por el servicio de hora de Windows en el momento conflictivo.

 151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW* TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307 151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW* TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41 151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW* TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

El punto principal es que los mensajes se ven precedidos por la disciplina de ClockDispln, que es la prueba de que W32Time interactúa con el reloj del sistema.

A continuación, debes buscar el informe más reciente en el registro antes del momento conflictivo que informa el equipo de origen que actualmente se usa como reloj de referencia. Puede ser una dirección IP, un nombre de equipo o el proveedor VMIC, lo que indica que se está sincronizando con el host para Hyper-V. En el ejemplo siguiente se proporciona la dirección IPv4 10.197.216.105.

 151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Ahora que has validado el primer sistema de la cadena de tiempo de referencia, debes investigar el archivo de registro en el origen de la hora de referencia y repetir los mismos pasos. Este proceso continúa hasta que obtienes un reloj físico, como un GPS o un origen de la hora conocido como NIST. Si el reloj de referencia es hardware GPS, es posible que también se requieran los registros de la fabricación.

## <a name="network-considerations"></a>Consideraciones sobre la red
Los algoritmos del protocolo NTP dependen de la simetría de la red. A medida que aumenta el número de saltos de la red, aumenta la probabilidad de asimetría. Por lo tanto, es difícil predecir qué tipos de precisiones se verán en los entornos específicos.

El monitor de rendimiento y los nuevos contadores de hora de Windows en Windows Server 2016 se pueden usar para evaluar la precisión de los entornos y crear bases de referencia. Además, puedes solucionar problemas para determinar el desfase actual de cualquier máquina de la red.

Hay dos estándares generales para la hora precisa en la red: PTP ([protocolo de tiempo de precisión, IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)), que tiene requisitos más estrictos en la infraestructura de red, pero a menudo puede proporcionar una precisión de submicrosegundo; y NTP ([protocolo de tiempo de red, RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona en una amplia variedad de redes y entornos, lo que facilita la administración.

Windows admite NTP simple (RFC2030) de forma predeterminada para las máquinas que no están unidas a un dominio. En el caso de las máquinas unidas a un dominio, usamos un protocolo NTP seguro llamado [MS-SNTP](/openspecs/windows_protocols/ms-sntp/8106cb73-ab3a-4542-8bc8-784dd32031cc), que aprovecha los secretos negociados del dominio que proporcionan una ventaja de administración sobre el protocolo NTP autenticado descrito en RFC1305 y RFC5905.

Tanto los protocolos que están unidos a un dominio como los que no lo están requieren el puerto UDP 123. Para obtener más información sobre los procedimientos recomendados de NTP, consulta el [borrador de IETF sobre los procedimientos recomendados más recientes del protocolo de tiempo de red](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Reloj de hardware confiable (RTC)

Windows no establece la hora, a menos que se superen determinados límites, si no que en su lugar sincroniza el reloj. Esto significa que W32tm ajusta la frecuencia del reloj a intervalos regulares, mediante la configuración de frecuencia de actualización del reloj, que se establece como valor predeterminado en una vez por segundo con Windows Server 2016. Si el reloj está atrasado, acelera la frecuencia y, si está adelantado, ralentiza la frecuencia. Sin embargo, durante ese tiempo entre los ajustes de frecuencia del reloj, el reloj del hardware está controlado. Si hay un problema con el reloj del hardware o firmware, la hora de la máquina puede ser menos precisa.

Este es otro motivo por el que debes probar tu entorno y establecerlo como base de referencia. Si el contador de rendimiento "Desfase de hora calculado" no se estabiliza a la precisión de destino, puede que quieras verificar que el firmware esté actualizado. Como otra prueba, puedes ver si el hardware duplicado reproduce el mismo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solución de problemas con la precisión horaria y NTP

Puedes usar la sección Detección de la jerarquía anterior para comprender el origen de la hora inexacta. Cuando observes el desfase de hora, busca el punto de la jerarquía en el que la hora diverge al máximo de su origen NTP. Una vez que comprendas la jerarquía, querrás probar y comprender por qué ese origen de la hora determinado no recibe una hora precisa.

Si te centras en el sistema con una hora divergente, puedes usar estas herramientas para recopilar más información que te ayude a determinar el problema y encontrar una solución. La siguiente referencia UpstreamClockSource es el reloj detectado mediante "W32tm/config/status".

- Registros de eventos del sistema
- Habilita el registro mediante el comando siguiente: w32tm logs - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- w32Time Registry key HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Seguimientos de la red local
- Contadores de rendimiento (de la máquina local o UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- Haz ping a UpstreamClockSource para comprender la latencia y el número de saltos en el origen.
- Tracert UpstreamClockSource

Problema| Síntomas| Solución|
----- | ----- | ----- |
| El reloj de TSC local no es estable.| El reloj se estabiliza mediante el reloj Perfmon - Physical Computer – Sync, pero sigues viendo el problema cada 1 o 2 minutos de varios 100 microsegundos. | Si se actualiza el firmware o se valida hardware diferente, no se muestra el mismo problema.|
| Latencia de red| w32tm stripchart muestra un valor de RoundTripDelay superior a 10 ms. La variación en el retraso produce un ruido hasta la mitad del tiempo del recorrido de ida y vuelta, por ejemplo, un retraso que solo se genera en una dirección.<br><br>UpstreamClockSource realiza varios saltos, tal y como se indica al hacer ping. El período de vida debe estar cerca de 128.<br><br>Usa tracert para encontrar la latencia en cada salto.  | Busca un origen de reloj más cercano para la hora. Una solución consiste en instalar un reloj de origen en el mismo segmento o seleccionar manualmente el reloj de origen que está más cerca geográficamente. Para un escenario de dominio, agrega una máquina con el rol GTimeServ. |
No se puede alcanzar de forma confiable el origen NTP.| W32tm /stripchart devuelve intermitentemente el mensaje "se agotó el tiempo de espera de la solicitud". |El origen NTP no responde.|
El origen NTP no responde.| Comprueba los contadores Perfmon para el recuento de orígenes de cliente NTP, las solicitudes entrantes del servidor NTP y las respuestas salientes del servidor NTP, y determina el uso en comparación con las bases de referencia.| Con los contadores de rendimiento del servidor, determina si la carga ha cambiado en referencia a las bases de referencia.<br><br>¿Hay problemas de congestión de la red?|
El controlador de dominio no usa el reloj más preciso.| Cambios en la topología o en el reloj maestro agregado recientemente.| w32tm /resync /rediscover|
Los relojes de los clientes experimentan una deriva| Evento 36 de Time-Service en el registro de eventos del sistema o texto del archivo de registro que describe lo siguiente: Contador "Recuento de orígenes de la hora del cliente NTP" que va de 1 a 0|Soluciona el problema con el origen ascendente y comprende si está experimentando problemas de rendimiento.|

### <a name="baselining-time"></a>Hora de la base de referencia

Establecer una base de referencia es importante para que puedas comprender el rendimiento y la precisión de la red, y compararlos con los de la base de referencia en el futuro cuando se produzcan problemas. Te recomendamos que establezcas el controlador de dominio principal raíz o cualquier máquina marcada con GTIMESRV como base de referencia. Así mismo, te recomendamos que establezcas el controlador de dominio principal como base de referencia en cada bosque. Por último, elige los controladores de dispositivos críticos o las máquinas que tengan características interesantes, como las cargas altas o de distancia, y establézcalos como base de referencia.

También resulta útil establecer Windows Server 2016 como base de referencia frente a 2012 R2, sin embargo, solo contarás con la herramienta w32tm /stripchart para realizar la comparación, ya que Windows Server 2012R2 no tiene contadores de rendimiento. Debes elegir dos máquinas con las mismas características, o actualizar una máquina y comparar los resultados después de la actualización. En el anexo Medidas de tiempo de Windows se ofrece más información sobre cómo realizar mediciones detalladas entre 2016 y 2012.

Con todos los contadores de rendimiento de W32Time, recopila datos durante al menos una semana. De este modo, podrás asegurarte de que tienes suficientes referencias para considerar varios en la red a lo largo del tiempo y que obtienes lo suficiente en una ejecución para confiar en que la precisión de la hora es estable.

### <a name="ntp-server-redundancy"></a>Redundancia del servidor NTP

Para la configuración manual del servidor NTP que se usa con máquinas que no están unidas a un dominio o con el controlador de dominio principal, tener más de un servidor es una buena medida de redundancia en caso de disponibilidad. También podría proporcionar una mejor precisión, suponiendo que todos los orígenes son precisos y estables. Sin embargo, si la topología no está bien diseñada o los orígenes de la hora no son estables, la precisión resultante podría ser peor, por lo que te recomendamos que tengas cuidado. El límite de servidores horarios admitidos a los que W32Time puede hacer referencia manualmente es de 10.

## <a name="leap-seconds"></a>Segundos intercalares

El período de rotación de la Tierra varía con el tiempo, debido a eventos climáticos y geológicos. Normalmente, la variación es aproximadamente de un segundo cada dos años. Siempre que la variación de la hora atómica crece demasiado, se inserta una corrección de un segundo (arriba o abajo), denominada secundo intercalar. Esto se hace de forma que la diferencia nunca supere los 0,9 segundos. Esta corrección se anuncia seis meses antes de la corrección real. Antes de Windows Server 2016, el servicio de hora de Microsoft no consideraba los segundos intercalares, sino que se basaba en el servicio de hora externo para corregirlo. Con el aumento de la precisión horaria de Windows Server 2016, Microsoft está trabajando en una solución más adecuada para el problema con los segundos intercalares.

## <a name="secure-time-seeding"></a>Propagación de la hora segura

W32time en Server 2016 incluye la característica de propagación de la hora segura. Esta característica determina la hora actual aproximada de las conexiones SSL salientes. Este valor de tiempo se usa para supervisar el reloj del sistema local y corregir cualquier error bruto. Puede obtener más información sobre la característica en [esta entrada de blog](/archive/blogs/w32time/secure-time-seeding-improving-time-keeping-in-windows). En las implementaciones con un origen de hora confiable y máquinas bien supervisadas que incluyen la supervisión del desfase de hora, puedes optar por no usar la característica de propagación de la hora segura y confiar en la infraestructura existente.

Puedes deshabilitar la característica con estos pasos:

1. Establece el valor de configuración del registro UtilizeSSLTimeData en 0 en una máquina específica:

    ```
    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f
    ```

2. Si no puedes reiniciar la máquina inmediatamente por algún motivo, puedes enviar una notificación al servicio W32time sobre la actualización de la configuración. De este modo, se detendrá la supervisión y la aplicación de la hora en función de los datos recopilados en las conexiones SSL.

    ```
    W32tm.exe /config /update
    ```

3. Al reiniciar la máquina, la configuración se aplica de inmediato y también se detiene la recopilación de datos de las horas de las conexiones SSL. Esta última parte representa una sobrecarga muy pequeña y no debe significar un problema de rendimiento.

4. Para aplicar esta configuración en un dominio completo, establece el valor UtilizeSSLTimeData de la configuración de la directiva de grupo W32time en 0 y publica la configuración. Cuando un cliente de la directiva de grupo seleccione la configuración, se enviará una notificación al servicio W32time y se detendrá la supervisión y la aplicación de la hora mediante los datos de las horas de SSL. La recopilación de los datos de las horas de SSL se detendrá cuando se reinicie cada máquina. Si el dominio tiene portátiles o tabletas delgados y otros dispositivos, puede que quieras excluir estas máquinas de este cambio de directiva. Eventualmente, estos dispositivos abordarán la descarga de la batería y requerirán la característica de propagación de tiempo seguro para arrancar su hora.