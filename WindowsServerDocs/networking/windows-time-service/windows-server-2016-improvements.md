

## <a name="windows-server-2016-improvements"></a>Mejoras en Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP y servicio de hora de Windows
Windows Server 2016 ha mejorado los algoritmos que utiliza para la condición el reloj local se sincronicen con la hora UTC y la hora correcta.  NTP utiliza 4 valores para calcular la diferencia de tiempo, en función de las marcas de tiempo del cliente de solicitud/respuesta y solicitud/respuesta del servidor.  Sin embargo, las redes son ruidosas y puede haber picos en los datos de NTP debido a la congestión de la red y otros factores que afectan a la latencia de red.  Algoritmos de 2016 de Windows Media Este ruido mediante una serie de técnicas diferentes, lo que resulta en un reloj estable y preciso.  Además, el origen se usa para las referencias de la hora exacta una API mejorada que nos ofrece una mejor resolución.  Con estas mejoras se pueden conseguir 1 precisión de ms con respecto a UTC en un dominio.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 ha mejorado el servicio de Hyper-V TimeSync. Las mejoras incluyen tiempo inicial más precisa en el inicio de la máquina virtual o restauración de máquinas virtuales y corrección de latencia de interrupción para los ejemplos proporcionados a w32time.  Esta mejora nos permite mantenerse con de 10µs del host con un RMS, (raíz al cuadrado, lo que indica la varianza), de 50µs, incluso en un equipo con el 75% de carga. Para obtener más información, consulte [arquitectura Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> Carga se creó mediante pruebas comparativas de prime95 con perfil equilibrada.

Además, el nivel de los satélites que notifica el Host al invitado es más transparente.  Anteriormente, el Host presentaría un estrato fijo de 2, independientemente de su precisión.  Con los cambios en Windows Server 2016, el host indica un estrato una unidad mayor que la capa de host, que da como resultado un mejor momento para invitados virtuales.  La capa de host viene determinada por w32time por medios normales, según su hora de origen.  Los invitados encontrará el reloj más preciso, en lugar de con el host predeterminado de Windows 2016 Unidos a un dominio.  Era por esta razón que se recomienda deshabilitar manualmente la configuración del proveedor de hora de Hyper-V para los equipos que participan en un dominio en 2012 R2 de Windows y, a continuación.

### <a name="monitoring"></a>Supervisión
Se han agregado los contadores del monitor de rendimiento.  Estos permiten a la línea base, supervisan y solucionar problemas de precisión de tiempo.  Estos contadores incluyen:

Contador|Descripción|
----- | ----- |
Calcula la diferencia horaria|   La hora absoluta de desplazamiento entre el reloj del sistema y el origen de tiempo seleccionado, como calculado por el servicio W32Time en microsegundos. Cuando esté disponible un nuevo ejemplo válido, el tiempo calculado se actualiza con la diferencia de tiempo indicada por el ejemplo. Este es el desplazamiento de hora real del reloj local. W32Time inicia la corrección de reloj mediante este desplazamiento y actualiza el tiempo calculado entre muestras con el desplazamiento de tiempo restante que debe aplicarse el reloj local. Precisión de reloj puede controlarse mediante este contador de rendimiento con un intervalo de sondeo baja (p. ej.: 256 segundos o menos) y buscando el valor del contador sea menor que el límite de precisión de reloj deseado.|
Ajuste de la frecuencia de reloj| El ajuste de frecuencia de reloj absoluta realizado en el reloj del sistema local por W32Time en partes por mil millones. Este contador le ayuda a visualizar las acciones que se va a realizadas por W32time.|
Retraso de ida y vuelta NTP|    Retraso de ida y vuelta más reciente ha experimentado por el cliente de NTP en recibir una respuesta del servidor en microsegundos. Este es el tiempo transcurrido en el cliente NTP entre transmitir una solicitud para el servidor NTP y recibir una respuesta válida desde el servidor. Este contador le ayuda a caracterizar los retrasos experimentados por el cliente NTP. Ida y vuelta más grande o diferente puede agregar ruido para los cálculos de tiempo NTP, que a su vez pueden afectar a la precisión de la sincronización de hora a través de NTP.|
Recuento de origen de clientes NTP|    Número de orígenes de hora NTP siendo usada por el cliente de NTP activos. Se trata de un recuento de activos, distintas direcciones IP de servidores de hora que responden a las solicitudes de este cliente. Este número puede ser mayor o menor que los interlocutores configurados, según la resolución de DNS de nombres de mismo nivel y la capacidad de cobertura actual.|
Servidor NTP las solicitudes entrantes|   Número de solicitudes recibidas por el servidor NTP (solicitudes/segundo).|
Respuestas salientes del servidor NTP|  Número de solicitudes respondidas por el servidor NTP (respuestas/s).|

Los 3 primeros contadores dirigen a escenarios de solución de problemas de precisión.  La precisión de tiempo de solución de problemas y NTP sección a continuación, en [recomendaciones](#BestPractices), incluye más detalles.
Los 3 últimos contadores abarcan escenarios de servidor NTP y son útiles cuando determine la carga y la línea de base de su rendimiento actual.

### <a name="configuration-updates-per-environment"></a>Actualizaciones de configuración por entorno
El siguiente describe los cambios en la configuración predeterminada entre Windows 2016 y versiones anteriores para cada rol.  La configuración de Windows Server 2016 y actualización de aniversario de Windows 10 (compilación 14393), ahora son únicos por qué no existe, se muestran como columnas independientes. 

|Rol|Parámetro|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Servidor Nano o independiente**||||
| |*Servidor horario*|time.windows.com|N/A|time.windows.com|
| |*Frecuencia de sondeo*|64 - 1024 segundos|N/A|Una vez a la semana|
| |*Frecuencia de actualización de reloj*|Una vez por segundo|N/A|Una vez en una hora|
|**Cliente independiente**||||
| |*Servidor horario*|N/A|time.windows.com|time.windows.com|
| |*Frecuencia de sondeo*|N/A|Una vez al día|Una vez a la semana|
| |*Frecuencia de actualización de reloj*|N/A|Una vez al día|Una vez a la semana|
|**Controlador de dominio**||||
| |*Servidor horario*|PDC/GTIMESERV|N/A|PDC/GTIMESERV|
| |*Frecuencia de sondeo*|64-1024 segundos|N/A|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|Una vez al día|N/A|Una vez a la semana|
|**Servidor miembro de dominio**||||
| |*Servidor horario*|DC|N/A|DC|
| |*Frecuencia de sondeo*|64-1024 segundos|N/A|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|Una vez por segundo|N/A|Una vez cada 5 minutos|
|**Cliente de miembro de dominio**||||
| |*Servidor horario*|N/A|DC|DC|
| |*Frecuencia de sondeo*|N/A|1204 - 32768 segundos|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|N/A|Una vez cada 5 minutos|Una vez cada 5 minutos|
|**Invitado de Hyper-V**||||
| |*Servidor horario*|Elige la mejor opción según los satélites del servidor Host y la hora|Elige la mejor opción según los satélites del servidor Host y la hora|El valor predeterminado es Host|
| |*Frecuencia de sondeo*|Según la función anterior|Según la función anterior|Según la función anterior|
| |*Frecuencia de actualización de reloj*|Según la función anterior|Según la función anterior|Según la función anterior|

>[!NOTE]
>Para Linux en Hyper-V, consulte el [Linux, lo que permite usar el tiempo del Host de Hyper-V](#AllowingLinux) sección más adelante.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto de un aumento de sondeo y la frecuencia de actualización de reloj
Con el fin de proporcionar un tiempo más preciso, se incrementan los valores predeterminados para el sondeo de frecuencias y actualizaciones de reloj que nos permitirá realizar pequeños ajustes con más frecuencia.  Esto hará que el tráfico UDP/NTP más, sin embargo, estos paquetes son pequeños por lo que debe ser muy poco o ningún impacto en los vínculos de banda ancha. Sin embargo, la ventaja, que es la hora debe ser mejor en una amplia variedad de entornos y hardware.

Para los dispositivos de seguridad de la batería, aumentar la frecuencia de sondeo puede causar problemas.  Los dispositivos de la batería no almacenan el tiempo mientras se ha desactivado.  Cuando reanude, requieran correcciones frecuentes en el reloj.  Aumentar la frecuencia de sondeo hará que el reloj se vuelva inestable y también se puede usar más energía.  Microsoft recomienda que no cambiar la configuración predeterminada del cliente.

Los controladores de dominio deberían verse afectados al mínimo incluso con el efecto de las actualizaciones de un aumento de los clientes de NTP en un dominio de AD multiplicado.  NTP tiene un consumo de recursos mucho menor en comparación con otros protocolos y un efecto marginal.  Es más probable alcanzar los límites de otra funcionalidad de dominio antes de que se está afectado por la configuración de un aumento de Windows Server 2016.  Active Directory utiliza NTP segura, que tiende a la hora de sincronización con menor precisión que NTP simple, pero hemos comprobado escalará verticalmente a los satélites dos clientes lejos el PDC.

Como plan conservador, debe reservar 100 solicitudes por segundo por núcleo de NTP.  Por ejemplo, un dominio compuesto por 4 controladores de dominio con 4 núcleos, debe ser capaz de atender las solicitudes NTP 1600 por segundo.  Si tiene 10 clientes k configurados a la hora de sincronización una vez cada 64 segundos y las solicitudes se reciben de manera uniforme con el tiempo, verá 10 000/64 o unos 160 solicitudes/segundo, se reparten entre todos los controladores de dominio.  Esto se encuentra fácilmente dentro de nuestro 1600 NTP solicitudes por segundo según este ejemplo.  Estos son conservadoras recomendaciones para la planificación y por supuesto tienen grande depende de la red, velocidades del procesador y la carga, así como siempre previsto y pruebas en sus entornos.

También es importante tener en cuenta que si se ejecutan los controladores de dominio con una carga considerable de CPU, más del 40%, esto seguramente agregará ruido en las respuestas NTP y afectan a la precisión de su tiempo en su dominio.  De nuevo, debe probar en su entorno para descubrir los resultados reales.

## <a name="time-accuracy-measurements"></a>Medidas de precisión de tiempo
### <a name="methodology"></a>Metodología
Para medir la precisión de tiempo para Windows Server 2016, hemos usado una variedad de herramientas, métodos y entornos.  Puede utilizar estas técnicas para medir y optimizar su entorno y determinar si los resultados de precisión cumplan sus requisitos. 

Nuestro reloj de origen del dominio está formada por dos servidores NTP de alta precisión con hardware GPS.  También usamos una máquina de prueba de referencia para las mediciones, que también tenían instalado de otro fabricante de hardware GPS de alta precisión.  Para algunas de las pruebas, necesitará un origen de reloj precisas y confiables que se usará como una referencia además de su origen de reloj de dominio.

Se utilizan cuatro métodos diferentes para medir la precisión con máquinas virtuales y físicas. Varios métodos proporcionan los medios independientes para validar los resultados.


1. Medir el reloj local, que está condicionado por w32tm, nuestro equipo de pruebas de referencia que tiene hardware independiente de GPS.  
2.  Medida NTP pings desde el servidor NTP para los clientes que usan W32tm "stripchart"
3.  Medida NTP pings desde el cliente al servidor NTP mediante W32tm "stripchart"
4.  Da como resultado una medida Hyper-V desde el host al invitado mediante el contador de marca de tiempo (TSC).  Este contador se comparte entre las particiones y la hora del sistema en ambas particiones.  Se calcula la diferencia entre la hora de host y la hora de cliente en la máquina virtual.  A continuación, usamos el reloj TSC para interpolar el tiempo de host desde el invitado, dado que las medidas no ocurren al mismo tiempo.  Además, usamos el factor de reloj TSV retrasos y latencia de la API.

W32tm está integrada, pero las otras herramientas que hemos usado durante nuestras pruebas están disponibles para el repositorio de Microsoft en GitHub como código abierto para las pruebas y el uso.  El sitio WIKI en el repositorio tiene más información que describe cómo usar las herramientas para realizar mediciones.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Los resultados de pruebas que se muestra a continuación son un subconjunto de medidas que se hacen en uno de los entornos de prueba.  Muestra la precisión mantenida al principio de la jerarquía de tiempo y el cliente de dominio secundario al final de la jerarquía de tiempo.  Esto se compara con las mismas máquinas en una topología de 2012 en función de comparación.

### <a name="topology"></a>Topología
Para la comparación, hemos probado una topología en función de Windows Server 2016 y Windows Server 2012 R2.  Ambas topologías constan de dos equipos físicos de host de Hyper-V que hacen referencia a una máquina de Windows Server 2016 con GPS reloj hardware instalado.  Cada host ejecuta 3 invitados de windows Unidos a un dominio, que se organizan según la topología siguiente.  Las líneas representan la jerarquía de tiempo y el protocolo o transporte que se usa.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Información general de gráficos de resultados
Los dos gráficos siguientes representan la precisión de tiempo para dos miembros específicos en un dominio basado en la topología anterior.  Cada gráfico muestra el Windows Server 2012 R2 y 2016 resultados superpuestos, que demuestra las mejoras visualmente.  La precisión se miden desde de la máquina invitada en comparación con el host.  Los datos gráficos representan un subconjunto de todo el conjunto de pruebas que hemos hecho y muestran el mejor de los casos y los peores escenarios.  

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Rendimiento del dominio raíz del PDC
El PDC raíz se sincroniza con el host de Hyper-V (con VMIC) que es un equipo con Windows Server 2016 con hardware GPS que ha demostrado que sea precisa y estable.  Se trata de un requisito crítico para la precisión de 1 ms, que se muestra como el área sombreada verde.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Rendimiento del cliente de dominio secundario
El cliente de dominio secundario se adjunta a un PDC del dominio secundario que se comunica con el PDC raíz.  Tiempo también es dentro de la 1 ms requisito.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Pruebas de larga distancia
El gráfico siguiente compara el salto de red virtual 1 a 6 saltos de red físicos con Windows Server 2016.  Dos gráficos se superponen entre sí con transparencia para mostrar los datos que se superponen.  Saltos de red cada vez mayor significan una latencia mayor y desviaciones de tiempo mayor.  El gráfico es ampliada y por lo que el 1 ms límites, representados por el área de color verde, es más grande.  Como puede ver, el tiempo es aún dentro de 1 ms con varios saltos.  Negativamente desplazamiento, que muestra una asimetría de red.  Por supuesto, todas las redes es diferente, y las medidas dependen de un gran número de factores del entorno.

![Hora de Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Procedimientos recomendados para la sincronización de hora precisa
### <a name="solid-source-clock"></a>Reloj sólida de fuente
Solo es tan bueno como el reloj de origen con que se sincroniza un tiempo de las máquinas.  Con el fin de lograr 1 ms de precisión, necesitará el hardware GPS o a una aplicación de tiempo de la red que se hace referencia como el reloj de origen maestro.  Con el valor predeterminado de time.windows.com, no puede proporcionar un origen de hora local y estable.  Además, conforme se adentren lejos el reloj de origen, la red afecta a la precisión.  Tener un reloj de origen maestro en cada data center es necesaria para la mejor precisión.

### <a name="hardware-gps-options"></a>Opciones de hardware GPS
Existen varias soluciones de hardware que pueden ofrecer la hora exacta.  En general, las soluciones de hoy en día se basan en antenas GPS.  También hay radio y soluciones de módem de acceso telefónico mediante líneas dedicadas.  Se adjunte a la red como un dispositivo o conecte un equipo, por ejemplo Windows a través de un dispositivo USB o de PCIe.  Diferentes opciones ofrecerán diferentes niveles de precisión y, como siempre, los resultados dependen de su entorno.  Las variables que afectan a la precisión incluyen disponibilidad GPS, estabilidad de la red y de carga y Hardware para PC.  Estos son factores importantes al elegir un reloj de origen, como se indicó, es un requisito de tiempo preciso y estable.

### <a name="domain-and-synchronizing-time"></a>Dominio y la sincronización de hora
Los miembros del dominio usar la jerarquía de dominios para determinar a qué equipo utilice como origen para sincronizar la hora.  Cada miembro del dominio encontrarán otra máquina para sincronizar con y guárdelo como su origen de reloj.  Cada tipo de miembro del dominio sigue un conjunto de reglas diferentes con el fin de buscar un origen de reloj para la sincronización de hora.  El PDC en la raíz del bosque es el origen de reloj de forma predeterminada para todos los dominios.  A continuación aparecen distintos roles y la descripción de alto nivel cómo buscan un origen:


- **Controlador de dominio con el rol PDC** : este equipo es el origen autorizado de la hora de un dominio. Tiene la hora más precisa disponible en el dominio y debe sincronizar con un controlador de dominio en el dominio primario, excepto en casos donde [GTIMESERV](#GTIMESERV) rol está habilitado. 
- **Cualquier otro controlador de dominio** : este equipo actuará como un origen de hora para clientes y servidores miembro del dominio. Un controlador de dominio se puede sincronizar con el PDC de su propio dominio, o cualquier controlador de dominio en su dominio primario.
- **Miembro de los clientes o servidores** : esta máquina se puede sincronizar con cualquier controlador de dominio o el PDC de su propio dominio, o un controlador de dominio o PDC en el dominio primario.

En función de los candidatos disponibles, un sistema de puntuación se utiliza para buscar la mejor fuente de tiempo.  Este sistema tiene en cuenta la confiabilidad de la fuente de tiempo y su ubicación relativa.  Esto sucede una vez que cuando el tiempo se ha iniciado el servicio.  Si necesita tener un control más preciso de cómo sincroniza el tiempo, puede agregar servidores de buen momento en ubicaciones específicas o agregar redundancia.  Consulte la [especificar un Local confiable tiempo servicio utilizando GTIMESERV](#GTIMESERV) sección para obtener más información.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Entornos de sistema operativo mixto (Win2012R2 y Win2008R2)
Cuando un entorno puro de dominio de Windows Server 2016 es necesario para la mejor precisión, todavía hay ventajas en un entorno mixto.  Implementación de Windows Server 2016 Hyper-V en un dominio de Windows 2012, los invitados se beneficiarán debido a las mejoras que se mencionó anteriormente, pero únicamente si los invitados son también Windows Server 2016.  Un PDC de Windows Server 2016, podrá entregar precisa más tiempo debido a los algoritmos mejorados que será un origen más estable.  Como reemplazar su PDC no puede ser una opción, en su lugar, puede agregar un controlador de dominio de Windows Server 2016 con la [GTIMESERV](#GTIMESERV) conjunto que sería una actualización de la precisión para el dominio.  Un controlador de dominio de Windows Server 2016 puede proporcionar un mejor momento a los clientes de nivel inferior de tiempo, sin embargo, sólo es tan bueno como su hora NTP de origen.

También como se indicó anteriormente, se han modificado las frecuencias de sondeo y la actualización de reloj con Windows Server 2016.  Se pueden cambiar manualmente a los controladores de dominio de nivel inferior o aplicarse a través de la directiva de grupo.  Mientras que no hemos probado estas configuraciones, deben comportarse bien en Win2008R2 y Win2012R2 y ofrecer algunas ventajas.

Versiones anteriores a Windows Server 2016 tenía varios problemas de mantener la hora exacta que dieron lugar a la hora del sistema distancia inmediatamente después de realiza un ajuste de mantener.  Por este motivo, la obtención de muestras de tiempo desde un origen NTP preciso con frecuencia y el reloj local con los datos de acondicionamiento conduce a desfase más pequeño en sus relojes del sistema en el período dentro de un muestreo, lo que resulta en un mejor tiempo manteniendo en las versiones del sistema operativo de nivel inferior. La mejor precisión observada fue aproximadamente de 5 ms cuando un cliente de NTP de Windows Server 2012 R2, configurado con la configuración de alta precisión, sincroniza su hora de un servidor NTP de Windows 2016 preciso.

En algunos escenarios que implican los controladores de dominio de invitado, ejemplos de Hyper-V TimeSync pueden interrumpir la sincronización de hora de dominio.  Ya no debe ser un problema para que se ejecutan en hosts de Server 2016 Hyper-V de invitados de Server 2016.

Para deshabilitar el servicio de Hyper-V TimeSync de proporcionar ejemplos de w32time, establezca la siguiente clave del registro de invitado:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Lo que permite Linux para usar la hora del Host de Hyper-V
Para invitados de Linux que se ejecutan en Hyper-V, los clientes normalmente se configuran para usar el demonio NTP para la sincronización de hora en los servidores NTP.  Si la distribución de Linux es compatible con la versión 4 del protocolo TimeSync y el invitado Linux tiene habilitado el servicio de integración de TimeSync, se sincronizarán con el tiempo de host. Esto podría provocar tiempo incoherente mantener si están habilitados ambos métodos.

Para sincronizar exclusivamente con el tiempo de host, se recomienda deshabilitar la sincronización de hora NTP, ya sea por:

- Deshabilitación de los servidores NTP en el archivo ntp.conf
- o deshabilitar el demonio NTP

En esta configuración, el parámetro de servidor horario es este host.  Su frecuencia de sondeo es de 5 segundos y la frecuencia de actualización de reloj también es de 5 segundos.

Para sincronizar exclusivamente a través de NTP, se recomienda deshabilitar el servicio de integración de TimeSync en el invitado.

> [!NOTE]
> Nota:  Compatibilidad con la hora exacta con los invitados Linux requiere una característica que solo se admite en los kernels de Linux más reciente precede y no es algo que es ampliamente disponible en todas las distribuciones de Linux todavía. Haga referencia a [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obtener más detalles sobre las distribuciones de soporte técnico.

#### <a name="GTIMESERV"></a>Especifique un servicio de hora confiable Local mediante GTIMESERV
Puede especificar uno o más controladores de dominio como los relojes de origen precisa mediante el uso de la GTIMESERV, buen servidor horario, marcas.  Por ejemplo, controladores de dominio específicos equipados con hardware GPS pueden marcarse como un GTIMESERV.  Así se asegurará de su dominio hace referencia a un reloj en función del hardware GPS.

> [!NOTE]
> Puede encontrar más información acerca de las marcas de dominio en el [documentación del protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV es otro relacionados dominio servicios de marca que indica si un equipo está actualmente autorizado, que puede cambiar si un controlador de dominio pierde la conexión.  Un controlador de dominio en este estado devolverá "Desconocido estrato" cuando se consulta a través de NTP.  Tras varios intentos, el controlador de dominio registrará System Event-servicio de hora de evento 36.

Si desea configurar un controlador de dominio como un GTIMESERV, esto se puede configurar manualmente mediante el comando siguiente.  En este caso el controlador de dominio está usando otras máquinas como el reloj principal.  Podría tratarse de un dispositivo o equipo dedicado.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obtener más información, consulte [configurar el servicio de hora de Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si el controlador de dominio tiene instalado el hardware GPS, deberá usar estos pasos para habilitar al servidor NTP y deshabilitar al cliente NTP.

Comience por deshabilitar al cliente de NTP y habilitar al servidor NTP con estos cambios de clave del registro.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

A continuación, reinicie el servicio de hora de Windows

    net stop w32time && net start w32time

Por último, indica que esta máquina tiene un origen de hora confiable mediante.
   
    w32tm /config /reliable:yes /update

Para comprobar que los cambios se han realizado correctamente, puede ejecutar los comandos siguientes que afectan a los resultados que se muestra a continuación. 

    w32tm /query /configuration

Valor|Configuración esperada|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 (local)|
NtpClient|  (Local)|

    w32tm /query /status /verbose

Valor|  Configuración esperada|
----- | ----- |
Satélites|    1 (referencia principal - sincronizada mediante el reloj de radio)|
ReferenceId|    0x4C4F434C (nombre de origen:  "LOCAL")|
Source| Reloj CMOS local|
Desplazamiento de fase|   0.0000000s|
Rol de servidor|    576 (servicio de hora confiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 en 3rd Party plataformas Virtual
Cuando se virtualiza Windows, de forma predeterminada el hipervisor es responsable de proporcionar un tiempo.  Pero los miembros deben ser está sincronizado con el controlador de dominio para Active Directory funcione correctamente unida al dominio.  Es mejor deshabilitar la virtualización de cualquier hora entre el invitado y el host de las plataformas de terceros virtual 3rd.

#### <a name="discovering-the-hierarchy"></a>Detección de la jerarquía
Puesto que la cadena de jerarquía de tiempo para el origen de reloj principal es dinámico en un dominio y negociados, deberá consultar el estado de un equipo concreto para entender es el origen de hora y la cadena al reloj del origen maestro.  Esto puede ayudar a diagnosticar problemas de sincronización de hora.

Dado desea solucionar problemas de un cliente específico; el primer paso es comprender su origen de hora mediante este comando w32tm.

    w32tm /query /status

Los resultados muestran el origen, entre otras cosas.  El origen indica con los que sincronizar la hora en el dominio.  Este es el primer paso de esta jerarquía de tiempo de las máquinas.
A continuación, use la entrada de origen de arriba y use el parámetro de /StripChart para encontrar el origen de la hora siguiente de la cadena.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

También es útil, el siguiente comando enumera cada controlador de dominio que puede encontrar en el dominio especificado e imprime un resultado que permite determinar a cada socio comercial.  Este comando incluirá las máquinas que se han configurado manualmente.
    
    w32tm /monitor /domain:my_domain

En la lista, puede trazar los resultados a través del dominio y comprender la jerarquía, así como el desplazamiento de tiempo en cada paso.  Al buscar el punto donde el desplazamiento de hora obtiene mucho peor, puede aislar la raíz de la hora incorrecta.  Desde allí puede intentar comprender por qué esa hora es incorrecta al activar [w32tm registro](#W32Logging). 

#### <a name="using-group-policy"></a>Uso de la directiva de grupo
Puede usar Directiva de grupo para lograr precisión más estricta, por ejemplo, asignar a clientes a servidores NTP específicos de uso o a control cómo inferiores del sistema operativo se configura cuando se virtualiza.  
A continuación encontrará una lista de posibles escenarios y configuración de directiva de grupo correspondiente:

**Virtualizar dominios** : con el fin de controlar los controladores de dominio virtualizados en Windows 2012R2 poder sincronizar la hora con su dominio, en lugar de con el host de Hyper-V, puede deshabilitar esta entrada del registro.   Para que el PDC, no desea deshabilitar la entrada como el host de Hyper-V proporcionará el origen de la hora más estable.  La entrada del registro requiere que se reinicie el servicio w32time después de cambiarlo.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Las cargas confidenciales precisión** : para cargas de trabajo confidenciales de tiempo precisión, puede configurar grupos de equipos para establecer los servidores NTP y cualquier relacionadas con la configuración de hora, como frecuencia de sondeo y el reloj de la actualización.  Esto normalmente está controlada por el dominio, pero para tener más control podría dirigirse máquinas específicas para que apunte directamente al reloj maestro.

Configuración de directiva de grupo|   Valor nuevo|
----- | ----- |
NtpServer|  ClockMasterName,0x8|
MinPollInterval|    6 – 64 segundos|
MaxPollInterval|    6|
UpdateInterval| 100: una vez por segundo|
EventLogFlags|  3 – todos los registros de tiempo especiales|

> [!NOTE]
> La configuración de NtpServer y EventLogFlags se encuentra en el sistema tiempo Service\Time proveedores con la configuración de la configuración de cliente de NTP de Windows.  Las otras 3 se encuentran en el servicio de hora del sistema mediante la configuración Global.

**Remoto precisión confidenciales cargas remoto** : para los sistemas en dominios de rama para la instancia por menor y la industria de crédito de pago (PCI), Windows usa la información del sitio actual y para encontrar un controlador de dominio local, a menos que haya un manual NTP ubicador de DC de origen de hora configurado.  Este entorno requiere 1 segundo de precisión, que utiliza la convergencia más rápido a la hora correcta.  Esta opción permite que el servicio w32time mover hacia atrás el reloj.  Si esto es aceptable y cumpla sus requisitos, puede crear la directiva siguiente.   Al igual que con cualquier entorno, asegura a prueba y la línea de base de la red. 

Configuración de directiva de grupo|   Valor nuevo|
----- | ----- |
MaxAllowedPhaseOffset|  1, si más que en el segundo, establezca reloj a la hora correcta.|

La configuración de MaxAllowedPhaseOffset se encuentra en el servicio de hora del sistema mediante la configuración Global.

> [!NOTE]
> Para obtener más información sobre la directiva de grupo y las entradas relacionadas, consulte [herramientas del servicio de hora de Windows](windows-time-service-tools-and-settings.md) y configuración del artículo en TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Consideraciones de Azure e IaaS de Windows

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina Virtual de Azure: Active Directory Domain Services
Si la VM de Azure que ejecuta Active Directory Domain Services forma parte de una existente en el entorno local bosques de Active Directory, a continuación, TimeSync(VMIC), debe deshabilitarse. Esto es para permitir que todos los controladores de dominio en el bosque, físico y virtual, use una jerarquía de sincronización de tiempo único. Consulte las notas del producto de prácticas recomendadas ["ejecutando dominio controladores en Hyper-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina Virtual de Azure: equipo unido al dominio
Si va a hospedar una máquina que está unido a un bosque de Active Directory existente, virtuales o físicas, el procedimiento recomendado es deshabilitar TimeSync para el invitado y garantizar que W32Time está configurado para sincronizar con su controlador de dominio a través de la configuración de tiempo para Tipo = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina Virtual de Azure: Máquina de grupo de trabajo independiente
Si la máquina virtual de Azure no está unida a un dominio ni es un controlador de dominio, la recomendación es mantener la configuración predeterminada del tiempo y tener la máquina virtual se sincronice con el host.

## <a name="windows-application-requiring-accurate-time"></a>Aplicación de Windows que requieren tiempo exacto
### <a name="time-stamp-api"></a>API de marca de tiempo
Los programas que requieren la mayor precisión con respecto a UTC y no el paso del tiempo, deben usar el [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Esto garantiza que la aplicación obtiene la hora del sistema, que está condicionada por el servicio de hora de Windows.

### <a name="udp-performance"></a>Rendimiento de UDP
Si tiene una aplicación que utiliza la comunicación UDP para las transacciones e importante para minimizar la latencia, hay algunos relacionados con las entradas del registro que puede usar para configurar un intervalo de puertos que desea excluir de la base de motor de filtrado de puertos.  Esto mejorará la latencia y aumentar el rendimiento.  Sin embargo, los cambios en el registro deben limitarse a los administradores experimentados.  Además, esta solución alternativa excluye los puertos que va a proteger el servidor de seguridad.  Consulte la referencia de artículo a continuación para obtener más información.

Para Windows Server 2012 y Windows Server 2008, deberá instalar primero una revisión.  Puede hacer referencia a este artículo de KB: [Pérdida de datagramas al ejecutar una aplicación de recepción de multidifusión en Windows 8 y Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Actualizar los controladores de red
Algunos proveedores de red tienen actualizaciones de controladores que mejorar el rendimiento con respecto a la latencia de controlador y el almacenamiento en búfer los paquetes UDP.  Póngase en contacto con su proveedor de red para ver si hay actualizaciones para ayudar con el rendimiento de UDP.

## <a name="logging-for-auditing-purposes"></a>Registro para fines de auditoría
Para cumplir las normativas de seguimiento de tiempo manualmente puede archivar los registros de w32tm, registros de eventos y obtener información del monitor de rendimiento.  Más adelante, la información archivada puede utilizarse para avalar el cumplimiento en un momento determinado en el pasado.  Los factores siguientes se utilizan para indicar la precisión.


1. Precisión mediante el contador del monitor de rendimiento calcula el desplazamiento de tiempo de reloj.  Esto muestra el reloj con el de la precisión deseada.
2.  Origen de reloj buscando "Respuesta del mismo nivel de" en los registros de w32tm.   Siguiendo el texto del mensaje es la dirección IP o VMIC, que describe el origen de hora y la siguiente cadena de los relojes de referencia para validar.
3.  Usar los registros de w32tm para validar que el estado de condición de reloj "ClockDispl disciplina: \*SESGAR\*tiempo\*"se producen.  Esto indica que w32tm está activo en el momento.

### <a name="event-logging"></a>Registro de eventos
Para obtener la historia completa, también necesitará la información de registro de eventos.  Al recopilar el registro de eventos del sistema y filtrado en el servidor de horario, Microsoft-Windows-Kernel-Boot, Microsoft-Windows-Kernel-General, es posible que pueda detectar si hay otras influencias que han cambiado la hora, por ejemplo, terceras partes.  Estos registros podrían ser necesarios para descartar las interferencias externas.  Directiva de grupo puede afectar a los registros de eventos se escriben en el registro.  Para obtener más información, vea la sección anterior en mediante la directiva de grupo.

### <a name="W32Logging"></a>El registro de depuración W32Time
Para habilitar w32tm con fines de auditoría, el siguiente comando habilita el registro que muestra las actualizaciones periódicas del reloj e indica el reloj de origen.  Reinicie el servicio para habilitar el registro nuevo.  

Para obtener más información, consulte [cómo activar el registro de depuración en el servicio de hora de Windows](https://support.microsoft.com/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Performance Monitor
El servicio de hora de Windows de Windows Server 2016 expone contadores de rendimiento que pueden usarse para recopilar el registro de auditoría.  Estos se pueden registrar local o remotamente.  Puede registrar los contadores de retraso de ida y vuelta y desplazamiento de tiempo del equipo.  
Y, al igual que cualquier contador de rendimiento, puede supervisarlos de forma remota y crear alertas con System Center Operations Manager.  Por ejemplo, puede utilizar una alerta con alarma cuando el tiempo de desplazamiento se desplaza desde la precisión deseada.  El [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tiene más información.

### <a name="windows-traceability-example"></a>Ejemplo de seguimiento de Windows
Desde los archivos de registro de w32tm desea validar dos piezas de información.  La primera es una indicación de que el archivo de registro actualmente es el reloj de condición.  Esto demuestre que el reloj se está condicionado por el servicio de hora de Windows en el momento de disputa.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

La cuestión principal es que vea mensajes precedidos por disciplina ClockDispln que es la prueba w32time está interactuando con el reloj del sistema.
 
A continuación deberá buscar el último informe en el registro antes de la hora de disputa que notifica el equipo de origen que se usa como el reloj de referencia.  Podría tratarse de una dirección IP, nombre de equipo o el proveedor VMIC, que indica que se está sincronizando con el Host de Hyper-V.  El ejemplo siguiente proporciona una dirección IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Ahora que ha validado el primer sistema de la cadena de hora de referencia, debe investigar el archivo de registro en el origen de hora de referencia y repita los mismos pasos.  Este proceso continúa hasta llegar a un reloj físico, como GPS o un origen de hora conocidos como NIST.  Si el reloj de la referencia es hardware GPS, registros de la fabricación también podrían ser necesarios.

## <a name="network-considerations"></a>Consideraciones de red
Los algoritmos de protocolo NTP tienen una dependencia en la simetría de la red.  Como el aumento del número de saltos de red, aumenta la probabilidad de asimetría.  No existe, es difícil predecir qué tipos de precisión que verá en sus entornos concretos. 

Monitor de rendimiento y los nuevos contadores de tiempo de Windows en Windows Server 2016 pueden usarse para evaluar la exactitud de los entornos y crear líneas base. Además, puede realizar la solución de problemas para determinar el desplazamiento actual de cualquier máquina en la red.

Hay dos normas generales para la hora exacta a través de la red.  PTP ([protocolo de tiempo de precisión - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tiene requisitos más estrictos en infraestructura de red, pero a menudo pueden proporcionar precisión microsegundo secundarias.  NTP ([protocolo de tiempo de red – RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona en una mayor variedad de redes y entornos, lo que hace más fácil de administrar. 

Windows admite NTP Simple (RFC2030) de forma predeterminada para máquinas combinadas que no sea de dominio.  Para equipos unidos al dominio, usamos un seguro NTP llamado [MS SNTP](https://msdn.microsoft.com/library/cc246877.aspx), que aprovecha los secretos de dominio negociada que proporcionan una ventaja de administración a través de NTP autenticado se describe en RFC1305 y RFC5905.   

Tanto el dominio y que no sea de dominio unido a protocolos requiere el puerto UDP 123.  Para obtener más información sobre los procedimientos recomendados NTP, consulte [red tiempo protocolo mejores prácticas IETF borrador actual](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Reloj de Hardware confiable (RTC)
Windows realiza no en tiempo de paso, a menos que se superan ciertos límites, pero en su lugar disciplinas el reloj.  Esto significa que w32tm ajusta la frecuencia del reloj a intervalos regulares, con la frecuencia de actualización de reloj establecer que el valor predeterminado es una vez por segundo con Windows Server 2016.  Si el reloj está detrás, acelera la frecuencia y si es con antelación, ralentiza la frecuencia.  Sin embargo, durante ese tiempo entre los ajustes de frecuencia de reloj, el reloj de hardware está en el control.  Si hay un problema con el firmware o el reloj de hardware, la hora de la máquina puede ser menos precisa.

Este es otro motivo que necesita para probar y línea base en su entorno.  Si el contador de rendimiento "Calcula el desplazamiento de tiempo" no se estabiliza en la precisión de destino, es posible que desee comprobar que el firmware está actualizado.  Como otra prueba, puede ver si el hardware duplicados reproducir el mismo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solución de problemas de NTP y la precisión de tiempo
Puede usar la Descubra la sección de jerarquía anterior para conocer el origen de la hora inexacta.  Examinar la diferencia horaria, encontrar el punto de la jerarquía donde tiempo difiere el máximo provecho de su origen NTP.  Una vez que comprenda la jerarquía, desea probar y comprender por qué no recibe la hora exacta ese origen de tiempo determinado.  

Centrarse en el sistema con tiempo divergente, puede usar estas herramientas siguientes para recopilar más información para ayudarle a determinar el problema y encontrar una solución.  La referencia UpstreamClockSource siguiente, es el reloj detectado mediante "/ de w32tm /config / Status".


- Registros de eventos del sistema
- Enable logging using: w32tm logs - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- clave del registro de w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Rastros de red local
- Contadores de rendimiento (desde el equipo local o la UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- PING UpstreamClockSource para comprender la latencia y número de saltos al origen
- Tracert UpstreamClockSource

Problema|    Síntomas|   Resolución|
----- | ----- | ----- |
Reloj TSC local no es estable.| Con Perfmon - equipo físico: sincronización reloj estable de reloj, pero sigue sin ver que cada 1 o 2 minutos de 100us varios. |   Actualizar el Firmware o validar un hardware diferente no muestra el mismo problema.|
Latencia de red|    w32tm stripchart muestra un RoundTripDelay de más de 10 ms.  Variación en el retraso hacer ruido tan grande como la mitad del tiempo de ida y vuelta, por ejemplo un retraso que solamente esté en una dirección.</br></br>UpstreamClockSource es múltiples saltos, tal y como indica el PING.  Período de vida debe ser cerca de 128.</br></br>Use Tracert para buscar la latencia en cada salto.    | Buscar un origen de reloj más cercano para el tiempo.  Una solución consiste en instalar un reloj de origen en el mismo segmento o elija manualmente al reloj de origen que esté más cercano geográficamente.  Para un escenario de dominio, agregar una máquina con el rol GTimeServ. |  
No se puede alcanzar el origen NTP de forma confiable|    W32tm /stripchart intermitentemente devuelve "Agotado el tiempo de solicitud"    |Origen NTP no está respondiendo|
Origen NTP no está respondiendo|    Compruebe los contadores Perfmon para las respuestas salientes de recuento de origen de clientes NTP, solicitudes de entrada de servidor NTP, NTP Server y determinar su uso en comparación con sus líneas base.|    Mediante los contadores de rendimiento de servidor, determine si se ha modificado para hacer referencia a las líneas base de las carga.</br></br>¿Hay red problemas de congestión?|
Controlador de dominio no utiliza el reloj más preciso|    Cambios en la topología o un reloj de tiempo maestra se ha agregado recientemente.|   w32tm /resync /rediscover|
Se deriva relojes del cliente| Servicio de hora de evento 36 en el registro de eventos del sistema o de texto en el archivo de registro que describe: Contador "recuento de origen de hora de cliente de NTP" va de 1 a 0|El origen ascendente de la solución de problemas y comprender si se está ejecutando con problemas de rendimiento.|

### <a name="baselining-time"></a>Tiempo de la línea de base
Línea de base es importante para que en primer lugar, puede comprender el rendimiento y la precisión de la red y comparar con la línea base en el futuro cuando se producen problemas.  Querrá previsto el PDC raíz o todos los equipos marcan con el GTIMESRV.  También le recomendamos el PDC en cada bosque de la línea de base.  Por último, elija cualquier crítico controladores de dominio o las máquinas que tienen características interesantes, como distancia o cargas elevadas y línea base esos.

También es útil para línea base Windows Server 2016 vs 2012 R2, sin embargo, solo tiene w32tm /stripchart como una herramienta que puede usar para comparar, puesto que Windows Server 2012 R2 no tiene contadores de rendimiento.  Debe elegir dos máquinas con las mismas características, o actualizar una máquina y comparar los resultados tras la actualización.  El anexo de mediciones de tiempo de Windows tiene más información sobre cómo hacer medidas detalladas entre 2016 y 2012.

Con todas los w32time contadores de rendimiento, recopilar datos de al menos una semana.  Esto asegurará que dispone de una referencia para tener en cuenta distintos en la red con el tiempo suficiente y suficiente de una ejecución para proporcionar la confianza de que la precisión de tiempo es estable.

### <a name="ntp-server-redundancy"></a>Redundancia de servidores NTP
Para la configuración de servidor NTP manual se puede usar con máquinas combinadas que no sea de dominio o controlador de dominio principal, tener más de un servidor es una medida de buena redundancia en el caso de disponibilidad.  También puede dar una mayor precisión, suponiendo que todos los orígenes son precisos y estable.  Sin embargo, si la topología no está bien diseñada, o los orígenes de hora no son estables, la precisión resultante podría ser peor, por lo que se recomienda con precaución.  El límite de tiempo compatible manualmente puede hacer referencia a servidores w32time es 10. 

## <a name="leap-seconds"></a>Segundos intercalares
Período de rotación de la tierra varía con el tiempo, deber climáticos y geológicos eventos. Normalmente, la variación es aproximadamente un segundo cada dos años. Cada vez que la variación de hora atómica se hace demasiado grande, una corrección de un segundo (arriba o abajo) se inserta, se llama a un segundo intercalar. Esto se hace de manera que la diferencia nunca supere el 0,9 segundos. Esta corrección se anuncia seis meses por delante de la corrección real. Antes de Windows Server 2016, el servicio de hora de Microsoft no era consciente de los segundos intercalares, pero se basaba en el servicio de hora externo que se encargue de esto. Con la precisión de tiempo mayor de Windows Server 2016, Microsoft está trabajando en una solución más adecuada para el problema del segundo intercalar.

## <a name="secure-time-seeding"></a>Proteger la propagación de tiempo
W32Time en Server 2016 incluye la característica de proteger la propagación de tiempo. Esta característica determina el tiempo aproximado actual de las conexiones salientes de SSL.  Este valor de hora se usa para supervisar el reloj del sistema local y corrija los errores en brutos. Puede leer más acerca de la característica de [esta entrada de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  En implementaciones con un orígenes de hora confiable y bien supervisadas máquinas que incluyen la supervisión para los desplazamientos de tiempo, decide no utilizar la característica de proteger la propagación de tiempo y se basan en la infraestructura existente en su lugar. 

Puede deshabilitar la característica con estos pasos:

1.  Establecer el valor de configuración del registro UtilizeSSLTimeData en 0 en una máquina específica:

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Si no puede reiniciar el equipo inmediatamente debido a alguna razón, se puede notificar al servicio W32time acerca de la actualización de configuración. Esto detiene la supervisión en tiempo y cumplimiento basado en datos de tiempo recopilados de las conexiones SSL. 

    W32tm.exe /config /update

3.  Reiniciar el equipo hace que la configuración efectivo inmediatamente y también hace que se va a dejar de recopilar los datos de tiempo de las conexiones SSL.  La última parte tiene una sobrecarga muy pequeña y no debe ser un problema de rendimiento.

4.  Para aplicar esta configuración en un dominio completo, establezca el valor de UtilizeSSLTimeData en configuración de directiva de grupo W32time en 0 y la configuración de publicación.  Cuando se selecciona la configuración de un cliente de directiva de grupo, se notifica al servicio W32time y detendrá la supervisión en tiempo y el cumplimiento con los datos en tiempo de SSL. Cuando se reinicia cada máquina, se detendrá la recopilación de datos de tiempo SSL. Si el dominio tiene portátiles delgados tabletas o portátiles y otros dispositivos, desea excluir dichas máquinas de este cambio de directiva. Estos dispositivos enfrentarán finalmente purga de la batería y necesita la propagación de tiempo seguro con características para arrancar su tiempo.


