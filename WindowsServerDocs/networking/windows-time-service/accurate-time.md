---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora exacta de 2016 de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Hora de Windows Server 2016 precisa

>Se aplica a: Windows Server 2016

Precisión de la sincronización de hora en Windows Server 2016 se ha mejorado considerablemente, mientras mantiene completa la NTP compatibilidad con versiones anteriores de Windows. En condiciones operativas razonables puede mantener un 1 ms precisión con respecto a la hora UTC o superior para los miembros del dominio de Windows Server 2016 y actualización de aniversario de Windows 10.

El servicio hora de Windows es un componente que use un modelo de complemento para los proveedores de sincronización de hora de cliente y servidor.  Hay dos proveedores de cliente integrado en Windows, y hay disponibles complementos de terceros. Usa un proveedor de [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx) para sincronizar la hora del sistema local a un servidor de referencia compatible con NTP o MS-NTP. El otro proveedor es para Hyper-V y las máquinas virtuales (VM) el host de Hyper-V sincroniza.  Cuando existen varios proveedores, Windows selecciona el mejor proveedor con el nivel de la capa en primer lugar, seguido por el retraso de raíz, dispersión de raíz y la hora de desplazamiento.

>[!NOTE]
>Para ver una introducción rápida de servicio hora de Windows, echa un vistazo a este [descripción general de vídeo ](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
En este tema, veremos... estos temas como a la habilitación de tiempo preciso en que se relacionan: 

- Mejoras
- Medidas
- Procedimientos recomendados

>[!IMPORTANT]
> Un apéndice al que hace referencia el artículo de tiempo preciso de 2016 de Windows puede descargarse [aquí](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Este documento proporciona más detalles sobre nuestras metodologías de prueba y medición.



> [!NOTE] 
> El modelo de complemento de proveedor de tiempo de windows es [documentado en TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).
<!-- -->





## <a name="domain-hierarchy"></a>Jerarquía de dominios
Configuraciones de dominio e independiente funcionan de manera diferente.

- Los miembros del dominio usan un protocolo seguro de NTP, que usa autenticación para garantizar la seguridad y la autenticidad de la referencia de tiempo.  Los miembros del dominio que se sincronizan con un reloj principal determinado por la jerarquía de dominios y un sistema de puntuación.  En un dominio, hay un nivel jerárquico de stratums de tiempo, mediante el cual cada controlador de dominio apunta a un elemento primario DC con una capa de tiempo más precisa.  Resuelve la jerarquía para el PDC o un controlador de dominio en el bosque de raíz o un controlador de dominio con la marca de dominio GTIMESERV, que indica un servidor horario buena para el dominio.  Consulta la [especificar un Local confiable tiempo servicio usando GTIMESERV](#GTIMESERV) siguiente sección.

- Equipos independientes están configurados para usar time.windows.com de manera predeterminada.  Este nombre se resuelve el ISP, que debe apuntar a una propiedad de recursos de Microsoft.  Al igual que todas las referencias de tiempo encuentra de forma remota, las interrupciones de red, puede evitar la sincronización.  Carga de tráfico de red y rutas de acceso de red asimétricas pueden reducir la precisión de la sincronización de hora.  Para una precisión de 1 ms, no puede depender de los recursos de una hora remoto.

Como invitados de Hyper-V tienen al menos dos proveedores de hora de Windows para elegir, la hora de host y NTP, es posible que veas distintos comportamientos con el dominio o independiente cuando se ejecuta como invitado.

> [!NOTE] 
> Para obtener más información sobre la jerarquía de dominio y sistema de puntuación, consulta el ["¿Qué es servicio hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Entrada de blog.

> [!NOTE]
> Capa es un concepto que se usa en los NTP Hyper-V proveedores y y su valor indica la ubicación de los relojes de la jerarquía.  1 de capa está reservada para el reloj del nivel superior y capa 0 está reservada para el hardware que se da por hecho que sean precisos y tiene poco o ningún retraso asociada a ella.  Capa 2 hablar con los servidores de capa 1, 3 de capa a capa 2 y así sucesivamente.  Mientras una capa inferior suele indica un reloj más preciso, es posible buscar discrepancias.  Además, W32time solo acepta tiempo desde el 15 de capa o a continuación.  Para ver la capa de un cliente, usa */Status de /query w32tm*.

## <a name="critical-factors-for-accurate-time"></a>Factores fundamentales por vez precisa
En cada caso hora exacta, hay tres factores fundamentales:

1. **Reloj de origen sólido** -el reloj de origen en tus necesidades de dominio precisos y estables. Por lo general, esto significa instalar un dispositivo GPS o al apuntar a un origen de capa 1 #3 de teniendo en cuenta. La analogía llega, si tienes dos barcos de agua y puedes intenta medir la altitud de uno en comparación con el otro, la precisión es mejor si el barco de origen es muy estables y no mover. Lo mismo ocurre por vez y, si el reloj de origen no es estable, a continuación, toda la cadena de relojes sincronizados es afectada y amplía en cada fase. También debe ser accesible porque las interrupciones en la conexión interferirá en la sincronización de hora. Y, por último, debe ser seguro. Si mantiene la hora de referencia no está correctamente u operado por una de las partes potencialmente malintencionada, podría exponer tu dominio a los ataques en función de tiempo.
2. **Reloj de cliente estable** -un reloj de cliente estable garantiza que el desfase del oscilador natural es restringible.  NTP usa varios ejemplos de potencialmente varios servidores NTP condición y disciplina el reloj de equipos locales.  Paso no cambia el tiempo, pero en su lugar se ralentiza o acelera el reloj local que abordar la precisión rápidamente y mantienen preciso entre solicitudes NTP.  Sin embargo, si oscilador del reloj del equipo cliente no es estable, pueden producirse más fluctuaciones entre ajustes y los algoritmos de que Windows usa para el reloj de la condición no funcionan correctamente.  En algunos casos, las actualizaciones de firmware podrían ser necesarias para tiempo preciso.
3. **Comunicación NTP simétrico** -es fundamental que la conexión para la comunicación NTP es simétrica.  NTP utiliza cálculos para ajustar el tiempo que supone que la revisión de la red está simétrica.  Si la ruta de acceso del paquete NTP tarda ir al servidor tarda una cantidad diferente de tiempo para volver, se ve afectada la precisión.  Por ejemplo, puede cambiar la ruta de acceso debido a cambios en la topología de red o paquetes que se enrutan a través de los dispositivos que tienen velocidades de interfaz diferente.


Para los dispositivos con batería, móvil y portátiles, debes considerar estrategias diferentes.  Según nuestra recomendación, conservando el tiempo preciso requiere el reloj sea disciplinado una vez por segundo, que se relaciona con la frecuencia de actualización del reloj. Estas opciones de configuración consumirán más batería de lo esperado y pueden interferir con el ahorro de energía modos disponibles en Windows para estos dispositivos. Dispositivos con batería también tienen determinados modos de energía que Detén todas las aplicaciones se ejecuten, que interfiere con capacidad de W32time disciplina el reloj y mantener el momento preciso. Además, relojes de los dispositivos móviles no será muy precisos comenzar.  Condiciones ambientales ambiente afectar a la precisión de reloj y un dispositivo móvil puede pasar una condición ambiental a la siguiente que podría interferir con su capacidad para mantener la hora correctamente.  Por lo tanto, Microsoft recomienda configurar dispositivos portátiles de batería con tecnología con la configuración de alta precisión. 

## <a name="why-is-time-important"></a>¿Por qué es importante el tiempo?  


## <a name="windows-server-2016-improvements"></a>Mejoras de Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP y servicio hora de Windows
Windows Server 2016 ha mejorado los algoritmos que se usa para corregir el tiempo y condición el reloj local para sincronizar con UTC.  NTP usa 4 valores para calcular el desplazamiento de tiempo, en función de las marcas de tiempo del cliente de solicitud y respuesta y solicitud/respuesta del servidor.  Sin embargo, las redes son ruidosas y puede haber picos en los datos de NTP debido a la congestión de la red y otros factores que afectan a la latencia de red.  Algoritmos de 2016 de Windows Media un vistazo a este ruido con un número de diferentes técnicas que da como resultado un reloj preciso y estable.  Además, el origen también se usan para las referencias de tiempo precisa una API mejorada que se brinda una mejor resolución.  Con estas mejoras podemos lograr precisión de ms 1 con respecto a la hora UTC en un dominio.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 ha mejorado el servicio de Hyper-V TimeSync. Mejoras incluyen tiempo inicial más precisa en el inicio de la máquina virtual o restauración de máquina virtual y corrección de la latencia de interrupción para ver muestras que proporcionan a w32time.  Esta mejora nos permite permanecer con de 10µs del host con una RMS, (raíz al cuadrado, que indica la variación), de 50µs, incluso en un equipo con el 75% de carga.

> [!NOTE]
> Consulta este artículo en [arquitectura de Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx) para obtener más información.

> [!NOTE]
> Carga se creó usando comparativa prime95 con perfil equilibrada.

Además, el nivel de capa que notifica el Host para el huésped es más transparente.  Anteriormente el Host presenta una capa fijo de 2, independientemente de su precisión.  Con los cambios en Windows Server 2016, el host notifica una capa superior a la capa de host que da como resultado una mejor momento para invitados virtuales.  La capa de host se determina por w32time por medios normales en función de su tiempo de origen.  Windows 2016 invitados encontrará el reloj más preciso, en lugar de con el host predeterminado Unidos a un dominio.  Es por este motivo que se recomienda deshabilitar manualmente la configuración de proveedor de la hora de Hyper-V para equipos que participan en un dominio en Windows 2012R2 y, a continuación.

### <a name="monitoring"></a>Supervisión
Se han agregado los contadores de rendimiento.  Estos le permiten línea base, supervisan y solucionar problemas de precisión de tiempo.  Estos contadores incluyen:

Contador|Descripción|
----- | ----- |
Calcula el desplazamiento de tiempo|   La hora absoluta offset entre el reloj del sistema y el origen de la hora elegida, tal como calculado por el servicio W32Time en microsegundos. Cuando hay una nueva muestra válida, se actualiza el tiempo calculado con el desplazamiento de tiempo que se indica en la muestra. Este es el desplazamiento de tiempo real de la hora local. W32Time inicia utilizando este desplazamiento de corrección de reloj y actualiza el tiempo calculado entre los ejemplos con el desplazamiento de tiempo restante que debe aplicarse a la hora local. Precisión de reloj puede controlarse con el contador de rendimiento con un intervalo de sondeo bajo (p. ej.: 256 segundos o menos) y busca el valor de contador sea menor que el límite de precisión de reloj deseado.|
Ajuste de la frecuencia de reloj| El ajuste de frecuencia de reloj absoluta realizado en el reloj del sistema local por W32Time en partes por mil millones. Este contador ayuda a visualizar las acciones realizadas por W32time.|
Retraso de ida y vuelta NTP|    Retraso de ida y vuelta experimentado por el cliente de NTP en recibir una respuesta del servidor en microsegundos más reciente. Este es el tiempo transcurrido en el cliente NTP entre transmitir una solicitud al servidor NTP y recibir una respuesta válida desde el servidor. Este contador ayuda caractericen los retrasos experimentados por el cliente NTP. Más grande o variable de ida y vuelta agregar ruido a los cálculos de tiempo NTP, que pueden afectar a su vez a la precisión de sincronización de hora mediante NTP.|
Recuento de origen del cliente NTP|    Número activo de tiempo de NTP orígenes siendo usada por el cliente NTP. Este es un recuento de activos, distintas direcciones IP de los servidores de tiempo que responden a las solicitudes de este cliente. Este número puede ser mayor o menor que el configurado sistemas del mismo nivel, según la resolución DNS de nombres del mismo nivel y capacidad de alcance actual.|
Servidor NTP solicitudes entrantes|   Número de solicitudes recibidos por el servidor NTP (solicitudes por segundo).|
Respuestas NTP servidor saliente|  Número de solicitudes contestadas por servidor NTP (respuestas/s).|

Los primeros 3 contadores de destino escenarios para solucionar problemas de precisión.  La solución de problemas de precisión de tiempo y NTP sección a continuación, en [los procedimientos recomendados](#BestPractices), tiene más de detalle.
Los últimos 3 contadores analizamos escenarios de servidor NTP y son útiles cuando determinar la carga y el nivel de referencia el rendimiento actual.

### <a name="configuration-updates-per-environment"></a>Actualizaciones de configuración por el entorno
El siguiente describe los cambios en la configuración predeterminada entre 2016 de Windows y las versiones anteriores para cada rol.  La configuración de Windows Server 2016 y Windows 10 aniversario Update(build 14393), ahora son únicos motivo por el que se muestran allí como columnas separadas. 

|Función|Configuración|Windows Server 2016|Windows 10, versión 1607|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Servidor independiente o Nano**||||
| |*Servidor horario*|time.windows.com|NA|time.windows.com|
| |*Frecuencia de sondeo*|64 - 1024 segundos|NA|Una vez por semana|
| |*Frecuencia de actualización de reloj*|Una vez por segundo|NA|Una vez por hora|
|**Cliente independiente**||||
| |*Servidor horario*|NA|time.windows.com|time.windows.com|
| |*Frecuencia de sondeo*|NA|Una vez al día|Una vez por semana|
| |*Frecuencia de actualización de reloj*|NA|Una vez al día|Una vez por semana|
|**Controlador de dominio**||||
| |*Servidor horario*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*Frecuencia de sondeo*|64-1024 segundos|NA|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|Una vez al día|NA|Una vez por semana|
|**Servidor miembro del dominio**||||
| |*Servidor horario*|CONTROLADOR DE DOMINIO|NA|CONTROLADOR DE DOMINIO|
| |*Frecuencia de sondeo*|64-1024 segundos|NA|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|Una vez por segundo|NA|Una vez cada cinco minutos|
|**Cliente de miembro de dominio**||||
| |*Servidor horario*|NA|CONTROLADOR DE DOMINIO|CONTROLADOR DE DOMINIO|
| |*Frecuencia de sondeo*|NA|1204 - 32768 segundos|1024 - 32768 segundos|
| |*Frecuencia de actualización de reloj*|NA|Una vez cada cinco minutos|Una vez cada cinco minutos|
|**Invitado Hyper-V**||||
| |*Servidor horario*|Elige la mejor opción en función de la capa del servidor Host y el tiempo|Elige la mejor opción en función de la capa del servidor Host y el tiempo|Valores predeterminados de Host|
| |*Frecuencia de sondeo*|En función de la función anterior|En función de la función anterior|En función de la función anterior|
| |*Frecuencia de actualización de reloj*|En función de la función anterior|En función de la función anterior|En función de la función anterior|

>[!NOTE]
>Para Linux en Hyper-V, consulta el [Linux que permite usar el tiempo de Host de Hyper-V](#AllowingLinux) siguiente sección.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto de sondeo mayor y la frecuencia de actualización de reloj
Con el fin de proporcionar una hora más exacta, aumentan los valores predeterminados de sondeo frecuencias y actualizaciones de reloj que nos permitirá realizar pequeños ajustes con más frecuencia.  Esto hará que la mayor tráfico UDP/NTP, sin embargo, estos paquetes son pequeños por lo que debería haber poco o ningún impacto sobre los enlaces de banda ancha. La ventaja, sin embargo, es que debe ser mejor en una amplia gama de entornos de hardware y el tiempo.

Para los dispositivos de baterías, aumentar la frecuencia de sondeo puede causar problemas.  Dispositivos de la batería no almacenan el tiempo mientras desactivado.  Cuando se reanudan, puede que requiere correcciones frecuentes en el reloj.  Aumentar la frecuencia de sondeo hará que el reloj se vuelva inestable y también se puede usar más energía.  Microsoft recomienda que no cambia la configuración predeterminada del cliente.

Controladores de dominio deben afectar mínimamente incluso con el efecto multiplicado de las actualizaciones mayor de los clientes NTP en un dominio de AD.  NTP tiene un consumo de recursos mucho más pequeño en comparación con un impacto marginal y de otros protocolos.  Es más probable que llegar a los límites de otras funcionalidades de dominio antes de que se ven afectadas por la configuración de aumento de Windows Server 2016.  Active Directory usan NTP segura, que suele sincronizar con menos precisión que NTP simple de tiempo, pero ya hemos verificado que escalar a una capa de dos de los clientes fuera de PDC.

Como un plan conservador, debe reservarse 100 solicitudes NTP por segundo por núcleo.  Por ejemplo, un dominio que se compone de 4 controladores de dominio con 4 núcleos, podrás atender solicitudes NTP 1600 por segundo.  Si tienes 10 clientes de k configurados para sincronizar una vez cada 64 segundos, la hora y las solicitudes reciben uniformemente con el tiempo, verá 10.000/64 o alrededor de 160 solicitudes por segundo, se propaga entre todos los controladores de dominio.  Esto fácilmente entra dentro de nuestro 1600 NTP solicitudes por segundo en función de este ejemplo.  Estas son conservadoras recomendaciones de diseño y por supuesto tienen una gran dependencia en tu red, la velocidad de procesador y la carga, por lo tanto, como siempre previsto y probar en sus entornos.

También es importante tener en cuenta que si los controladores de dominio se ejecutan con una carga considerable de CPU, más del 40%, esto seguramente agregará ruido respuestas NTP y afectar a la precisión de tiempo en tu dominio.  De nuevo, deberás probar en tu entorno para comprender los resultados reales.

## <a name="time-accuracy-measurements"></a>Medidas de precisión de tiempo
### <a name="methodology"></a>Metodología
Para medir la precisión de tiempo para Windows Server 2016, usamos una variedad de herramientas, métodos y entornos.  Puedes usar estas técnicas para medir y ajustar el entorno y determinar si los resultados de la precisión cumplir con los requisitos. 

Nuestro reloj de origen del dominio consistía en dos servidores NTP de alta precisión con hardware GPS.  También hemos usado un equipo de prueba de referencia independientes para las mediciones, que también tenían instalado de otro fabricante de hardware GPS de alta precisión.  Para algunas de las pruebas, necesitarás un origen de reloj exacta y fiable para usar como referencia además de su origen de reloj de dominio.

Hemos usado los cuatro métodos diferentes para medir la precisión con físicos y máquinas virtuales. Varios métodos proporcionan medios independientes para validar los resultados.


1. Medir el reloj local, que está sujeta al w32tm, frente a nuestro equipo de prueba de referencia que tiene hardware GPS independiente.  
2.  Medida NTP ping desde el servidor NTP a los clientes mediante W32tm "stripchart"
3.  Medida NTP ping desde el cliente al servidor NTP mediante W32tm "stripchart"
4.  Medida Hyper-V resultados del host para el huésped con el contador de marca de tiempo (TSC).  Este contador se comparte entre las particiones y la hora del sistema en las dos particiones.  Calculamos la diferencia de la hora de host y el cliente en la máquina virtual.  A continuación, usamos el reloj TSC para interpolar la hora de host de invitado, dado que las mediciones no ocurrir al mismo tiempo.  Además, usamos el factor de reloj TSV retrasos y la latencia en la API.

W32tm está integrada, pero las otras herramientas que hemos usado durante la prueba están disponibles para el repositorio de Microsoft en GitHub como código abierto para su uso y pruebas.  En el repositorio de la WIKI de tiene más información acerca de cómo usar las herramientas para realizar mediciones.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Los resultados de prueba que se muestra a continuación son un subconjunto de las medidas que tomamos en uno de los entornos de prueba.  Ilustran la precisión que se mantiene en el inicio de la jerarquía de tiempo y el cliente de dominio secundario al final de la jerarquía de tiempo.  Esto se compara con los mismos equipos en una topología de 2012 en función de comparación.

### <a name="topology"></a>Topología
Para fines de comparación, probamos un 2012R2 de Windows Server y la topología en función de Windows Server 2016.  Ambas topologías constan de dos equipos host Hyper-V físicos que hacen referencia a una máquina Windows Server 2016 GPS reloj hardware instalado.  Cada host ejecuta 3 invitados de windows de unido a un dominio, que se organizan según la topología siguiente.  Las líneas representan la jerarquía de tiempo y el protocolo/transporte que se usa.

![Hora de Windows](media/Windows-2016-Accurate-Time/topology1.png)

![Hora de Windows](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Introducción a los resultados gráficos
Los siguientes dos gráficos representan la precisión de tiempo de dos miembros concretos de un dominio en función de la topología anterior.  Cada gráfico muestra tanto 2012R2 de Windows Server 2016 resultados superpuestos, que muestra las mejoras visualmente.  La precisión se mide desde con el equipo de invitado en comparación con el host.  Los datos gráficos representa un subconjunto de todo el conjunto de pruebas que hemos hecho y muestran el mejor caso y situaciones.  

![Hora de Windows](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Rendimiento del dominio raíz PDC
El PDC raíz se sincroniza con el host de Hyper-V (con VMIC) que es un Windows Server 2016 con hardware GPS que ha demostrado para ser precisos y estables.  Este es un requisito fundamental para una precisión de 1 ms, que se muestra como el área sombreado verde.

![Hora de Windows](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Rendimiento del cliente de dominio secundarios
El cliente del dominio secundarios está conectado a un dominio de PDC secundarios que se comunica con el PDC raíz.  Tiempo también es dentro de la 1 ms requisito.

![Hora de Windows](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Prueba de larga distancia
El siguiente gráfico compara los saltos de red virtual 1a 6 saltos física de red con Windows Server 2016.  Dos gráficos se superponen entre sí con transparencia para mostrar datos superpuestos.  Saltos de red aumenta significan mayor latencia y desviaciones de tiempo más grande.  El gráfico está ampliada y así el 1 ms límites, representados por el área verde, es más grande.  Como puedes ver, el tiempo sigue estando dentro de 1 ms con varios saltos.  Negativamente desplazamiento, que muestra una asimetría de red.  Por supuesto, cada red es diferente, y las mediciones dependen de una gran variedad de factores ambientales.

![Hora de Windows](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Los procedimientos recomendados para la sincronización de hora precisa
### <a name="solid-source-clock"></a>Reloj de origen sólido
Solo es tan buena como el reloj de origen con que se sincroniza uno de los equipos.  Para lograr 1 ms de precisión, necesitarás hardware GPS o un dispositivo de tiempo en la red hace referencia como el reloj de un origen principal.  Con el valor predeterminado de time.windows.com, no puede proporcionar un origen de hora local y estable.  Además, mientras sacas más lejos el reloj de origen, la red afecta a la precisión.  Tener un reloj de maestro de origen de datos de cada centro es necesario para la mejor precisión.

### <a name="hardware-gps-options"></a>Opciones de GPS de hardware
Existen varias soluciones de hardware que pueden ofrecer una hora precisa.  En general, las soluciones de hoy en día se basan en antenas GPS.  También hay radio y soluciones de módem con líneas dedicadas.  Se acople a la red como un dispositivo o se conecta a un equipo, por ejemplo Windows a través de un dispositivo USB o PCIe.  Opciones distintas ofrecerá distintos niveles de precisión y, como siempre, los resultados dependen del entorno.  Las variables que afectan a la precisión incluyen GPS disponibilidad, estabilidad de la red y carga y Hardware del equipo.  Estos son los factores importantes al elegir un reloj de origen, como se indicó, es un requisito para tiempo preciso y estable.

### <a name="domain-and-synchronizing-time"></a>Dominio y la sincronización de hora
Los miembros del dominio usan la jerarquía de dominios para determinar qué equipo usan como origen para sincronizar la hora.  Cada miembro de dominio encontrará otro equipo para sincronizar con y guardarla como origen de reloj de TI.  Cada tipo de miembro de dominio sigue un conjunto de reglas diferentes para poder encontrar un origen de reloj para la sincronización de hora.  El PDC en la raíz del bosque es el origen de reloj de forma predeterminada para todos los dominios.  A continuación aparecen diferentes funciones y la descripción de alto nivel de modo encuentran un origen:


- **Controlador de dominio con la función de PDC** : este equipo es el origen en tiempo de autorización para un dominio. Se tendrá el tiempo más preciso disponible en el dominio y debes sincronizar con un controlador de dominio en el dominio principal, excepto en casos donde [GTIMESERV](#GTIMESERV) función está habilitada. 
- **Otros controladores de dominio** : esta máquina actuará como una fuente de tiempo para los clientes y servidores miembro del dominio. Un controlador de dominio puede sincronizar con el PDC de su propio dominio o cualquier controlador de dominio de su dominio principal.
- **Los servidores miembro o clientes** : esta máquina se puede sincronizar con cualquier controlador de dominio o el PDC de su propio dominio, o un controlador de dominio o PDC en el dominio principal.

En función de los candidatos disponibles, se usa un sistema de puntuación para buscar el origen de tiempo recomendado.  Este sistema tiene en cuenta la confiabilidad de la fuente de tiempo y su ubicación relativa.  Esto ocurre una vez cuando el tiempo se inició el servicio.  Si es necesario tener un control más preciso de cómo se sincroniza tiempo, puedes agregar servidores buen momento en ubicaciones específicas o agregar redundancia.  Consulta la [especificar un Local confiable tiempo servicio usando GTIMESERV](#GTIMESERV) sección para obtener más información.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Entornos de sistema operativo mixto (Win2012R2 y Win2008R2)
Mientras un entorno de dominio de Windows Server 2016 puro es necesario para la mejor precisión, aún hay ventajas en un entorno mixto.  Implementar Windows Server 2016 Hyper-V en un dominio de Windows 2012 se beneficiarán a los invitados debido a las mejoras que hemos mencionado anteriormente, pero solo si los invitados también son Windows Server 2016.  Windows Server 2016 PDC, podrá ofrecer más precisa debido a los algoritmos mejorados será una fuente más estable.  Reemplazar el PDC podría no ser una opción, en su lugar, puede agregar un controlador de dominio de Windows Server 2016 con la [GTIMESERV](#GTIMESERV) álbum conjunto lo que significa que una actualización en la precisión de tu dominio.  Un controlador de dominio de Windows Server 2016 puede ofrecer una mejor a los clientes de tiempo dirección descendente, pero solo es tan buena como el tiempo de NTP de origen.

También, como se indicó anteriormente, la frecuencia de sondeo y la actualización de reloj se han modificado con Windows Server 2016.  Se pueden cambiar manualmente para los controladores de dominio de nivel inferior o aplicados a través de la directiva de grupo.  Si bien no hemos probado estas configuraciones, deben se comporte correctamente en Win2008R2 y Win2012R2 y entregar algunas ventajas.

Versiones antes de Windows Server 2016 tenía una varios problemas conservando el tiempo preciso mantener que resultó en el desplazamiento de tiempo de sistema inmediatamente después de que se realizó un ajuste.  Por este motivo, obtener ejemplos de tiempo de un origen NTP precisa con frecuencia y acondicionamiento el reloj local con los datos conduce a desfase menor en sus relojes de sistema en el período de muestreo internos, lo que mejor momento mantener en versiones del sistema operativo de nivel inferior. La mejor precisión observada fue aproximadamente 5 ms cuando un cliente de Windows Server 2012R2 NTP, configurado con la configuración de alta precisión, sincronizar su tiempo desde un servidor de Windows 2016 NTP precisa.

En algunos escenarios que implican a controladores de dominio de invitado, muestras de Hyper-V TimeSync pueden interrumpir la sincronización de hora de dominio.  Ya no debe ser un problema para los invitados Server 2016 ejecutándose en hosts 2016 Hyper-V Server.

Para deshabilitar el servicio de Hyper-V TimeSync de proporcionar ejemplos para w32time, Establece la siguiente clave del registro de invitado:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Permitir que Linux pueda usar la hora de Host de Hyper-V
Para clientes de Linux se ejecute en Hyper-V, los clientes normalmente están configurados para usar el daemon NTP para la sincronización de hora contra los servidores NTP.  Si la distribución de Linux admite el protocolo de la versión 4 TimeSync y el invitado de Linux tiene el servicio de integración TimeSync habilitado, se sincronizarán con el tiempo de host. Esto podría dar lugar a tiempo incoherente mantener si están habilitados ambos métodos.

Para sincronizar exclusivamente con el tiempo de host, se recomienda deshabilitar la sincronización de hora NTP, ya sea por:

- Deshabilitar a los servidores NTP en el archivo ntp.conf
- o deshabilitar el daemon NTP

En esta configuración, el parámetro de servidor horario es este host.  Su frecuencia de sondeo es de 5 segundos y la frecuencia de actualización de reloj también es de 5 segundos.

Para sincronizar exclusivamente sobre NTP, se recomienda deshabilitar el servicio de integración TimeSync en el invitado.

> [!NOTE]
> Nota: Compatibilidad con el tiempo preciso con Linux invitados requiere una característica que solo se admite en los últimos kernels Linux dirección ascendentes y no es algo que está ampliamente disponible en todos los distros Linux aún. Consulte [máquinas virtuales de Linux compatibles y FreeBSD para Hyper-V en Windows](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obtener más información acerca de las distribuciones de soporte técnico.

#### <a name="GTIMESERV"></a>Especificar un servicio de hora confiable Local con GTIMESERV
Puedes especificar uno o varios controladores de dominio como relojes de origen preciso mediante el uso de la GTIMESERV, buena servidor horario, indicadores.  Por ejemplo, se pueden marcar determinados controladores de dominio equipados con hardware GPS como un GTIMESERV.  Así se asegurará de que tu dominio hace referencia a un reloj en función del hardware GPS.

> [!NOTE]
> Para obtener más información acerca de las marcas de dominio puede encontrarse en la [documentación de protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV es otra relacionados dominio servicios de marca que indica si un equipo está actualmente autorizado, que puede cambiar si un controlador de dominio pierde la conexión.  Un controlador de dominio en este estado devolverá "Desconocido capa" cuando efectúa la consulta mediante NTP.  Después de intentarlo varias veces, el controlador de dominio registrará 36 de evento de servicio hora de sistema eventos.

Si quieres configurar un controlador de dominio como un GTIMESERV, esto se puede configurar manualmente mediante el siguiente comando.  En este caso el controlador de dominio está usando otra Machine (s) como el reloj principal.  Esto podría ser un dispositivo o equipo dedicado.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obtener más información, consulta [configurar el servicio hora de Windows](https://technet.microsoft.com/library/cc731191.aspx)

Si el controlador de dominio tiene instalado el hardware GPS, debes usar estos pasos para deshabilitar al cliente NTP y habilitar al servidor NTP.

Iniciar deshabilitando el cliente NTP y habilitar al servidor NTP con estos cambios de clave del registro.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

A continuación, reinicia el servicio hora de Windows

    net stop w32time && net start w32time

Por último, debes indicar que el equipo tiene un origen de hora confiable con.
   
    w32tm /config /reliable:yes /update

Para comprobar que los cambios se han realizado correctamente, puedes ejecutar los siguientes comandos que afectan a los resultados que se muestra a continuación. 

    w32tm /query /configuration

Valor|Configuración esperado|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Locales)|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (Local)|
Habilitado |1 (local)|
NtpClient|  (Locales)|

    w32tm /query /status /verbose

Valor|  Configuración esperado|
----- | ----- |
Capa|    1 (referencia principal - sincronizada mediante el reloj de radio)|
ReferenceId|    0x4C4F434C (nombre de origen: "LOCAL")|
Origen| Reloj CMOS local|
Desplazamiento de fase|   0.0000000s|
Rol de servidor|    576 (servicio de hora confiable)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 en plataformas virtuales de terceros 3
Cuando Windows se virtualiza, de manera predeterminada el hipervisor es responsable de proporcionar tiempo.  Pero Unidos a un dominio miembros deben ser está sincronizado con el controlador de dominio para Active Directory funcionar correctamente.  Es mejor deshabilitar la virtualización de cualquier tiempo entre el invitado y el host de cualquier plataforma virtual de terceros 3.

#### <a name="discovering-the-hierarchy"></a>Detección de la jerarquía
Dado que la cadena de la jerarquía de tiempo en el origen de reloj principal es dinámica en un dominio y negocia, tendrás que consultar el estado de un equipo determinado comprender es origen en tiempo y la cadena en el reloj del origen principal.  Esto puede ayudar a diagnosticar problemas de sincronización de hora.

Dado quieres solucionar un cliente específico; el primer paso es comprender su origen en tiempo con este comando w32tm.

    w32tm /query /status

Los resultados muestran el código fuente entre otras cosas.  El origen indica con el que sincronizan la hora en el dominio.  Este es el primer paso de esta jerarquía de tiempo de máquinas.
A continuación, usar entrada de origen desde arriba y usa el parámetro /StripChart para encontrar el siguiente origen en tiempo de la cadena.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

También resulta útil, el siguiente comando enumera cada controlador de dominio que se puede encontrar en el dominio especificado e imprime un resultado que te permite determinar a cada partner.  Este comando incluye equipos que se han configurado manualmente.
    
    w32tm /monitor /domain:my_domain

En la lista, puede hacer un seguimiento los resultados a través del dominio y comprender la jerarquía, así como el desplazamiento de tiempo en cada paso.  Buscando el punto donde el desplazamiento de tiempo empeora significativamente, puede determinar la raíz de la hora incorrecta.  Desde allí puedes intentar comprender por qué ese momento es incorrecto activando [w32tm registro](#W32Logging). 

#### <a name="using-group-policy"></a>Mediante la directiva de grupo
Puedes usar directivas de grupo para lograr precisión más estricta, por ejemplo, asignar a los clientes a servidores de uso específicos NTP o control cómo bajo nivel del sistema operativo se configura cuando virtualizado.  
A continuación te mostramos una lista de los escenarios posibles y la configuración de directiva de grupo relevantes:

**Virtualizados dominios** : para controlar virtualizan los controladores de dominio en 2012R2 de Windows para que sincroniza con su dominio de tiempo, en lugar de con el host de Hyper-V, puedes deshabilitar esta entrada del registro.   Para que el PDC, no quieres deshabilitar la entrada como el host de Hyper-V dará a conocer el origen en tiempo más estable.  La entrada del registro requiere reiniciar el servicio w32time cuando se cambia.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Se carga exactitud con información confidencial** : para cargas de trabajo con información confidencial de tiempo precisión, puedes configurar grupos de equipos para establecer los servidores NTP y cualquier relacionados con la configuración de tiempo, como la frecuencia de actualización de sondeo y reloj.  Normalmente esto se controla por el dominio, pero para tener más control pueden enviarse equipos específicos para señalar el reloj principal.

Configuración de directiva de grupo|   Nuevo valor|
----- | ----- |
NtpServer|  ClockMasterName, 0 x 8|
MinPollInterval|    6-64 segundos|
MaxPollInterval|    6|
UpdateInterval| 100: una vez por segundo|
EventLogFlags|  3: todos los registros de tiempo especiales|

> [!NOTE]
> La configuración NtpServer y EventLogFlags se encuentra en System\Widows tiempo Service\Time proveedores con la configuración de cliente de Windows NTP configurar.  Las otras 3 se encuentran en el servicio de la hora de sistema mediante las opciones de configuración Global.

**Remoto precisión con información confidencial cargas remoto** : sistemas de dominios de rama de instancia comercial y el sector de crédito de pago (PCI), Windows usa la información del sitio actual y ubicador de DC para encontrar un controlador de dominio local, a menos que haya un NTP manual tiempo origen configurado.  Este entorno requiere 1 segundo de precisión, que utiliza la convergencia más rápido a la hora correcta.  Esta opción permite al servicio w32time retroceder el reloj.  Si esto es aceptable y que cumpla con los requisitos, puedes crear la siguiente directiva.   Al igual que con cualquier entorno, garantiza a prueba y de línea base de la red. 

Configuración de directiva de grupo|   Nuevo valor|
----- | ----- |
MaxAllowedPhaseOffset|  Si más que en el segundo, 1, Establece reloj hora correcta.|

La configuración de MaxAllowedPhaseOffset se encuentra en el servicio de la hora de sistema mediante las opciones de configuración Global.

> [!NOTE]
> Para obtener más información sobre la directiva de grupo y entradas relacionadas, consulta [herramientas de servicios de Windows tiempo](windows-time-service-tools-and-settings.md) y el artículo de configuración en TechNet.

#### <a name="azure-and-windows-iaas-considerations"></a>Consideraciones de Azure y IaaS de Windows

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina Virtual Azure: Servicios de dominio de Active Directory
Si la VM de Azure que se ejecute los servicios de dominio de Active Directory forma parte de un local existente bosque de Active Directory, a continuación, TimeSync(VMIC), debe deshabilitarse. Esto es permitir que todos los controladores de dominio del bosque, físicas y virtuales usar una jerarquía de sincronización sola hora. Consulta las notas del producto mejor práctica ["ejecutando dominio controladores de Hyper-V"](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina Virtual Azure: Equipo unido al dominio
Si aloja una máquina que es el dominio unido a un bosque de Active Directory existentes, virtuales o físicos, el procedimiento recomendado consiste en deshabilitar TimeSync para el invitado asegurarse W32Time está configurada para sincronizar con su controlador de dominio a través de configuración de tiempo por tipo = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina Virtual Azure: Equipo de grupo de trabajo independiente
Si la máquina virtual de Azure no está unida a un dominio, así como tampoco es un controlador de dominio, la recomendación es mantener la configuración predeterminada del tiempo y usar para sincronizar con el host de la máquina virtual.

### <a name="windows-application-requiring-accurate-time"></a>Aplicación de Windows que requieren tiempo precisa
#### <a name="time-stamp-api"></a>API de marca de tiempo
Los programas que requieren la mayor precisión con respecto a la hora UTC y no en el paso del tiempo, deben usar el [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Esto garantiza que la aplicación obtiene la hora del sistema, que está sujeta por el servicio hora de Windows.

#### <a name="udp-performance"></a>Rendimiento UDP
Si tienes una aplicación que usa UDP comunicación para las transacciones e importante para reducir la latencia, hay algunas relacionadas con las entradas del registro que puedes usar para configurar un intervalo de puertos se excluirán del puerto el motor de filtro de base.  Esto mejorar tanto la latencia y aumentar el rendimiento.  Sin embargo, los cambios en el registro deberían limitarse a los administradores experimentados.  Además, esta solución alternativa excluye los puertos del firewall protejan.  Consulta la referencia de artículo, a continuación para obtener más información.

Para Windows Server 2012 y Windows Server 2008, tendrás que instalar una revisión en primer lugar.  Puedes hacer referencia a este artículo KB: [pérdida de datagramas cuando se ejecuta una aplicación de receptor de multidifusión en Windows 8 y en Windows Server 2012](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>Actualizar los controladores de red
Algunos proveedores de red tienen actualizaciones de controladores que mejorar el rendimiento con respecto a la latencia de controladores y paquetes UDP de búfer.  Ponte en contacto con su proveedor de red para ver si hay actualizaciones para ayudar con rendimiento UDP.

### <a name="logging-for-auditing-purposes"></a>Registro para fines de auditoría
Para cumplir con las normas de seguimiento de tiempo puede archivar manualmente w32tm registros, registros de eventos e información de supervisión del rendimiento.  Más adelante, la información archivada puede usarse para certificar cumplimiento en un momento específico en el pasado.  Los siguientes factores se usan para indicar la precisión.


1. Precisión de reloj con el contador de monitor de rendimiento de calcular el desplazamiento de tiempo.  Este muestra el reloj con la precisión deseada.
2.  Busca "Del mismo nivel respuesta" en los registros de w32tm el origen de reloj.   Siguiendo el texto del mensaje es la dirección IP o VMIC, que describe el origen en tiempo y el siguiente en la cadena de los relojes de referencia para validar.
3.  Estado de la condición mediante los registros de w32tm para validar que los relojes "ClockDispl disciplina: \*SKEW\*TIME\*" se producen.  Esto indica que w32tm está activo en el momento.

#### <a name="event-logging"></a>Registro de eventos
Para obtener la historia completa, también necesitarás información de registro de eventos.  Al recopilar el registro de eventos del sistema y filtrado en el servidor de horario, Microsoft-Windows-Kernel-arranque, Microsoft-Windows-Kernel-General, podrás descubrir si hay otros factores que influyen que ha cambiado el tiempo, por ejemplo, otros proveedores.  Estos registros podrían ser necesarios para descartar interferencia externa.  Directiva de grupo puede afectar a los registros de eventos se escriben en el registro.  Para obtener más información, consulte la sección anterior en mediante Directiva de grupo.

#### <a name="W32Logging"></a>Registro de depuración W32Time
Para habilitar w32tm con fines de auditoría, el siguiente comando habilita el registro que muestra las actualizaciones periódicas del reloj e indica el reloj de origen.  Reiniciar el servicio para habilitar el registro nuevo.  

Para obtener más información, consulta [cómo activar el registro de depuración en el servicio hora de Windows](https://support.microsoft.com/en-us/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>Monitor de rendimiento
El servicio hora de Windows de Windows Server 2016 expone los contadores de rendimiento que pueden usarse para recopilar el registro de auditoría.  Estos se pueden registrar de manera local o remota.  Puede grabar los contadores de retraso de ida y vuelta y el desplazamiento de tiempo del equipo.  
Y, al igual que cualquier contador de rendimiento, puedes supervisar de forma remota y crear alertas mediante System Center Operations Manager.  Por ejemplo, puedes usar una alerta con alarma cuando el desplazamiento de tiempo se desplaza desde la precisión deseada.  La [administración de System Center Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tiene más información.

#### <a name="windows-traceability-example"></a>Ejemplo de seguimiento de Windows
Desde los archivos de registro w32tm quieres validar dos fragmentos de información.  La primera es una indicación de que el archivo de registro está actualmente reloj de la condición.  Esto demostrar que el reloj se se preparó por el servicio hora de Windows en el momento de conflicto.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

El principal punto es que verá mensajes disciplina ClockDispln que es w32time prueba el prefijo está interactuando con el reloj del sistema.
 
A continuación debes buscar el último informe en el registro antes de la hora de conflicto que notifica el equipo de origen que se está aplicando como el reloj de referencia.  Esto podría ser una dirección IP, el nombre de equipo o el proveedor VMIC, que indica que está sincronizando con el Host de Hyper-V.  En el ejemplo siguiente se proporciona una dirección IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Ahora que ha validado el primer sistema de la cadena de hora de referencia, debes investigar el archivo de registro en el origen en tiempo de referencia y repite los mismos pasos.  Este proceso continúa hasta que llegues a un reloj de físico, como GPS o un origen de hora conocida como NIST.  Si el reloj de referencia es hardware GPS, los registros de la fabricación también podrían ser necesarios.

### <a name="network-considerations"></a>Consideraciones de red
Los algoritmos de protocolo NTP tengan una dependencia en la simetría de la red.  Como el aumento de la cantidad de saltos de red, aumenta la probabilidad de asimetría.  Allí, es difícil predecir qué tipos de precisión, verás en sus entornos concretos. 

Monitor de rendimiento y los nuevos contadores de hora de Windows en Windows Server 2016 pueden usarse para evaluar la precisión de entornos y crear líneas de base. Además, puedes realizar la solución de problemas para determinar el desplazamiento actual de cualquier equipo en la red.

Hay dos estándares generales hora exacta en la red.  PTP ([protocolo de tiempo de precisión - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tiene los requisitos más estrictos en la infraestructura de red, pero normalmente se puede proporcionar la precisión microsegundos secundarias.  NTP ([tiempo protocolo de red: RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona en una gran variedad de redes y entornos, lo que hace que sea más fácil de administrar. 

Windows admite NTP Simple (RFC2030) de manera predeterminada para las máquinas combinadas no del dominio.  Para equipos unidos a un dominio, usamos un NTP seguro llamado [MS SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx), que aprovecha los secretos de dominio negociada que proporcionan una ventaja de administración sobre NTP autenticadas descritas en RFC1305 y RFC5905.   

Tanto el dominio y no del dominio combinadas protocolos requiere el puerto UDP 123.  Para obtener más información sobre los procedimientos recomendados NTP, consulte [red tiempo mejor actual prácticas IETF borrador del protocolo](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Reloj de Hardware de confianza (RTC)
Windows lleva tiempo de paso no, a menos que se superan determinados límites, pero en su lugar disciplinas el reloj.  Esto significa que w32tm ajusta la frecuencia del reloj a intervalos regulares, con la frecuencia de actualización de reloj de configuración, los valores predeterminados para una vez por segundo con Windows Server 2016.  Si el reloj se encuentra detrás, acelera la frecuencia y si se trata con antelación, se ralentiza la frecuencia.  Sin embargo, durante este tiempo entre los ajustes de la frecuencia de reloj, el reloj de hardware está en el control.  Si hay un problema con el firmware o el reloj de hardware, el tiempo en el equipo puede ser menos preciso.

Este es otro motivo, que debes probar y línea base en su entorno.  Si el contador de rendimiento "Calcular el desplazamiento de tiempo" no estabilizado con la precisión que te interesa, te convendrá comprobar que el firmware está actualizado.  Otra prueba, puedes ver si el hardware duplicado reproducir el mismo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solución de problemas de precisión de tiempo y NTP
Puedes usar la detección la sección de jerarquía anterior para conocer el origen de una hora inexacta.  Mira el desplazamiento de tiempo, se encuentra el punto en la jerarquía donde tiempo difiere el máximo provecho de su origen NTP.  Una vez que comprendas la jerarquía, querrás probar y comprender por qué ese origen de tiempo determinado no recibe tiempo preciso.  

Centrarse en el sistema con tiempo divergente, puedes usar estas herramientas siguientes para reunir más información para ayudarte a determinar el problema y para encontrar una solución.  La referencia de UpstreamClockSource a continuación, es el reloj detectado mediante "/ de w32tm /config Status".


- Registros de eventos del sistema
- Habilitar el registro usando: w32tm registros - w32tm/Debug/Enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- clave del registro de w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Seguimientos de la red local
- Contadores de rendimiento (desde el equipo local o la UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- UpstreamClockSource PING a comprender la latencia y el número de saltos al origen
- Tracert UpstreamClockSource

Problema|    Síntomas|   Resolución|
----- | ----- | ----- |
Reloj TSC local no es estable.| Usando sincronización Perfmon - equipo físico: reloj estable de reloj, pero sigue sin ver que cada 1-2 minutos de varios 100us. |   Actualizar el Firmware o validar diferentes tipos de hardware no muestran el mismo problema.|
Latencia de red|    w32tm stripchart muestra un RoundTripDelay de más de 10 ms.  Variación en el retraso causar ruido tan grande como la mitad de la hora de ida y vuelta, por ejemplo un retraso solo en una sola dirección.</br></br>UpstreamClockSource es varios saltos, como se indica en el comando "ping".  TTL debe ser similar a 128.</br></br>Usar Tracert para buscar la latencia en cada salto.    | Buscar un origen de reloj más cerca por vez.  Una solución es instalar un reloj de origen en el mismo segmento o apuntar manualmente al reloj de origen que esté más próxima geográficamente.  Para un escenario de dominio, agregar un equipo con el rol GTimeServ. |  
No se puede alcanzar el origen NTP de forma confiable|    W32tm /stripchart intermitentemente devuelve "Solicitud agotada"    |Origen NTP no responde|
Origen NTP no responde|    Comprueba los contadores de rendimiento para las respuestas del recuento de origen de cliente NTP, NTP solicitudes entrantes de servidor, NTP servidor saliente y determinar su uso en comparación con las líneas de base.|    Con los contadores de rendimiento de servidor, determinar si la carga ha cambiado con respecto a las líneas de base.</br></br>¿Hay red problemas congestión?|
Controlador de dominio no mediante el reloj más preciso|    Cambios en la topología o reloj maestro agregado recientemente.|   w32tm /resync /rediscover|
Se deriva relojes del cliente| Servicio hora de evento 36 en registro de eventos del sistema o el texto en el archivo de registro que describen: "NTP cliente tiempo origen" contador van de 1 a 0|Solucionar problemas de la fuente en dirección ascendente y comprender si se está ejecutando en problemas de rendimiento.|

### <a name="baselining-time"></a>Hora de nivel de referencia
Nivel de referencia es importante para que en primer lugar, pueda comprender el rendimiento y la precisión de la red y en comparación con la línea base en el futuro cuando se producen problemas.  Querrás previsto la raíz PDC o todos los equipos marcan con la GTIMESRV.  También te recomendamos el PDC en todos los bosques de línea base.  Por último elige cualquier críticos controladores de dominio o equipos que tienen características interesantes, como la distancia o cargas elevadas y previsto esos.

También resulta útil a la línea base de Windows Server 2016 vs 2012 R2, sin embargo, solo tienes w32tm /stripchart como una herramienta que puedes usar para comparar, dado que Windows Server 2012R2 carece de contadores de rendimiento.  Debes seleccionar dos equipos con las mismas características, o actualizar un equipo y comparar los resultados después de la actualización.  El apéndice de las medidas de tiempo de Windows tiene más información sobre cómo realizar mediciones detalladas entre 2016 y 2012.

Con la todos los w32time contadores de rendimiento, recopilar datos para al menos una semana.  Así se asegurará de que tienes suficiente de una referencia a la cuenta para varios en la red con el tiempo y suficiente de una ejecución de confianza que sea estable la precisión de la hora de proporcionar.

### <a name="ntp-server-redundancy"></a>NTP servidor redundancia
Para la configuración manual de servidor NTP usado con máquinas combinadas de no del dominio o el PDC, tener más de un servidor es una medida de buena redundancia en el caso de disponibilidad.  También puede ofrecer una mejor precisión, suponiendo que todas las fuentes son precisos y estables.  Sin embargo, si aún no está bien diseñada la topología, o las fuentes de tiempo no son estables, la precisión resultante podría ser peor, por lo que recomendamos que tengas precaución.  El límite de tiempo compatible manualmente hacer referencia a los servidores w32time es 10. 

## <a name="leap-seconds"></a>LEAP segundos
Período de rotación de la tierra varía con el tiempo, causado por eventos climáticas y geológicos. Por lo general, la variación es menos de un segundo cada par de años. Siempre que la variación de tiempo atómica crece demasiado grande, se inserta una corrección de un segundo (arriba o abajo), se llama a un segundo leap. Esto se hace de modo tal que la diferencia nunca supere 0,9 segundos. Esta corrección se anunció seis meses antes de la corrección real. Antes de Windows Server 2016, el servicio hora de Microsoft no tenía conocimiento de leap segundos, pero confiar en el servicio de horario externo para encargarse de esto. Con la precisión de tiempo mayor de Windows Server 2016, Microsoft está trabajando en una solución más adecuada para el problema de segundo leap.

## <a name="secure-time-seeding"></a>Proteger el tiempo en grano
W32Time en Server 2016 incluye la función de inicialización de tiempo seguro. Esta función determina la hora actual aproximada desde las conexiones SSL salientes.  Este valor de hora se usa para supervisar el reloj del sistema local y corrige los errores brutos. Puedes leer más acerca de la característica en [esta entrada de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  En las implementaciones con orígenes de una hora confiable y bien supervisados máquinas que incluyen la supervisión de desplazamientos de tiempo, decide no utilizar la característica de inicialización de tiempo seguro y se basan en la infraestructura existente en su lugar. 

Puedes deshabilitar la característica con estos pasos:

1.  Establecer el valor de configuración del registro UtilizeSSLTimeData en 0 en un equipo específico:

    reg agregar HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Si no puedes reiniciar el equipo inmediatamente por algún motivo, puede notificar servicio W32time sobre la actualización de la configuración. Esto impide la supervisión en tiempo y en función de los datos en tiempo de ejecución se recopila desde las conexiones SSL. 

    / Update /config de W32tm.exe

3.  Reiniciar el equipo hace que la configuración efectiva inmediatamente y también hace que deje de recopilar los datos en tiempo de conexiones SSL.  La última parte tiene una sobrecarga muy pequeña y no debe ser un problema de rendimiento.

4.  Para aplicar esta configuración en todo un dominio, Establece el valor de UtilizeSSLTimeData en configuración de directiva de grupo W32time en 0 y publicar la configuración.  Cuando la configuración es seleccionada por un cliente de directiva de grupo, se notifica W32time servicio y dejará de supervisión de tiempo y la aplicación con datos en tiempo de SSL. Se detendrá la recopilación de datos de tiempo SSL cuando se reinicie cada equipo. Si el dominio portátiles delgados portátiles o tabletas y otros dispositivos, puedes excluir estos equipos de este cambio de directiva. Estos dispositivos enfrentan finalmente descarga de batería y la propagación de tiempo seguro necesita características para arrancar el tiempo.














 













 




