---
title: Planeación de la capacidad para Active Directory Domain Services
description: Explicación detallada de los factores que se deben tener en cuenta durante el planeamiento de la capacidad de AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dac13ac94e38cf671239d35507e07d7ac3a0c1ab
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866728"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planeación de la capacidad para Active Directory Domain Services

Este tema está escrito originalmente por Ken Brumfield, Ingeniero Senior de campo Premier en Microsoft y proporciona recomendaciones para planear la capacidad de Active Directory Domain Services (AD DS).

## <a name="goals-of-capacity-planning"></a>Objetivos del planeamiento de capacidad

El planeamiento de la capacidad no es el mismo que para solucionar los incidentes de rendimiento. Están estrechamente relacionados, pero son bastante diferentes. Los objetivos del planeamiento de la capacidad son:  

- Implementar y operar correctamente un entorno 
- Minimice el tiempo empleado en la solución de problemas de rendimiento.  
  
En el planeamiento de la capacidad, una organización podría tener un destino de línea base de un 40% de uso del procesador durante períodos de máxima actividad para satisfacer los requisitos de rendimiento del cliente y dar cabida al tiempo necesario para actualizar el hardware en el centro de información. Mientras que, para recibir notificaciones de incidentes de rendimiento anómalos, se puede establecer un umbral de alerta de supervisión al 90% durante un intervalo de 5 minutos.

La diferencia es que cuando se supera continuamente el umbral de administración de capacidad (un evento único no es un problema), agregar capacidad (es decir, agregar en procesadores más o más rápidos) sería una solución o el escalado del servicio entre varios servidores sería solución. Los umbrales de alerta de rendimiento indican que la experiencia del cliente está sufriendo actualmente y que se necesitan pasos inmediatos para solucionar el problema.

Como analogía: la administración de la capacidad está a punto de prevenir un accidente de automóvil (conducción defensiva, asegurarse de que los frenos funcionan correctamente, etc.), mientras que la solución de problemas de rendimiento es lo que hacen la policía, el Departamento de bomberos y los profesionales médicos de emergencia. después de un accidente. Se trata de la "conducción defensiva", el estilo de Active Directory.

Durante los últimos años, la guía de planeamiento de la capacidad de los sistemas de escalado vertical ha cambiado drásticamente. Los siguientes cambios en las arquitecturas del sistema han motivado las suposiciones fundamentales sobre el diseño y el escalado de un servicio:

- plataformas de servidor de 64 bits  
- Virtualización  
- Mayor atención al consumo de energía  
- Almacenamiento SSD  
- Escenarios de nube  

Además, el enfoque se desplaza de un ejercicio de planeamiento de la capacidad basado en servidor a un ejercicio de planeamiento de la capacidad basado en el servicio. Active Directory Domain Services (AD DS), un servicio distribuido consolidado que muchos productos de Microsoft y de terceros usan como back-end, se convierte en uno de los productos más importantes que se deben planear correctamente para garantizar la capacidad necesaria para que se ejecuten otras aplicaciones.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisitos de línea de base para la guía de planeamiento de capacidad

En este artículo, se esperan los siguientes requisitos de línea base:

- Los lectores han leído y están familiarizados con las [directrices para la optimización del rendimiento de Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La plataforma de Windows Server es una arquitectura basada en x64. Pero incluso si su entorno de Active Directory está instalado en Windows Server 2003 x86 (ahora más allá del final del ciclo de vida de soporte técnico) y tiene un árbol de información de directorio (DIT) de menos de 1,5 GB de tamaño y que se puede retener fácilmente en la memoria, las instrucciones de este el artículo sigue siendo aplicable.
- El planeamiento de la capacidad es un proceso continuo y debe revisar con regularidad si el entorno cumple con las expectativas.
- La optimización se realizará en varios ciclos de vida de hardware a medida que cambian los costos de hardware. Por ejemplo, la memoria es más barata, el costo por núcleo disminuye o el precio de las diferentes opciones de almacenamiento cambian.
- Planee el período máximo de actividad del día. Se recomienda consultarlo en intervalos de 30 minutos o de una hora. Cualquier valor mayor puede ocultar los picos reales y todo lo que sea menor podría quedar distorsionado por "picos transitorios".
- Planee el crecimiento en el transcurso del ciclo de vida de hardware de la empresa. Esto puede incluir una estrategia de actualización o adición de hardware de forma escalonada o una actualización completa cada tres a cinco años. Cada una de ellas requerirá una "estimación" de cuánto aumentará la carga de Active Directory. Los datos históricos, si se recopilan, le ayudarán en esta evaluación. 
- Planear la tolerancia a errores. Una vez que se deriva una estimación *n* , planee escenarios que incluyan *n* &ndash; 1, *n* &ndash; 2, *n* &ndash; *x*.
  - Agregue servidores adicionales en función de la necesidad de la organización para asegurarse de que la pérdida de un único servidor o de varios servidores no supere el número máximo de estimaciones de capacidad máxima.
  - Tenga en cuenta también que el plan de crecimiento y el plan de tolerancia a errores deben estar integrados. Por ejemplo, si se requiere un controlador de dominio para admitir la carga, pero el cálculo es que la carga se duplicará en el próximo año y requiere dos controladores de dominio en total, no habrá suficiente capacidad para admitir la tolerancia a errores. La solución sería iniciar con tres controladores de DC. También podría planear agregar el tercer DC después de 3 o 6 meses si los presupuestos son estrechos.

    >[!NOTE]
    >La adición de aplicaciones compatibles con Active Directory puede tener un impacto perceptible en la carga del controlador de dominio, tanto si la carga procede de los servidores de aplicaciones como de los clientes.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Proceso de tres pasos para el ciclo de planeamiento de la capacidad

En el planeamiento de la capacidad, primero decida qué calidad de servicio se necesita. Por ejemplo, un centro de información principal admite un mayor nivel de simultaneidad y requiere una experiencia más coherente para los usuarios y el consumo de aplicaciones, lo que requiere una mayor atención a la redundancia y la reducción de los cuellos de botella del sistema y de la infraestructura. Por el contrario, una ubicación satélite con pocos usuarios no necesita el mismo nivel de simultaneidad o tolerancia a errores. Por lo tanto, es posible que la oficina satélite no necesite tanta atención en la optimización del hardware y la infraestructura subyacentes, lo que puede dar lugar a ahorros de costos. Todas las recomendaciones y las instrucciones que se incluyen son para un rendimiento óptimo y se pueden relajar selectivamente en escenarios con requisitos menos exigentes.

La siguiente pregunta es: virtualizado o físico. Desde la perspectiva de la planificación de la capacidad, no hay ninguna respuesta correcta o incorrecta; solo hay un conjunto diferente de variables con las que trabajar. Los escenarios de virtualización se desconectan a una de estas dos opciones:

- "Asignación directa" con un invitado por host (donde la virtualización existe únicamente para abstraer el hardware físico del servidor)
- "Host compartido"

Los escenarios de prueba y producción indican que el escenario de "asignación directa" se puede tratar de manera idéntica a un host físico. Sin embargo, "host compartido" presenta una serie de consideraciones que se han escrito con más detalle más adelante. El escenario de "host compartido" significa que AD DS también está compitiendo por los recursos y hay penalizaciones y consideraciones de optimización para hacerlo.

Teniendo en cuenta estas consideraciones, el ciclo de planeamiento de la capacidad es un proceso iterativo de tres pasos:

1. Mida el entorno existente, determine dónde están los cuellos de botella del sistema actualmente y obtenga los aspectos básicos del entorno necesarios para planear la cantidad de capacidad necesaria.
1. Determine el hardware necesario según los criterios descritos en el paso 1.
1. Supervisar y validar que la infraestructura tal como está implementada está funcionando en las especificaciones. Algunos datos recopilados en este paso se convierten en la línea base para el siguiente ciclo de planeamiento de la capacidad.

### <a name="applying-the-process"></a>Aplicar el proceso

Para optimizar el rendimiento, asegúrese de que estos componentes principales se seleccionan correctamente y se optimizan para las cargas de la aplicación:

1. Memoria
1. Red
1. Almacenamiento
1. Procesador
1. Inicio de sesión de red

Los requisitos básicos de almacenamiento de AD DS y el comportamiento general del software cliente bien escrito permiten entornos con un máximo de 10.000 a 20.000 usuarios a una gran inversión en el planeamiento de la capacidad con respecto al hardware físico, como casi cualquier servidor moderno. el sistema de clase controlará la carga. Dicho esto, en la tabla siguiente se resume cómo evaluar un entorno existente para seleccionar el hardware adecuado. Cada componente se analiza en detalle en secciones posteriores para ayudar a AD DS los administradores a evaluar su infraestructura mediante recomendaciones de línea de base y entidades de seguridad específicas del entorno.

En general:

- Cualquier cambio de tamaño basado en los datos actuales solo será preciso para el entorno actual.
- En el caso de las estimaciones, se espera que la demanda crezca a lo largo del ciclo de vida del hardware.
- Determine si está sobredimensionado hoy y crezca en el entorno más grande, o agregue capacidad a lo largo del ciclo de vida.
- En lo que se refiere a la virtualización, se aplican las mismas metodologías y entidades de seguridad de planeamiento de la capacidad, salvo que la sobrecarga de la virtualización debe agregarse a cualquier dominio relacionado.
- El planeamiento de la capacidad, como cualquier cosa que intente predecir, no es una ciencia precisa. No espere que los elementos calculen perfectamente y con una precisión del 100%. Esta guía es la recomendación más detallada; agregue capacidad para la seguridad adicional y valide continuamente que el entorno permanece en el destino.

### <a name="data-collection-summary-tables"></a>Tablas de Resumen de recopilación de datos

#### <a name="new-environment"></a>Nuevo entorno

| Componente | Cálculos |
|-|-|
|Almacenamiento/tamaño de la base de datos|de 40 KB a 60 KB para cada usuario|
|RAM|Tamaño de la base de datos<br />Recomendaciones del sistema operativo base<br />Aplicaciones de terceros|
|Red|1 GB|
|CPU|1000 usuarios simultáneos para cada núcleo|

#### <a name="high-level-evaluation-criteria"></a>Criterios de evaluación de alto nivel

| Componente | Criterios de evaluación | Consideraciones sobre planeación |
|-|-|-|
|Almacenamiento/tamaño de la base de datos|La sección titulada "para activar el registro de espacio en disco que ha liberado la desfragmentación" en [límites de almacenamiento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Rendimiento de almacenamiento/base de datos|<ul><li>"LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg segundos/lectura," "LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg de disco/escritura", "LogicalDisk ( *\<unidad de base de datos Ntds) )\>* \Avg de segundos de disco/transferencia "</li><li>"LogicalDisk ( *\<unidad\>de base de datos Ntds*) \ lecturas/seg." "LogicalDisk ( *\<unidad\>de base de datos Ntds*) \ escrituras/seg." "LogicalDisk ( *\<unidad\>debasededatosNtds*) \ Transferencias/s "</li></ul>|<ul><li>El almacenamiento tiene dos preocupaciones<ul><li>El espacio disponible, que con el tamaño del almacenamiento basado en el eje actual y en la SSD, es irrelevante para la mayoría de los entornos de AD.</li> <li>Operaciones de entrada/salida (e/s) disponibles: en muchos entornos, a menudo se pasa por alto. Pero es importante evaluar solo los entornos en los que no hay suficiente RAM para cargar toda la base de datos NTDS en la memoria.</li></ul><li>El almacenamiento puede ser un tema complejo y debe implicar la experiencia del fabricante del hardware para que el tamaño sea adecuado. Especialmente con escenarios más complejos, como escenarios SAN, NAS e iSCSI. Sin embargo, en general, el costo por Gigabyte de almacenamiento suele ser la oposición directa del costo por e/s:<ul><li>RAID 5 tiene un costo menor por Gigabyte que RAID 1, pero RAID 1 tiene un costo menor por e/s</li><li>Las unidades de disco duro basadas en el eje tienen un costo menor por Gigabyte, pero las SSD tienen un costo menor por e/s</li></ul><li>Después de reiniciar el equipo o el servicio Active Directory Domain Services, la caché del motor de almacenamiento extensible (ESE) está vacía y el rendimiento se enlazará en disco mientras se calienta la memoria caché.</li><li>En la mayoría de los entornos, AD es una e/s de lectura intensiva en un patrón aleatorio a los discos, lo que evita muchas de las ventajas del almacenamiento en caché y las estrategias de optimización de lectura.  Además, AD tiene una caché más grande en la memoria que la mayoría de las memorias caché del sistema de almacenamiento.</li></ul>
|RAM|<ul><li>Tamaño de la base de datos</li><li>Recomendaciones del sistema operativo base</li><li>Aplicaciones de terceros</li></ul>|<ul><li>El almacenamiento es el componente más lento de un equipo. Cuanto más pueda residir en la RAM, menos será necesario ir a disco.</li><li>Asegúrese de que hay suficiente RAM asignada para almacenar el sistema operativo, los agentes (antivirus, copia de seguridad, supervisión), la base de datos NTDS y el crecimiento a lo largo del tiempo.</li><li>En el caso de los entornos en los que maximizar la cantidad de memoria RAM no es rentable (como una ubicación satélite) o no factible (DIT es demasiado grande), haga referencia a la sección Storage para asegurarse de que el almacenamiento tenga el tamaño adecuado.</li></ul>|
|Red|<ul><li>"Interfaz de red\*() \Bytes recibidos por segundo"</li><li>"Interfaz de red\*() \Bytes enviados por segundo"|<ul><li>En general, el tráfico enviado desde un controlador de dominio supera mucho el tráfico enviado a un controlador de dominio.</li><li>Como conexión Ethernet conmutada, el tráfico de red de entrada y salida de dúplex completo debe ajustarse de forma independiente.</li><li>La consolidación del número de controladores de dominio aumentará la cantidad de ancho de banda que se usa para devolver las respuestas a las solicitudes de cliente para cada controlador de dominio, pero estará suficientemente cerca para lineal en todo el sitio.</li><li>Si quita los controladores de dominio de Ubicación satélite, no olvide agregar el ancho de banda para el controlador de dominio satélite en los controladores de dominio del concentrador y usarlo para evaluar la cantidad de tráfico WAN que habrá.</li></ul>|
|CPU|<ul><li>"Disco lógico ( *\<unidad\>de base de datos Ntds*) \Avg segundos/lectura"</li><li>"Proceso (LSASS)\\% de tiempo de procesador"</li></ul>|<ul><li>Después de eliminar el almacenamiento como cuello de botella, solucione la cantidad de potencia de proceso necesaria.</li><li>Aunque no es absolutamente lineal, el número de núcleos de procesador consumido en todos los servidores dentro de un ámbito específico (por ejemplo, un sitio) se puede usar para medir el número de procesadores necesarios para admitir la carga total de clientes. Agregue el mínimo necesario para mantener el nivel actual de servicio en todos los sistemas dentro del ámbito.</li><li>Los cambios en la velocidad del procesador, incluidos los cambios relacionados con la administración de energía, los números de impacto derivados del entorno actual. Por lo general, es imposible evaluar con exactitud cómo pasar de un procesador de 2,5 GHz a un procesador de 3 GHz reducirá el número de CPU necesarias.</li></ul>|
|NetLogon|<ul><li>"Netlogon (\*) \Semaphore adquiere"</li><li>"Netlogon (\*) tiempos de espera de \Semaphore"</li><li>"Netlogon (\*) \Average de tiempo de espera del semáforo"</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI solo afecta a entornos con autenticaciones NTLM y/o la validación de PAC. La validación de PAC está activada de forma predeterminada en versiones de sistema operativo anteriores a Windows Server 2008. Se trata de una configuración de cliente, por lo que los controladores de sesión se verán afectados hasta que esté desactivada en todos los sistemas cliente.</li><li>Los entornos con una autenticación de confianza cruzada importante, que incluye confianzas dentro de bosques, tienen un mayor riesgo si no tienen un tamaño adecuado.</li><li>Las consolidaciones de servidor aumentarán la simultaneidad de la autenticación entre confianzas.</li><li>Es necesario acomodar los picos, como las conmutaciones por error de los clústeres, a medida que los usuarios vuelven a autenticarse en el nuevo nodo de clúster.</li><li>Es posible que los sistemas cliente individuales (por ejemplo, un clúster) también necesiten la optimización.</li></ul>|

## <a name="planning"></a>Planificación

Durante mucho tiempo, la recomendación de la comunidad de ajustar el tamaño de la AD DS ha sido "poner tanta RAM como el tamaño de la base de datos". En su mayor parte, esta recomendación es todo lo que la mayoría de los entornos necesitan para preocuparse. Pero el ecosistema que consume AD DS ha ido mucho más grande, ya que tiene los propios entornos AD DS, desde su introducción en 1999. Aunque el aumento de la capacidad de proceso y el cambio de las arquitecturas x86 a las arquitecturas x64 han realizado los aspectos más sutiles de tamaño para el rendimiento irrelevante para un conjunto mayor de clientes que ejecutan AD DS en hardware físico, el crecimiento de la virtualización tiene representó los problemas de optimización a una audiencia más grande que antes.

La siguiente orientación es, por lo tanto, sobre cómo determinar y planear las demandas de Active Directory como un servicio independientemente de si se implementa en una combinación física, física o virtual, o un escenario meramente virtualizado. Como tal, se desglosará la evaluación en cada uno de los cuatro componentes principales: almacenamiento, memoria, red y procesador. En Resumen, para maximizar el rendimiento en AD DS, el objetivo es llegar lo más cerca posible al procesador.

## <a name="ram"></a>RAM

Simplemente, cuanto más se puede almacenar en la memoria caché, menos es necesario ir a disco. Para maximizar la escalabilidad del servidor, la cantidad mínima de RAM debe ser la suma del tamaño actual de la base de datos, el tamaño total de SYSVOL, la cantidad recomendada de sistema operativo y las recomendaciones de proveedor para los agentes (antivirus, supervisión, copia de seguridad, etc.). ). Se debe agregar una cantidad adicional para acomodar el crecimiento de la duración del servidor. Esto será subjetiva en el entorno en función de las estimaciones de crecimiento de la base de datos en función de los cambios del entorno.

En el caso de los entornos en los que maximizar la cantidad de memoria RAM no es rentable (como una ubicación satélite) o no factible (el DIT es demasiado grande), consulte la sección Storage para asegurarse de que el almacenamiento está correctamente diseñado.

Una consecuencia que se genera en el contexto general de tamaño de la memoria es el tamaño del archivo de paginación. En el mismo contexto que cualquier otra memoria relacionada, el objetivo es minimizar el disco más lento. Por lo tanto, la pregunta debe ir de "¿cómo se debe ajustar el tamaño del archivo de paginación?" "¿cuánta RAM se necesita para minimizar la paginación?" En el resto de esta sección se describe la respuesta a la última pregunta. Esto deja la mayor parte del análisis para el ajuste de tamaño del archivo de paginación en el dominio de recomendaciones generales del sistema operativo y la necesidad de configurar el sistema para volcados de memoria, que no están relacionados con el rendimiento de la AD DS.

### <a name="evaluating"></a>Evaluar

La cantidad de RAM que necesita un controlador de dominio (DC) es realmente un ejercicio complejo por estos motivos:

- Alta posibilidad de error al intentar usar un sistema existente para medir la cantidad de RAM necesaria, ya que LSASS se recortará en condiciones de presión de memoria, con lo que se DEFLATE artificialmente la necesidad.
- El hecho de que un DC individual solo necesita almacenar en caché lo que es "interesante" para sus clientes. Esto significa que los datos que deben almacenarse en memoria caché en un controlador de dominio de un sitio con solo un servidor de Exchange serán muy diferentes de los datos que deben almacenarse en caché en un controlador de dominio que solo autentique a los usuarios.
- La mano de obra para evaluar la RAM para cada DC en cada caso es prohibitiva y cambia a medida que cambia el entorno.
- Los criterios que se basan en la recomendación le ayudarán a tomar decisiones informadas: 
- Cuanto más se pueda almacenar en la memoria caché, menos será necesario ir al disco. 
- El almacenamiento es el componente más lento de un equipo. El acceso a los datos en medios de almacenamiento de SSD y basados en el eje se encuentra en el orden de 1, 000 y 1000 veces más lento que el acceso a los datos de la RAM.

Por lo tanto, para maximizar la escalabilidad del servidor, la cantidad mínima de RAM es la suma del tamaño actual de la base de datos, el tamaño total de SYSVOL, la cantidad recomendada del sistema operativo y las recomendaciones de proveedor para los agentes (antivirus, supervisión, copia de seguridad, etc.). Agregue cantidades adicionales para acomodar el crecimiento de la duración del servidor. Esto será subjetiva en el entorno en función de las estimaciones de crecimiento de la base de datos. Sin embargo, en el caso de las ubicaciones de satélite con un pequeño conjunto de usuarios finales, estos requisitos se pueden relajar, ya que estos sitios no tendrán que almacenar en caché tantas solicitudes como servicio.

En el caso de los entornos en los que maximizar la cantidad de memoria RAM no es rentable (como una ubicación satélite) o no factible (DIT es demasiado grande), haga referencia a la sección Storage para asegurarse de que el almacenamiento tenga el tamaño adecuado.

> [!NOTE]
> Un consecuencia del tamaño de la memoria es el tamaño del archivo de paginación. Dado que el objetivo es minimizar el acceso al disco mucho más lento, la pregunta va de "¿cómo se debe ajustar el tamaño del archivo de paginación?". "¿cuánta RAM se necesita para minimizar la paginación?" En el resto de esta sección se describe la respuesta a la última pregunta. Esto deja la mayor parte del análisis para el ajuste de tamaño del archivo de paginación en el dominio de recomendaciones generales del sistema operativo y la necesidad de configurar el sistema para volcados de memoria, que no están relacionados con el rendimiento de la AD DS.

### <a name="virtualization-considerations-for-ram"></a>Consideraciones sobre la virtualización para RAM

Evite la confirmación de la memoria en el host. El objetivo fundamental de la optimización de la cantidad de RAM es minimizar la cantidad de tiempo empleado en el disco. En los escenarios de virtualización, el concepto de memoria sobrecommit existe cuando se asigna más RAM a los invitados y, a continuación, existe en la máquina física. Esto no es un problema en sí mismo. Se convierte en un problema cuando la memoria total utilizada activamente por todos los invitados supera la cantidad de memoria RAM del host y el host subyacente inicia la paginación. El rendimiento se convierte en enlazado a disco en los casos en los que el controlador de dominio va a NTDS. dit para obtener datos, o bien el controlador de dominio va al archivo de paginación para obtener los datos, o bien el host va a disco para obtener datos que el invitado considera que está en RAM.

### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

|Componente|Memoria estimada (ejemplo)|
|-|-|
|RAM recomendada del sistema operativo base (Windows Server 2008)|2 GB|
|Tareas internas de LSASS|200 MB|
|Agente de supervisión|100 MB|
|Antivirus|100 MB|
|Base de datos (catálogo global)|8,5 GB está seguro de que???|
|Cojín para que se ejecute la copia de seguridad, los administradores pueden iniciar sesión sin impacto|1 GB|
|Total|12 GB|

**Recomendar 16 GB**

Con el tiempo, se puede suponer que se agregarán más datos a la base de datos y el servidor probablemente estará en producción durante 3 o 5 años. En función de una estimación del crecimiento del 33%, 16 GB sería una cantidad razonable de RAM para colocar en un servidor físico. En una máquina virtual, dada la facilidad con la que se puede modificar la configuración y se puede Agregar RAM a la máquina virtual, a partir de 12 GB con el plan para supervisar y actualizar en el futuro es razonable.

## <a name="network"></a>Red

### <a name="evaluating"></a>Evaluar
Esta sección no trata sobre la evaluación de las demandas en relación con el tráfico de replicación, que se centra en el tráfico que atraviesa la WAN y que se trata exhaustivamente en [Active Directory tráfico de replicación](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), que en la evaluación de la red y el ancho de banda total. capacidad necesaria, incluidas las consultas de cliente, las aplicaciones directiva de grupo, etc. En el caso de los entornos existentes, se puede recopilar mediante los contadores de\*rendimiento "interfaz de red \Bytes recibidos/seg.\*" y "interfaz de red \Bytes enviados/seg." Intervalos de muestra para los contadores de la interfaz de red en 15, 30 o 60 minutos. Todo menos normalmente será demasiado volátil para las mediciones correctas; cualquier valor mayor suavizará las búsquedas diarias de forma excesiva.

> [!NOTE]
> Por lo general, la mayoría del tráfico de red en un controlador de dominio es saliente, ya que el controlador de dominio responde a las consultas de cliente. Este es el motivo por el que se centra en el tráfico de salida, aunque se recomienda evaluar también cada entorno para el tráfico de entrada. Se pueden usar los mismos enfoques para abordar y revisar los requisitos de tráfico de red de entrada. Para obtener más información, vea el artículo [929851 de Knowledge Base: El intervalo de puertos dinámicos predeterminado para TCP/IP ha cambiado en Windows Vista y en Windows](http://support.microsoft.com/kb/929851)Server 2008.

### <a name="bandwidth-needs"></a>Necesidades de ancho de banda

La planificación de la escalabilidad de la red cubre dos categorías distintas: la cantidad de tráfico y la carga de la CPU del tráfico de red. Cada uno de estos escenarios es directo en comparación con algunos de los otros temas de este artículo.

Al evaluar la cantidad de tráfico que se debe admitir, existen dos categorías únicas de planeamiento de la capacidad para AD DS en términos de tráfico de red. La primera es el tráfico de replicación que atraviesa los controladores de dominio y que se trata minuciosamente en la referencia [Active Directory el tráfico de replicación](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) y sigue siendo relevante para las versiones actuales de AD DS. El segundo es el tráfico de cliente a servidor entre sitios. Uno de los escenarios más sencillos para planear, el tráfico entre sitios recibe principalmente solicitudes pequeñas de los clientes en relación con las grandes cantidades de datos que se envían de vuelta a los clientes. 100 MB generalmente será adecuado en entornos de hasta 5.000 usuarios por servidor, en un sitio de. El uso de un adaptador de red de 1 GB y la compatibilidad con el ajuste de escala en lado de recepción (RSS) se recomienda para todos los usuarios anteriores a 5.000. Para validar este escenario, especialmente en el caso de los escenarios de consolidación de servidores, examine la\*interfaz de red () \ bytes/seg. en todos los controladores de dominio de un sitio, agréguelos juntos y divida por el número objetivo de controladores de dominio para asegurarse de que hay es la capacidad adecuada. La manera más sencilla de hacerlo es usar la vista "área apilada" en el monitor de confiabilidad y rendimiento de Windows (anteriormente conocido como PerfMon), asegurándose de que todos los contadores se escalan de la misma forma.

Considere el siguiente ejemplo (también conocido como, una forma realmente realmente compleja de validar que la regla general es aplicable a un entorno específico). Se realizan las suposiciones siguientes:

- El objetivo es reducir el tamaño de los servidores que sea posible. Idealmente, un servidor llevará a cabo la carga y se implementará un servidor adicional para la redundancia (escenario *N* + 1). 
- En este escenario, el adaptador de red actual solo admite 100 MB y se encuentra en un entorno de conmutación.  
  El uso máximo de ancho de banda de red de destino es del 60% en un escenario N (pérdida de un controlador de dominio).
- Cada servidor tiene aproximadamente 10.000 clientes conectados a él.

Conocimiento obtenido de los datos del gráfico (interfaz de red (\*) \Bytes enviados/seg.):

1. El día laborable comienza en torno a 5:30 y gira hacia abajo a las 7:00 PM.
1. El período de picos más ocupado es de 8:00 A.M. a 8:15 AM, con más de 25 bytes enviados/seg. en el controlador de dominio más ocupado.  
   > [!NOTE]
   > Todos los datos de rendimiento son históricos. Por lo tanto, el punto de datos pico en 8:15 indica la carga de 8:00 a 8:15.
1. Hay picos anteriores a 4:00 AM, con más de 20 bytes enviados/seg. en el controlador de dominio más ocupado, lo que podría indicar la carga de distintas zonas horarias o la actividad de la infraestructura en segundo plano, como las copias de seguridad. Dado que el pico en 8:00 AM supera esta actividad, no es relevante.
1. Hay cinco controladores de dominio en el sitio.
1. La carga máxima es de aproximadamente 5,5 MB/s por DC, que representa el 44% de la conexión de 100 MB. Con estos datos, se puede estimar que el ancho de banda total necesario entre 8:00 AM y 8:15 AM es 28 MB/s.
   >[!NOTE]
   >Tenga cuidado con el hecho de que los contadores de envío/recepción de la interfaz de red se encuentran en bytes y el ancho de banda de red se mide en bits. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Sacar

1. Este entorno actual cumple el nivel N + 1 de tolerancia a errores en el uso de destino del 60%. Desconectar un sistema cambiará el ancho de banda por servidor de aproximadamente 5,5 MB/s (44%) a unos 7 MB/s (56%).
1. En función del objetivo previamente indicado de la consolidación en un servidor, esto supera el uso de destino máximo y, teóricamente, el posible uso de una conexión de 100 MB.
1. Con una conexión de 1 GB, se representará el 22% de la capacidad total.
1. En condiciones de funcionamiento normales en el escenario *N* + 1, la carga de cliente se distribuirá relativamente uniformemente a aproximadamente 14 MB/s por servidor o el 11% de la capacidad total.
1. Para asegurarse de que la capacidad es adecuada durante la falta de disponibilidad de un controlador de dominio, los destinos operativos normales por servidor serían aproximadamente un 30% de uso de red o 38 MB/s por servidor. Los destinos de la conmutación por error serían del 60% de uso de red o de 72 MB/s por servidor.  
  
En Resumen, la implementación final de los sistemas debe tener un adaptador de red de 1 GB y estar conectado a una infraestructura de red que admita dicha carga. Una nota adicional es que, dada la cantidad de tráfico de red generado, la carga de CPU de las comunicaciones de red puede tener un impacto significativo y limitar la escalabilidad máxima de AD DS. Este mismo proceso se puede usar para calcular la cantidad de comunicación entrante con el controlador de dominio. Pero dada la predominación del tráfico saliente en relación con el tráfico entrante, es un ejercicio académico para la mayoría de los entornos. Garantizar la compatibilidad de hardware para RSS es importante en entornos con más de 5.000 usuarios por servidor. En escenarios con un tráfico de red elevado, el equilibrio de la carga de interrupción puede ser un cuello de botella. Esto puede ser detectado por el tiempo\*de\% interrupción del procesador () que se distribuye de forma desigual entre las CPU. Las NIC habilitadas para RSS pueden mitigar esta limitación y aumentar la escalabilidad.

> [!NOTE]
> Se puede usar un enfoque similar para estimar la capacidad adicional necesaria al consolidar centros de datos o retirar un controlador de dominio en una ubicación de satélite. Simplemente recopile el tráfico entrante y saliente a los clientes y que será la cantidad de tráfico que ahora estará presente en los vínculos WAN.  
>  
> En algunos casos, puede experimentar más tráfico del esperado porque el tráfico es más lento, por ejemplo, cuando la comprobación de certificados no cumple los tiempos de espera agresivos de la WAN. Por esta razón, el tamaño y el uso de WAN deben ser un proceso iterativo y continuo.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Consideraciones sobre la virtualización para el ancho de banda de red

Es fácil realizar recomendaciones para un servidor físico: 1 GB para servidores que admiten más de 5000 usuarios. Una vez que varios invitados empiecen a compartir una infraestructura de conmutador virtual subyacente, es necesario prestar atención adicional para asegurarse de que el host tiene el ancho de banda de red adecuado para admitir todos los invitados del sistema y, por tanto, requiere rigor adicional. Esto no es nada más que una extensión de garantizar la infraestructura de red en el equipo host. Esto es así independientemente de si la red está incluida en el controlador de dominio que se ejecuta como invitado de la máquina virtual en un host con el tráfico de red que va a través de un conmutador virtual o si está conectado directamente a un conmutador físico. El conmutador virtual es simplemente un componente más en el que el vínculo superior necesita admitir la cantidad de datos que se transmiten. Por lo tanto, el adaptador de red físico del host físico vinculado al conmutador debe ser capaz de admitir la carga de DC más los demás invitados que comparten el conmutador virtual conectado al adaptador de red físico.

### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

|Sistema|Ancho de banda máximo|
|-|-|
DC 1|6,5 MB/s|
DC 2|6,25 MB/s|
|DC 3|6,25 MB/s|
|DC 4|5,75 MB/s|
|DC 5|4,75 MB/s|
|Total|28,5 MB/s|

**Recomendar 72 MB/s** (28,5 MB/s divididos por 40%)

|Recuento de sistemas de destino|Ancho de banda total (anterior)|
|-|-|
|2|28,5 MB/s|
|Comportamiento normal resultante|28,5 &divide; 2 = 14,25 MB/s|

Como siempre, a lo largo del tiempo se puede hacer que la carga de cliente aumente y este crecimiento debe planearse lo mejor posible. La cantidad recomendada para planear permitiría un crecimiento estimado en el tráfico de red del 50%.

## <a name="storage"></a>Almacenamiento

El almacenamiento de planeación constituye dos componentes:

- Capacidad o tamaño de almacenamiento
- Rendimiento

Una gran cantidad de tiempo y documentación se dedica a la planeación de la capacidad, lo que a menudo se pasa por alto el rendimiento. Con los costos de hardware actuales, la mayoría de los entornos no son lo suficientemente grandes como para que cualquiera de ellos sea realmente un problema, y la recomendación de "poner en la memoria RAM como el tamaño de la base de datos" suele abarcar el resto, aunque puede ser exagerada para ubicaciones de satélite mayores entorno.

### <a name="sizing"></a>Ajuste de tamaño

#### <a name="evaluating-for-storage"></a>Evaluación de almacenamiento

En comparación con los 13 años en que se presentó Active Directory, una hora en la que las unidades de 4 GB y 9 GB eran los tamaños de unidad más comunes, el ajuste de tamaño para Active Directory no es siquiera una consideración para todos menos los entornos más grandes. Con los tamaños de disco duro más pequeños disponibles en el intervalo de 180 GB, el sistema operativo completo, SYSVOL y NTDS. dit pueden encontrarse fácilmente en una unidad. Como tal, se recomienda dejar en desuso la inversión pesada en esta área.

La única recomendación a tener en cuenta es asegurarse de que el 110% del tamaño de NTDS. dit está disponible para habilitar Defrag. Además, se deben realizar las adaptaciones de crecimiento a lo largo de la vida del hardware.

La primera consideración y la más importante es evaluar el tamaño de NTDS. DIT y SYSVOL. Estas medidas darán lugar a un ajuste de la asignación de disco fijo y RAM. Debido al bajo costo (relativamente) de estos componentes, no es necesario que el cálculo sea riguroso y preciso. El contenido sobre cómo evaluar esto para los entornos nuevos y existentes se puede encontrar en la serie de artículos de [almacenamiento de datos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) . En concreto, consulte los siguientes artículos:

- En el **caso &ndash; de los entornos existentes** , la sección titulada "para activar el registro de espacio en disco que se libera mediante la desfragmentación" en el artículo [límites de almacenamiento](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **En el caso &ndash; de los nuevos entornos** , el artículo titulado [estimaciones de crecimiento para Active Directory usuarios y unidades organizativas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Los artículos se basan en las estimaciones de tamaño de datos realizadas en el momento de la publicación de Active Directory en Windows 2000. Use tamaños de objeto que reflejen el tamaño real de los objetos de su entorno.

Al revisar los entornos existentes con varios dominios, puede haber variaciones en los tamaños de las bases de datos. En este caso, use el catálogo global (GC) más pequeño y los tamaños que no sean de GC.  

El tamaño de la base de datos puede variar entre versiones del sistema operativo. Los controladores de dominio que ejecutan sistemas operativos anteriores, como Windows Server 2003, tienen un tamaño de base de datos menor que un controlador de dominio que ejecuta un sistema operativo posterior como Windows Server 2008 R2, especialmente cuando se habilitan características Active Directory como la papelera de reciclaje o la itinerancia de credenciales.

> [!NOTE]  
  >
>- En el caso de los nuevos entornos, tenga en cuenta que las estimaciones de las estimaciones de crecimiento de Active Directory usuarios y unidades organizativas indican que 100.000 usuarios (en el mismo dominio) consumen aproximadamente 450 MB de espacio. Tenga en cuenta que los atributos rellenados pueden tener un gran impacto en la cantidad total. Los atributos se rellenarán en muchos objetos de productos de terceros y de Microsoft, incluidos Microsoft Exchange Server y Lync. Se prefiere una evaluación basada en la cartera de productos del entorno, pero el ejercicio de detallar los cálculos matemáticos y las pruebas para estimaciones precisas de todos los entornos, pero los más grandes, puede que realmente no merezca la pena tiempo y esfuerzo.
>- Asegúrese de que el 110% del tamaño de NTDS. dit esté disponible como espacio disponible para habilitar la desfragmentación sin conexión y planee el crecimiento durante un período de vida de hardware de tres a cinco años. Dado que el almacenamiento económico es, la estimación del almacenamiento al 300% del tamaño del DIT como asignación de almacenamiento es segura para acomodar el crecimiento y la posible necesidad de desfragmentación sin conexión.

#### <a name="virtualization-considerations-for-storage"></a>Consideraciones sobre la virtualización para el almacenamiento

En un escenario en el que se asignan varios archivos de disco duro virtual (VHD) a un solo volumen, use un disco de tamaño fijo de al menos el 210% (100% del DIT + 110% de espacio libre) el tamaño del DIT para asegurarse de que haya espacio suficiente reservado.  

#### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

|Datos recopilados de la fase de evaluación| |
|-|-|
|Tamaño de NTDS. dit|35 GB|
|Modificador para permitir la desfragmentación sin conexión|2.1|
|Almacenamiento total necesario|73,5 GB|

> [!NOTE]
> Este almacenamiento necesario se suma al almacenamiento necesario para SYSVOL, sistema operativo, archivo de paginación, archivos temporales, datos almacenados en caché locales (como archivos de instalador) y aplicaciones.

### <a name="storage-performance"></a>Rendimiento del almacenamiento

#### <a name="evaluating-performance-of-storage"></a>Evaluación del rendimiento del almacenamiento

Como componente más lento dentro de cualquier equipo, el almacenamiento puede tener el mayor impacto adverso en la experiencia del cliente. En el caso de los entornos lo suficientemente grandes como para que las recomendaciones de ajuste de tamaño de RAM no sean factibles, las consecuencias de pasar el almacenamiento de planeación para el rendimiento pueden ser devastadoras.  Además, las complejidades y las variedades de la tecnología de almacenamiento mejoran el riesgo de que se produzca un error, ya que la relevancia de las prácticas recomendadas de larga duración de "poner el sistema operativo, los registros y la base de datos" en discos físicos independientes está limitada en escenarios útiles.  Esto se debe a que el procedimiento recomendado de larga duración se basa en la suposición de que un "disco" es un eje dedicado y que se puede aislar la e/s.  Estas suposiciones que hacen que esto sea cierto ya no son relevantes con la introducción de:

- Nuevos tipos de almacenamiento y escenarios de almacenamiento compartido y virtual
- Ejes compartidos en una red de área de almacenamiento (SAN)
- Archivo VHD en un almacenamiento conectado a la red o SAN
- Unidades de estado sólido
- Arquitecturas de almacenamiento en capas (es decir, almacenamiento en caché de nivel de almacenamiento SSD mayor almacenamiento basado en el eje)

En Resumen, el objetivo final de todos los esfuerzos de rendimiento del almacenamiento, con independencia de la arquitectura y el diseño del almacenamiento subyacente, consiste en garantizar que la cantidad necesaria de operaciones de entrada/salida por segundo (IOPS) estén disponibles y que esas IOPS se produzcan dentro de un período de tiempo aceptable. . En esta sección se explica cómo evaluar qué AD DS demandas del almacenamiento subyacente para asegurarse de que las soluciones de almacenamiento están diseñadas correctamente.  Dada la variabilidad de las tecnologías de almacenamiento de hoy en día, es mejor trabajar con los proveedores de almacenamiento para garantizar una IOPS adecuada.  En el caso de los escenarios con almacenamiento conectado localmente, consulte el Apéndice C para obtener los conceptos básicos sobre cómo diseñar escenarios tradicionales de almacenamiento local.  Normalmente, estas entidades de seguridad se aplican a capas de almacenamiento más complejas y también ayudarán en el cuadro de diálogo con los proveedores que admiten soluciones de almacenamiento de back-end.

- Dada la amplia gama de opciones de almacenamiento disponibles, se recomienda participar en los conocimientos de los equipos de soporte técnico de hardware o de los proveedores para asegurarse de que la solución específica satisface las necesidades de AD DS. Los números siguientes son la información que se proporcionaría a los especialistas de almacenamiento.

En el caso de los entornos donde la base de datos es demasiado grande para almacenarse en RAM, use los contadores de rendimiento para determinar la cantidad de e/s que debe admitirse:

- LogicalDisk (\*) \Avg en segundos de disco/lectura (por ejemplo, si Ntds. dit está almacenado en D:/ , la ruta de acceso completa sería LogicalDisk (D:) \Avg de disco/lectura).
- LogicalDisk (\*) \Avg en segundos/escritura de disco
- LogicalDisk (\*) \Avg en segundos de disco/transferencia
- LogicalDisk (\*) \ lecturas/s
- LogicalDisk (\*) \ escrituras/s
- LogicalDisk (\*) \ transferencias/s

Se deben muestrear en intervalos de 15/30/60 minutos para realizar pruebas comparativas de las demandas del entorno actual.

#### <a name="evaluating-the-results"></a>Evaluación de los resultados

> [!NOTE]
> El enfoque se centra en las lecturas de la base de datos, ya que suele ser el componente más exigente, la misma lógica se puede aplicar a las escrituras en el archivo de registro mediante la sustitución del disco lógico LogicalDisk ( *\<Registro\>NTDS*) \Avg en segundos/escritura y LogicalDisk (*Registro\>NTDS) \ escrituras/seg.):\<*
>  
> - LogicalDisk ( *\<NTDS\>* ) \Avg en segundos/lectura indica si el tamaño del almacenamiento actual es el adecuado.  Si los resultados son aproximadamente iguales a la hora de acceso al disco para el tipo de disco, LogicalDisk ( *\<NTDS\>* ) \ lecturas/seg. es una medida válida.  Compruebe las especificaciones del fabricante para el almacenamiento en el back-end, pero los intervalos buenos para LogicalDisk ( *\<NTDS\>* ) \Avg Disk sec/Read serían aproximadamente:
>   - de 7200 a 12,5 milisegundos (MS)
>   - de 10.000 a 10 ms
>   - de 15.000 – 4 a 6 ms  
>   - SSD: de 1 a 3 MS  
>   - >[!NOTE]
>     >Existen recomendaciones que indican que el rendimiento del almacenamiento se degrada en 15ms a 20 ms (según el origen).  La diferencia entre los valores anteriores y la otra orientación es que los valores anteriores son el intervalo operativo normal.  Las otras recomendaciones son instrucciones para la solución de problemas con el fin de identificar cuándo la experiencia del cliente se degrada considerablemente y es apreciable.  Consulte el Apéndice C para obtener una explicación más detallada.
> - LogicalDisk ( *\<NTDS\>* ) \ lecturas por segundo es la cantidad de e/s que se está realizando.
>   - Si LogicalDisk ( *\<NTDS\>* ) \Avg Disk sec/Read está dentro del intervalo óptimo para el almacenamiento de back-end, se puede usar el LogicalDisk ( *\<NTDS\>* ) \ lecturas por segundo directamente para ajustar el almacenamiento.
>   - Si LogicalDisk ( *\<NTDS\>* ) \Avg Disk sec/Read no está dentro del intervalo óptimo para el almacenamiento de back-end, se necesita e/s adicional de acuerdo con la siguiente fórmula:
>     > (LogicalDisk ( *\<NTDS\>* ) \Avg en segundos de disco/ &divide; lectura) (tiempo de acceso al &times; disco del medio físico) (LogicalDisk ( *\<NTDS\>* ) \Avg de disco/lectura)

Consideraciones:

- Tenga en cuenta que si el servidor está configurado con una cantidad de memoria RAM poco óptima, estos valores serán inexactos para fines de planeación.  Se producirán erróneamente en el lado superior y se podrán seguir usando en el peor de los casos.
- La adición o optimización de la RAM específicamente impulsará una disminución en la cantidad de e/s de lectura (LogicalDisk ( *\<NTDS\>* ) \ lecturas/seg.  Esto significa que la solución de almacenamiento no tiene que ser tan robusta como la calculada inicialmente.  Desafortunadamente, todo lo más específico que esta instrucción general depende del entorno de la carga del cliente y no se pueden proporcionar instrucciones generales.  La mejor opción es ajustar el tamaño del almacenamiento después de optimizar la RAM.

#### <a name="virtualization-considerations-for-performance"></a>Consideraciones sobre la virtualización para el rendimiento

Al igual que todas las discusiones de virtualización anteriores, la clave aquí es asegurarse de que la infraestructura compartida subyacente puede admitir la carga del controlador de dominio más los demás recursos que usan el medio compartido subyacente y todas las rutas a él. Esto es cierto si un controlador de dominio físico está compartiendo el mismo medio subyacente en una infraestructura SAN, NAS o iSCSI que otros servidores o aplicaciones, tanto si es un invitado que usa acceso de paso a una infraestructura SAN, NAS o iSCSI que comparte el medios subyacentes, o si el invitado usa un archivo VHD que reside en medios compartidos de forma local o en una infraestructura SAN, NAS o iSCSI. El ejercicio de planeación consiste en asegurarse de que el medio subyacente puede admitir la carga total de todos los consumidores.

Además, desde una perspectiva de invitado, dado que hay rutas de acceso de código adicionales que deben atravesarse, hay un impacto en el rendimiento de tener que pasar por un host para tener acceso a cualquier almacenamiento. No sorprendentemente, las pruebas de rendimiento del almacenamiento indican que la virtualización tiene un impacto en el rendimiento que depende del uso del procesador del sistema host (consulte el Apéndice A: Criterios de ajuste de tamaño de la CPU), que se ve afectado por los recursos del host solicitado por el invitado. Esto contribuye a las consideraciones de virtualización con respecto a las necesidades de procesamiento en un escenario virtualizado (consulte [consideraciones sobre la virtualización para el procesamiento](#virtualization-considerations-for-processing)).

Hacer esto más complejo es que hay una variedad de opciones de almacenamiento diferentes que están disponibles y que tienen un impacto en el rendimiento diferente. Como una estimación segura al migrar de físico a virtual, use un multiplicador de 1,10 para ajustarse a diferentes opciones de almacenamiento de invitados virtualizados en Hyper-V, como el almacenamiento de paso a través, el adaptador SCSI o IDE. Los ajustes que deben realizarse al transferir entre distintos escenarios de almacenamiento son irrelevantes en lo que se refiere a si el almacenamiento es local, SAN, NAS o iSCSI.

#### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

Determinar la cantidad de e/s necesaria para un sistema en buen estado en condiciones de funcionamiento normales:

- LogicalDisk ( *\<unidad\>de base de datos Ntds*) \ transferencias/s durante el período máximo de 15 minutos 
- Para determinar la cantidad de e/s necesaria para el almacenamiento donde se supera la capacidad del almacenamiento subyacente:
  >*IOPS necesarios* = (LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg en &divide; segundos/ &times; *\<lectura destino promedio de\>segundos de disco/lectura*) LogicalDisk ( *\< NTDS (unidad\>de base de datos*) \ lectura/s

|Contador|Valor|
|-|-|
|Real LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg en segundos/transferencia|.02 segundos (20 milisegundos)|
|Destino LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg de disco/transferencia|.01 segundos|
|Multiplicador para cambio en la e/s disponible|0,02 &divide; 0,01 = 2|  
  
|Nombre de valor|Valor|
|-|-|
|LogicalDisk ( *\<unidad\>de base de datos Ntds*) \ transferencias/s|400|
|Multiplicador para cambio en la e/s disponible|2|
|IOPS totales necesarias durante el período de máxima actividad|800|

Para determinar la velocidad a la que se desea que la memoria caché se caliente:

- Determine el tiempo máximo aceptable para la caché. Es la cantidad de tiempo que debe tardar en cargar la base de datos completa desde el disco, o en escenarios donde no se puede cargar la base de datos completa en la memoria RAM, lo que sería el tiempo máximo para llenar la RAM.
- Determine el tamaño de la base de datos, sin incluir espacios en blanco.  Para obtener más información, consulte [evaluar para almacenamiento](#evaluating-for-storage).  
- Divida el tamaño de la base de datos entre 8 KB; Este será el total de IOs necesario para cargar la base de datos.
- Divida el total de e/s por el número de segundos del período de tiempo definido.

Tenga en cuenta que la tasa calculada, aunque sea precisa, no será exacta porque las páginas cargadas previamente se expulsan si ESE no está configurado para tener un tamaño de caché fijo y AD DS utiliza de forma predeterminada el tamaño de la memoria caché variable.

|Puntos de datos que se van a recopilar|Valores
|-|-|
|Tiempo máximo aceptable para calor|10 minutos (600 segundos)
|Tamaño de la base de datos|2 GB|  
  
|Paso de cálculo|Fórmula|Resultado|
|-|-|-|
|Calcular el tamaño de la base de datos en páginas|(2 GB &times; 1024 &times; 1024) = *tamaño de la base de datos en KB*|2\.097.152 KB|
|Calcular el número de páginas de la base de datos|2\.097.152 KB &divide; 8 KB = *número de páginas*|262.144 páginas|
|Calcular IOPS necesarias para hacer que la memoria caché esté completamente caliente|262.144 páginas &divide; 600 segundos = *IOPS necesarias*|437 IOPS|

## <a name="processing"></a>En proceso

### <a name="evaluating-active-directory-processor-usage"></a>Evaluación del uso del procesador Active Directory

Para la mayoría de los entornos, una vez que el almacenamiento, la memoria RAM y las redes están correctamente optimizados, tal y como se describe en la sección planeación, la administración de la cantidad de capacidad de procesamiento será el componente que merece más atención. Hay dos desafíos en la evaluación de la capacidad de la CPU necesaria:

- Si las aplicaciones del entorno se están comportando correctamente en una infraestructura de servicios compartidos y se describe en la sección titulada "búsquedas costosas e ineficaces" en el artículo creación de Microsoft Active más eficaz Aplicaciones habilitadas para el directorio o migración desde llamadas de SAM de nivel inferior a llamadas LDAP.  
  
  En entornos más grandes, la razón por la que esto es importante es que las aplicaciones codificadas de forma deficiente pueden impulsar la volatilidad en la carga de la CPU, "robar" una cantidad de tiempo de CPU injustificada de otras aplicaciones, impulsar artificialmente las necesidades de capacidad y distribuir la carga de forma desequilibrada los controladores de DC.  
- Como AD DS es un entorno distribuido con una gran variedad de clientes potenciales, la estimación del gasto de un "cliente único" es subjetiva en el entorno debido a los patrones de uso y el tipo o la cantidad de aplicaciones que aprovechan AD DS. En Resumen, de forma muy similar a la sección de redes, para una aplicabilidad amplia, este enfoque es mejor desde la perspectiva de evaluar la capacidad total necesaria en el entorno.

En el caso de los entornos existentes, a medida que se analizó el tamaño del almacenamiento, se supone que el almacenamiento tiene el tamaño adecuado y, por tanto, los datos relativos a la carga del procesador son válidos. Para reiterarse, es fundamental asegurarse de que el cuello de botella del sistema no es el rendimiento del almacenamiento. Cuando existe un cuello de botella y el procesador está esperando, hay Estados inactivos que desaparecerán una vez que se quite el cuello de botella.  Como los Estados de espera del procesador se quitan, por definición, el uso de la CPU aumenta porque ya no tiene que esperar en los datos. Por lo tanto, recopile los contadores de rendimiento "disco lógico ( *\<unidad\>de base de datos Ntds*) \Avg en segundos/lectura\\" y "proceso (LSASS)% de tiempo de procesador". Los datos de "proceso (LSASS)\\% de tiempo de procesador" serán artificialmente bajos si "disco lógico ( *\<unidad\>de base de datos Ntds*) \Avg segundos/lectura" es superior a 10 o 15 ms, que es un umbral general que el soporte técnico de Microsoft usa para solucionar problemas de rendimiento relacionados con el almacenamiento. Como antes, se recomienda que los intervalos de muestra sean 15, 30 o 60 minutos. Todo menos normalmente será demasiado volátil para las mediciones correctas; cualquier valor mayor suavizará las búsquedas diarias de forma excesiva.

### <a name="introduction"></a>Introducción

Para planear la planeación de la capacidad de los controladores de dominio, la capacidad de procesamiento requiere la mayor atención y comprensión. Al cambiar el tamaño de los sistemas para garantizar el máximo rendimiento, siempre hay un componente que es el cuello de botella y en un controlador de dominio con el tamaño adecuado. este será el procesador.

De forma similar a la sección de redes en la que se revisa la demanda del entorno de manera individual, se debe hacer lo mismo para la capacidad de proceso solicitada. A diferencia de la sección redes, donde las tecnologías de red disponibles superan la demanda normal, preste más atención para ajustar el tamaño de la capacidad de la CPU.  Como cualquier entorno de un tamaño uniforme; cualquier cosa en unos cuantos miles de usuarios simultáneos puede suponer una carga significativa en la CPU.

Desafortunadamente, debido a la enorme variabilidad de las aplicaciones cliente que aprovechan AD, una estimación general de los usuarios por CPU es woefully inaplicable a todos los entornos. En concreto, las demandas de proceso están sujetas al comportamiento del usuario y al perfil de aplicación. Por lo tanto, cada entorno debe tener un tamaño individual.

#### <a name="target-site-behavior-profile"></a>Perfil de comportamiento del sitio de destino

Como se mencionó anteriormente, al planear la capacidad de un sitio completo, el objetivo es destinar un diseño con un diseño de capacidad *N* + 1, de modo que el error de un sistema durante el período máximo permitirá la continuación del servicio con un nivel de calidad razonable. Esto significa que en un escenario "*N*", la carga en todos los cuadros debe ser inferior al 100% (mejor aún, menor que 80%). durante los períodos de máxima actividad.

Además, si las aplicaciones y los clientes del sitio usan los procedimientos recomendados para buscar controladores de dominio (es decir, con la [función DsGetDcName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), los clientes deben distribuirse relativamente uniformemente con picos transitorios menores debidos a cualquier número de factores.

En el ejemplo siguiente se realizan las suposiciones siguientes:

- Cada uno de los cinco controladores de DC del sitio tiene cuatro CPU.
- El uso total de la CPU de destino durante el horario comercial es del 40% en condiciones de funcionamiento normales ("*n* + 1") y 60% de lo contrario ("*n*"). Durante el horario no comercial, el uso de la CPU de destino es del 80% porque se espera que el software de copia de seguridad y otros mantenimiento consuman todos los recursos disponibles.

![Gráfico de uso de CPU](media/capacity-planning-considerations-cpu-chart.png)

Analizando los datos del gráfico (utilidad de procesador de información\% de procesador (_ total)) para cada uno de los controladores de DC:

- En su mayor parte, la carga se distribuye relativamente uniformemente, que es lo que cabría esperar cuando los clientes usan el ubicador de DC y tienen búsquedas bien escritas. 
- Hay una serie de picos de cinco minutos del 10%, con un tamaño de hasta un 20%. Por lo general, a menos que hagan que se supere el destino del plan de capacidad, investigarlos no merece la pena.  
- El período máximo para todos los sistemas está entre aproximadamente 8:00 AM y 9:15 AM. Con una transición suave de aproximadamente 5:00 a aproximadamente 5:00 PM, suele ser indicativo del ciclo comercial. Los picos más aleatorios del uso de CPU en un escenario de caja por cuadro entre 5:00 PM y 4:00 AM estarán fuera de los problemas de planeación de capacidad.

  >[!NOTE]
  >En un sistema bien administrado, los picos pueden ser software de copia de seguridad en ejecución, exámenes completos del antivirus del sistema, inventario de hardware o software, implementación de software o revisión, etc. Dado que se encuentran fuera del ciclo de negocio de usuario máximo, los destinos no se superan.

- Dado que cada sistema es aproximadamente del 40% y todos los sistemas tienen el mismo número de CPU, en caso de que se produzca un error o se desconecte, los sistemas restantes se ejecutarán a un 53% Estimado (el 40% de la carga del sistema D) se divide uniformemente y se agrega al sistema A y a la carga de 40% del sistema C. Por varias razones, esta suposición lineal no es perfectamente precisa, pero proporciona una precisión suficiente para el medidor.  

  **Escenario alternativo:** Dos controladores de dominio que ejecutan en el 40%: Se produce un error en un controlador de dominio, la CPU calculada en el resto sería una estimación del 80%. En este momento se superan los umbrales descritos anteriormente para el plan de capacidad y también se empieza a limitar gravemente la cantidad de espacio para el 10% al 20% que se ve en el perfil de carga anterior, lo que significa que los picos irían al controlador de dominio al 90% al 100% durante el escenario "*N*" y de la capacidad de respuesta se degrada finita.

### <a name="calculating-cpu-demands"></a>Calcular las demandas de la CPU

El contador del\\objeto de rendimiento "porcentaje de tiempo de procesador" suma la cantidad total de tiempo que todos los subprocesos de una aplicación invierten en la CPU y divide por la cantidad total de tiempo del sistema que ha transcurrido. El efecto de esto es que una aplicación multiproceso en un sistema de varias CPU puede superar el 100% de tiempo de CPU y se interpretaría de manera muy diferente que la\\"utilidad de procesador% de información de procesador". En la práctica, el "proceso (\\LSASS)% de tiempo de procesador" se puede ver como el recuento de las CPU que se ejecutan en el 100% que son necesarias para admitir las demandas del proceso. Un valor de 200% significa que se necesitan 2 CPU, cada una a 100%, para admitir la carga AD DS completa. Aunque una CPU que se ejecuta con una capacidad del 100% es la más rentable desde la perspectiva del dinero empleado en las CPU y el consumo energético y energético, por una serie de motivos que se detallan en el Apéndice A, se produce una mejor capacidad de respuesta en un sistema de varios subprocesos cuando el sistema está no se ejecuta en el 100%.

Para dar cabida a picos transitorios en la carga de cliente, se recomienda establecer como destino un período de CPU de un período de tiempo máximo entre el 40% y el 60% de la capacidad del sistema. Al trabajar con el ejemplo anterior, esto significa que se necesitarían las CPU entre 3,33 (60% de destino) y 5 (40% de destino) para la carga de AD DS (proceso Lsass). Se debe agregar capacidad adicional en según las demandas del sistema operativo base y otros agentes necesarios (como antivirus, copia de seguridad, supervisión, etc.). Aunque es necesario evaluar el impacto de los agentes por cada entorno, se puede realizar una estimación entre el 5 y el 10% de una sola CPU. En el ejemplo actual, esto sugeriría que se necesitan entre las CPU 3,43 (60% Target) y 5,1 (40% Target) durante los períodos de máxima actividad.

La forma más fácil de hacerlo es usar la vista "área apilada" en el monitor de confiabilidad y rendimiento de Windows (PerfMon), asegurándose de que todos los contadores se escalen de la misma manera.

Se asume que:

- El objetivo es reducir el tamaño de los servidores que sea posible. Idealmente, un servidor llevaría la carga y se agregaría un servidor adicional para la redundancia (escenario *N* + 1).

![Gráfico de tiempo de procesador para el proceso Lsass (en todos los procesadores)](media/capacity-planning-considerations-proc-time-chart.png)

Conocimiento obtenido de los datos del gráfico (proceso (LSASS)\\% de tiempo de procesador):

- El día laborable comienza la rampa en torno a 7:00 y disminuye a las 5:00 PM.
- El período de picos más ocupado es de 9:30 AM a 11:00 AM. 
  > [!NOTE]
  > Todos los datos de rendimiento son históricos. El punto de datos máximo en 9:15 indica la carga de 9:00 a 9:15.
- Hay picos anteriores a 7:00 AM que podrían indicar la carga de distintas zonas horarias o la actividad de la infraestructura en segundo plano, como las copias de seguridad. Dado que el pico en 9:30 AM supera esta actividad, no es relevante.
- Hay tres controladores de dominio en el sitio.

En la carga máxima, LSASS consume aproximadamente el 485% de una CPU o 4,85 CPU que se ejecutan en el 100%. Según la matemática anterior, esto significa que el sitio necesita aproximadamente 12,25 CPU para AD DS. Agregue las sugerencias anteriores del 5% al 10% para los procesos en segundo plano y eso significa que el reemplazo del servidor en la actualidad necesitaría aproximadamente de 12,30 a 12,35 CPU para admitir la misma carga. Ahora es necesario calcular una estimación del entorno para el crecimiento.

### <a name="when-to-tune-ldap-weights"></a>Cuándo optimizar pesos LDAP

Hay varios escenarios en los que se debe tener en cuenta la optimización de [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) . En el contexto del planeamiento de la capacidad, esto se haría cuando las cargas de la aplicación o del usuario no se equilibran uniformemente, o los sistemas subyacentes no están equilibrados uniformemente en lo que respecta a la capacidad. Los motivos para hacerlo más allá del planeamiento de la capacidad están fuera del ámbito de este artículo.

Hay dos razones comunes para optimizar pesos LDAP:

- El emulador de PDC es un ejemplo que afecta a todos los entornos para los que el comportamiento de carga de usuarios o aplicaciones no se distribuye uniformemente. Como algunas herramientas y acciones tienen como destino el emulador de PDC, como las herramientas de administración de directiva de grupo, los segundos intentos en caso de errores de autenticación, el establecimiento de confianza, etc., los recursos de CPU en el emulador de PDC pueden ser más demandados que en cualquier otra parte de el sitio.
  - Solo es útil ajustar esto si hay una diferencia notable en el uso de la CPU para reducir la carga en el emulador de PDC y aumentar la carga en otros controladores de dominio permitirá una distribución más uniforme de la carga.
  - En este caso, establezca LDAPSrvWeight entre 50 y 75 para el emulador de PDC.
- Servidores con recuentos diferentes de CPU (y velocidades) en un sitio.  Por ejemplo, suponga que hay servidores de 2 8 y un servidor de 1 4-Core.  El último servidor tiene la mitad de los procesadores de los otros dos servidores.  Esto significa que una carga de cliente bien distribuida aumentará el promedio de carga de la CPU en el cuadro de cuatro núcleos hasta aproximadamente el doble de los dos cuadros de ocho núcleos.
  - Por ejemplo, los cuadros 2 8-Core se ejecutarán en el 40% y el cuadro de cuatro núcleos se ejecutaría en el 80%.
  - Además, tenga en cuenta el impacto de la pérdida del cuadro 1 8-Core en este escenario, en concreto el hecho de que el cuadro de cuatro núcleos se sobrecargará ahora.

#### <a name="example-1---pdc"></a>Ejemplo 1: PDC

| |Uso con valores predeterminados|Nuevo LdapSrvWeight|Nueva utilización estimada|
|-|-|-|-|
|DC 1 (emulador de PDC)|53%|57|40 %|
|DC 2|33%|100|40 %|
|DC 3|33%|100|40 %|

La siguiente instrucción es que si el rol de emulador de PDC se transfiere o se Asumi, especialmente en otro controlador de dominio del sitio, habrá un aumento espectacular en el nuevo emulador de PDC.

Con el ejemplo de la sección [Perfil de comportamiento del sitio de destino](#target-site-behavior-profile), se supuso que los tres controladores de dominio del sitio tenían cuatro CPU. ¿Qué debe ocurrir en condiciones normales, si uno de los controladores de dominio tuviera ocho CPU? Debería haber dos controladores de dominio con un uso del 40% y uno con un 20% de uso. Aunque esto no es malo, existe la posibilidad de equilibrar la carga un poco mejor. Aproveche los pesos de LDAP para lograr esto.  Un escenario de ejemplo sería:

#### <a name="example-2---differing-cpu-counts"></a>Ejemplo 2: recuentos de CPU diferentes

| |Utilidad procesador\\de información %&nbsp;del procesador (_ total)<br />Uso con valores predeterminados|Nuevo LdapSrvWeight|Nueva utilización estimada|
|-|-|-|-|
|4-CPU DC 1|40|100|30 %|
|4-CPU DC 2|40|100|30 %|
|8-CPU DC 3|20|200|30 %|

No obstante, tenga cuidado con estos escenarios. Como se puede observar más arriba, la expresión matemática es realmente agradable y está bien en papel. Pero a lo largo de este artículo, la planeación de un escenario "*N* + 1" es de primordial importancia. Se debe calcular el impacto de un DC sin conexión para cada escenario. En el escenario inmediatamente anterior, en el que la distribución de la carga es uniforme, para garantizar una carga del 60% durante un escenario "*N*", con la carga equilibrada uniformemente entre todos los servidores, la distribución será suficiente para que las proporciones sean coherentes. Observando el escenario de optimización del emulador de PDC y, en general, cualquier escenario en el que la carga de usuarios o aplicaciones esté desequilibrada, el efecto es muy diferente:

| |Uso optimizado|Nuevo LdapSrvWeight|Nueva utilización estimada|
|-|-|-|-|
|DC 1 (emulador de PDC)|40 %|85|47%|
|DC 2|40 %|100|53%|
|DC 3|40 %|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Consideraciones sobre la virtualización para el procesamiento

Hay dos niveles de planeación de capacidad que deben realizarse en un entorno virtualizado. En el nivel de host, de forma similar a la identificación del ciclo de negocio descrito anteriormente para el procesamiento del controlador de dominio, es necesario identificar los umbrales durante el período máximo. Dado que las entidades de seguridad subyacentes son las mismas para un equipo host que programa los subprocesos invitados en la CPU como para obtener AD DS subprocesos en la CPU de un equipo físico, se recomienda el mismo objetivo de 40% a 60% en el host subyacente. En el siguiente nivel, el nivel de invitado, dado que las entidades de seguridad de la programación de subprocesos no han cambiado, el objetivo del invitado permanece en el intervalo del 40% al 60%.

En un escenario de asignación directa, un invitado por host, todo el planeamiento de la capacidad realizado hasta este punto debe agregarse a los requisitos (RAM, disco, red) del sistema operativo del host subyacente. En un escenario de host compartido, las pruebas indican que hay un 10% de impacto en la eficiencia de los procesadores subyacentes. Esto significa que si un sitio necesita 10 CPU en un destino del 40%, la cantidad recomendada de CPU virtuales que se asignará a todos los invitados "*N*" sería 11. En un sitio con una distribución mixta de servidores físicos y servidores virtuales, el modificador solo se aplica a las máquinas virtuales. Por ejemplo, si un sitio tiene un escenario "*N* + 1", un servidor físico o asignado directamente con 10 CPU sería equivalente a un invitado con 11 CPU en un host, con 11 CPU reservadas para el controlador de dominio.

A lo largo del análisis y el cálculo de las cantidades de CPU necesarias para admitir la carga AD DS, los números de CPU que se asignan a lo que se puede adquirir en términos hardware físico no se asignan correctamente. La virtualización elimina la necesidad de redondear. La virtualización reduce el esfuerzo necesario para agregar capacidad de proceso a un sitio, dada la facilidad con la que se puede Agregar una CPU a la máquina virtual. No elimina la necesidad de evaluar con precisión la potencia de proceso necesaria para que el hardware subyacente esté disponible cuando sea necesario agregar CPU adicionales a los invitados.  Como siempre, recuerde planear y supervisar el crecimiento en la demanda.

### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

|Sistema|CPU máxima|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|DC 3|218%|
|CPU total usada|485%|  
  
|Recuento de sistemas de destino|Ancho de banda total (anterior)|
|-|-|
|CPU necesarias en el destino del 40%|4,85 &divide; . 4 = 12,25|

Si se repite debido a la importancia de este punto, *Recuerde planear el crecimiento*. Suponiendo un crecimiento del 50% en los próximos tres años, este entorno necesitará 18,375 de &times; CPU (12,25 1,5) en el marcado de tres años. Un plan alternativo sería revisar después del primer año y agregar capacidad adicional según sea necesario.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carga de autenticación de cliente entre confianza para NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Evaluación de la carga de autenticación del cliente entre relaciones

Muchos entornos pueden tener uno o varios dominios conectados mediante una confianza. Una solicitud de autenticación para una identidad en otro dominio que no usa la autenticación Kerberos debe atravesar una confianza mediante el canal seguro del controlador de dominio a otro controlador de dominio en el dominio de destino o en el siguiente dominio de la ruta de acceso. al dominio de destino. El número de llamadas simultáneas que usan el canal seguro que un controlador de dominio puede realizar en un controlador de dominio en un dominio de confianza se controla mediante una configuración conocida como **MaxConcurrentAPI**. En el caso de los controladores de dominio, asegurarse de que el canal seguro puede controlar la cantidad de carga se logra mediante uno de estos dos enfoques: optimizar **MaxConcurrentAPI** o, dentro de un bosque, crear confianzas de acceso directo. Para medir el volumen de tráfico a través de una relación de confianza individual, consulte [Cómo realizar el ajuste del rendimiento para la autenticación NTLM mediante la configuración MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante la recopilación de datos, esto, al igual que con todos los demás escenarios, se debe recopilar durante los períodos de máxima actividad del día para que los datos sean útiles.

> [!NOTE]
> Los escenarios entre bosques e interbosques pueden provocar que la autenticación atraviese varias confianzas y que cada fase debe ajustarse.

#### <a name="planning"></a>Planificación

Hay una serie de aplicaciones que usan la autenticación NTLM de forma predeterminada o la usan en un escenario de configuración determinado. Los servidores de aplicaciones aumentan la capacidad y el servicio de un número cada vez mayor de clientes activos. También hay una tendencia que los clientes mantienen las sesiones abiertas durante un tiempo limitado y, en su lugar, se vuelven a conectar de forma regular (como la sincronización de extracción de correo electrónico). Otro ejemplo común para la carga de NTLM alta son los servidores proxy web que requieren autenticación para el acceso a Internet.

Estas aplicaciones pueden producir una carga significativa para la autenticación NTLM, lo que puede suponer una gran sobrecarga en los controladores de dominio, especialmente cuando los usuarios y los recursos se encuentran en dominios diferentes.

Hay varios métodos para administrar la carga de confianza cruzada, que en la práctica se usan en conjunto en lugar de en un escenario exclusivo de/o. Las opciones posibles son:

- Reduzca la autenticación de cliente entre relaciones localizando los servicios que un usuario consume en el mismo dominio en el que reside el usuario.
- Aumente el número de canales seguros disponibles. Esto es relevante para el tráfico entre bosques y bosques, y se conoce como confianzas abreviadas.
- Ajuste la configuración predeterminada de **MaxConcurrentAPI**.

Para optimizar **MaxConcurrentAPI** en un servidor existente, la ecuación es:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires*  +  *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Para obtener más información, [consulte el artículo de KB 2688798: Cómo optimizar el rendimiento de la autenticación NTLM mediante el uso de la](http://support.microsoft.com/kb/2688798)configuración MaxConcurrentApi.

## <a name="virtualization-considerations"></a>Consideraciones de virtualización

Ninguno, se trata de una configuración de optimización del sistema operativo.

### <a name="calculation-summary-example"></a>Ejemplo de Resumen de cálculo

|Tipo de datos|Valor|
|-|-|
|El semáforo adquiere (mínimo)|6\.161|
|El semáforo adquiere (máximo)|6\.762|
|Tiempos de espera de semáforos|0|
|Tiempo medio de espera del semáforo|0,012|
|Duración de la recopilación (segundos)|1:11 minutos (71 segundos)|
|Fórmula (desde KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0,012/|
|Valor mínimo de **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0,012 &divide; 71 =. 101|

Para este sistema durante este período de tiempo, los valores predeterminados son aceptables.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Supervisión del cumplimiento de los objetivos de planeamiento de capacidad

En este artículo, se ha explicado que la planeación y el escalado van hacia los objetivos de uso. Este es un gráfico de Resumen de los umbrales recomendados que deben supervisarse para asegurarse de que los sistemas funcionan con los umbrales de capacidad adecuados. Tenga en cuenta que estos no son umbrales de rendimiento, sino umbrales de planeamiento de capacidad. Un servidor que opere en exceso de estos umbrales funcionará, pero es el momento de iniciar la validación de que todas las aplicaciones están bien compartidas. Si estas aplicaciones están bien compartidas, es el momento de empezar a evaluar las actualizaciones de hardware u otros cambios de configuración.

|Category|Contador de rendimiento|Intervalo/muestreo|Destino|Advertencia|
|-|-|-|-|-|
|Procesador|Información del procesador (_\\total)% utilidad del procesador|60 mín.|40 %|60%|
|RAM (Windows Server 2008 R2 o versiones anteriores)|Memoria\mbytes MB|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|Duración media de la caché en espera Memory\Long-Term (s)|30 minutos|Se debe probar|Se debe probar|
|Red|Interfaz de red\*() \Bytes enviados/seg.<br /><br />Interfaz de red\*() \Bytes recibidos/seg.|30 minutos|40 %|60%|
|Almacenamiento|LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg en segundos/lectura<br /><br />LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg en segundos/escritura|60 mín.|10 ms|15 ms|
|Servicios de AD|\Average de\*tiempo de espera del semáforo de Netlogon ()|60 mín.|0|1 segundo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Apéndice A: Criterios de ajuste de tamaño de la CPU

### <a name="definitions"></a>Definiciones

**Procesador (microprocesador):** componente que lee y ejecuta instrucciones de programa.  

**CPU:** Unidad de procesamiento central  

**Procesador de varios núcleos:** varias CPU en el mismo circuito integrado  

Varias **CPU:** varias CPU, no en el mismo circuito integrado  

**Procesador lógico:** un motor de proceso lógico desde la perspectiva del sistema operativo  

Esto incluye Hyper-threaded, un núcleo en el procesador de varios núcleos o un procesador de un solo núcleo.  

Como los sistemas de servidor de hoy en día tienen varios procesadores, varios procesadores de varios núcleos y Hyper-Threading, esta información se generaliza para cubrir ambos escenarios. Como tal, se usará el término procesador lógico, ya que representa la perspectiva del sistema operativo y de la aplicación de los motores informáticos disponibles.

### <a name="thread-level-parallelism"></a>Paralelismo en el nivel de subprocesos

Cada subproceso es una tarea independiente, ya que cada subproceso tiene su propia pila e instrucciones. Dado que AD DS es multiproceso y el número de subprocesos disponibles se puede optimizar mediante el uso de [Cómo ver y establecer la Directiva LDAP en Active Directory mediante el uso de Ntdsutil. exe](http://support.microsoft.com/kb/315071), se escala bien en varios procesadores lógicos.

### <a name="data-level-parallelism"></a>Paralelismo de nivel de datos

Esto implica compartir datos entre varios subprocesos dentro de un proceso (en el caso del AD DS proceso por sí solo) y entre varios subprocesos en varios procesos (en general). Con interés en reducir la rentabilidad, esto significa que cualquier cambio realizado en los datos se reflejará en todos los subprocesos en ejecución en todos los niveles de caché (L1, L2 y L3) en todos los núcleos que ejecuten los subprocesos, así como la actualización de la memoria compartida. El rendimiento puede reducirse durante las operaciones de escritura, mientras que las distintas ubicaciones de memoria se componen de forma coherente antes de que el procesamiento de instrucciones pueda continuar.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Velocidad de la CPU frente a consideraciones de varios núcleos

La regla general es que los procesadores lógicos más rápidos reducen la duración que se tarda en procesar una serie de instrucciones, mientras que un mayor número de procesadores lógicos significa que se pueden ejecutar más tareas al mismo tiempo. Estas reglas de Thumb se dividen a medida que los escenarios pasan a ser intrínsecamente más complejos con consideraciones sobre la obtención de datos de la memoria compartida, la espera del paralelismo de nivel de datos y la sobrecarga de administrar varios subprocesos. Esta es también la razón por la que la escalabilidad en sistemas de varios núcleos no es lineal.

Tenga en cuenta las siguientes analogías en estas consideraciones: Piense en una autopista, donde cada subproceso es un coche individual, cada calle es un núcleo y el límite de velocidad es la velocidad del reloj.

1. Si solo hay un automóvil en la autopista, no importa si hay dos calles o 12 calles. Ese coche solo va a ser tan rápido como lo permitirá el límite de velocidad.
1. Supongamos que los datos que necesita el subproceso no están disponibles de inmediato. La analogía sería que un segmento de carretera está apagado. Si solo hay un automóvil en la autopista, no importa cuál es el límite de velocidad hasta que se vuelve a abrir la calle (los datos se capturan de la memoria).
1. A medida que aumenta el número de automóviles, aumenta la sobrecarga que supone administrar el número de automóviles. Compare la experiencia de conducción y la cantidad de atención necesaria cuando el viaje está prácticamente vacío (por ejemplo, por la tarde) frente a que el tráfico es pesado (por ejemplo, la tarde, pero no la hora de prisa). Además, tenga en cuenta la cantidad de atención necesaria a la hora de conducir en una autopista de dos carriles, donde solo hay otro carril para preocuparse de lo que hacen los controladores, en lugar de una autopista de seis carriles en la que hay que preocuparse de lo que hacen muchos otros controladores.
   > [!NOTE]
   > La analogía sobre el escenario de hora de prisa se extiende en la sección siguiente: Tiempo de respuesta/cómo afecta la ocupación del sistema al rendimiento.

Como resultado, los detalles sobre los procesadores más o más rápidos pasan a ser muy subjetivos para el comportamiento de la aplicación, que en el caso de AD DS es muy específico del entorno e incluso varía entre el servidor y el servidor dentro de un entorno. Esta es la razón por la que las referencias que se incluyen anteriormente en el artículo no invierten en gran medida en la precisión y se incluye un margen de seguridad en los cálculos. Al tomar decisiones de compra basadas en presupuestos, se recomienda que la optimización del uso de los procesadores en el 40% (o el número deseado para el entorno) se produzca en primer lugar, antes de considerar la compra de procesadores más rápidos. El aumento de la sincronización entre más procesadores reduce la ventaja real de más procesadores de la progresión&times; lineal (2 el número de procesadores proporciona&times; menos de 2 potencia de proceso adicional disponible).

> [!NOTE]
> La ley de Amdahl y la ley de Gustafson son los conceptos relevantes aquí.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tiempo de respuesta/cómo afecta la ocupación del sistema al rendimiento

La teoría de colas es el estudio matemático de las líneas de espera (colas). En la teoría de la puesta en cola, la ley de uso se representa con la ecuación:

*U* k = *B* &divide; *T*

Donde *U* k es el porcentaje de uso, *B* es la cantidad de tiempo ocupado y *T* es el tiempo total que se observó el sistema. Convertido en el contexto de Windows, es decir, el número de subprocesos de intervalo de 100-nanosegundos (NS) que están en estado de ejecución dividido por el número de intervalos de 100-NS que estaban disponibles en el intervalo de tiempo dado. Esta es exactamente la fórmula para calcular la utilidad del procesador de% (referencia del [objeto de procesador](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) y [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

La teoría de puesta en cola también proporciona la fórmula: *N*  = *u* k &divide; (1 &ndash; *u* k) para calcular el número de elementos en espera en función del uso (*N* es la longitud de la cola). En el gráfico de este procedimiento sobre todos los intervalos de uso se proporciona el siguiente cálculo de cuánto tiempo la cola se debe obtener en el procesador en cualquier carga de CPU determinada.

![Longitud de cola](media/capacity-planning-considerations-queue-length.png)

Se observa que después del 50% de la carga de la CPU, como promedio, siempre hay una espera de otro elemento de la cola, con un aumento notablemente rápido tras aproximadamente el 70% de uso de la CPU.

Volviendo a la analogía de conducción utilizada anteriormente en esta sección:

- Los tiempos de inactividad de la "mitad de la tarde", hipotéticamente, entrarán en un intervalo del 40% al 70%. Hay suficiente tráfico, de modo que una capacidad de elegir cualquier calle no se restringe de forma principal, y la posibilidad de que otro controlador esté en el camino, mientras que el nivel alto, no requiere que el nivel de esfuerzo "encuentre" un espacio seguro entre otros automóviles de la carretera.
- Uno observará que cuando el tráfico se aproxima a la hora de la mañana, el sistema de carreteras se aproxima a la capacidad del 100%. Cambiar las calles puede llegar a ser muy desafiante, ya que los automóviles están tan próximos que se debe hacer con mayor precaución.

Este es el motivo por el que los promedios a largo plazo de la capacidad estimada con cautela en el 40% permiten la sala principal para picos anómalos en la carga, ya se trate de picos transitorios (por ejemplo, consultas mal codificadas que se ejecutan durante unos minutos) o ráfagas anómalas en la carga general (la mañana de el primer día después de un fin de semana largo).

La instrucción anterior en relación con el cálculo del porcentaje de tiempo de procesador es la misma que la ley de uso es un poco de una simplificación para la facilidad del lector general. Para aquellos más rigurosamente rigurosos:  
- Traducción de [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = el número de subprocesos de 100-NS "inactivo" emplea en el procesador lógico. El cambio en la variable "*X*" en el cálculo de [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *T* = el número total de intervalos 100-NS en un intervalo de tiempo determinado. El cambio en la variable "*Y*" en el cálculo de [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) .
  - *U* k = el porcentaje de uso del procesador lógico en el "subproceso inactivo" o en el% de tiempo de inactividad.  
- Cómo trabajar con las matemáticas:
  - *U* k = 1:% de tiempo de procesador
  - % De tiempo de procesador = 1 – *U* k
  - % De tiempo de procesador = 1 – *B* / *T*
  - % De tiempo de procesador = 1 – *x1* – *x0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Aplicar los conceptos al planeamiento de capacidad

Las matemáticas anteriores pueden hacer que las determinaciones sobre el número de procesadores lógicos necesarios en un sistema parezcan abrumadoramente complejas. Esta es la razón por la que el enfoque para el ajuste de tamaño de los sistemas se centra en determinar el uso de destino máximo en función de la carga actual y calcular el número de procesadores lógicos que se necesitan para llegar allí. Además, aunque las velocidades de procesador lógicos tendrán un impacto significativo en el rendimiento, la eficacia de la memoria caché, los requisitos de coherencia de memoria, la programación y sincronización de subprocesos, y la carga de clientes equilibrada de forma imperfecta tendrá un impacto significativo en rendimiento que variará en función del servidor. Con el costo relativamente económico de la potencia de proceso, el intento de analizar y determinar el número perfecto de CPU necesario es más un ejercicio académico del que proporciona el valor empresarial.

40 por ciento no es un requisito difícil y rápido, es un inicio razonable. Varios consumidores de Active Directory requieren varios niveles de capacidad de respuesta. Puede haber escenarios en los que los entornos se ejecuten con una utilización del 80% o 90% como promedio sostenido, ya que el aumento de los tiempos de espera para el acceso al procesador no afectará de forma notable al rendimiento del cliente. Es importante volver a iterar que hay muchas áreas en el sistema que son mucho más lentas que el procesador lógico del sistema, incluido el acceso a la RAM, el acceso al disco y la transmisión de la respuesta a través de la red. Todos estos elementos deben ajustarse conjuntamente. Ejemplos:

- Agregar más procesadores a un sistema que ejecuta el 90% que está enlazado a disco probablemente no va a mejorar significativamente el rendimiento. Un análisis más profundo del sistema probablemente identificará una gran cantidad de subprocesos que no se están obteniendo siquiera en el procesador porque están esperando a que se complete la e/s.
- La resolución de los problemas enlazados a disco puede significar que los subprocesos que antes gastaron mucho tiempo en un estado de espera dejarán de estar en un estado de espera de e/s y habrá más competencia para el tiempo de CPU, lo que significa que el uso del 90% en el anterior el ejemplo irá al 100% (porque no puede ir más alto). Ambos componentes deben ajustarse conjuntamente.
  > [!NOTE]
  > Información del procesador (*\\)% la utilidad del procesador puede superar el 100% con los sistemas que tienen un modo "Turbo".  Aquí es donde la CPU supera la velocidad de procesador recomendada durante breves períodos.  Consulte la documentación de los fabricantes de CPU y la descripción del contador para obtener más información.  

La explicación de las consideraciones de uso de todo el sistema también incorpora los controladores de dominio de conversación como invitados virtualizados. [Tiempo de respuesta/cómo afecta la ocupación del sistema al rendimiento](#response-timehow-the-system-busyness-impacts-performance) se aplica tanto al host como al invitado en un escenario virtualizado. Este es el motivo por el que en un host con un solo invitado, un controlador de dominio (y generalmente cualquier sistema) tiene cerca del mismo rendimiento que en el hardware físico. Agregar invitados adicionales a los hosts aumenta la utilización del host subyacente, con lo que se incrementan los tiempos de espera para obtener acceso a los procesadores como se explicó anteriormente. En Resumen, el uso del procesador lógico debe administrarse tanto en el host como en el nivel de invitado.

Extendiendo las analogías anteriores, lo que deja la autopista como hardware físico, la máquina virtual invitada se analogía con un bus (un Bus Express que va directamente al destino que el conductor quiere). Imagine los cuatro escenarios siguientes:

- Está fuera de horas, un conductor se encuentra en un bus casi vacío y el bus se encuentra en una carretera que también está casi vacía. Dado que no hay tráfico para competir con, el conductor tiene un buen viaje y llega a él tan rápido como si hubiera controlado el piloto en su lugar. Los tiempos de desplazamiento del conductor siguen restringidos por el límite de velocidad.
- Está fuera de horas, por lo que el bus está casi vacío, pero la mayoría de las calles de la carretera están cerradas, por lo que la autopista todavía está congestionada. El conductor está en un bus casi vacío en un camino congestionado. Aunque el conductor no tiene mucha competencia en el bus por dónde sentarse, el tiempo total de la carrera sigue dictado por el resto del tráfico fuera.
- Se trata de una hora de prisa, por lo que la autopista y el bus se congestionan. El recorrido no solo tarda más, sino que el bus es una pesadilla porque las personas están rellenando el hombro y la autopista no es mucho mejor. Agregar más buses (procesadores lógicos al invitado) no significa que puedan encajar en la carretera más fácilmente, o que se acortará el viaje.
- El escenario final, aunque puede estar estirando ligeramente la analogía, es donde el bus está lleno, pero el camino no se congestiona. Aunque el conductor todavía tendrá problemas para entrar y salir del bus, el viaje será eficaz después de que el bus esté en la carretera. Este es el único escenario en el que agregar más buses (procesadores lógicos al invitado) mejorará el rendimiento de los invitados.

A partir de ahí, es relativamente fácil extrapolar que hay una serie de escenarios entre el 0% y el estado de uso 100% de la carretera, así como el estado del 0% y el 100% utilizado del bus que tienen distintos grados de impacto.

La aplicación de las entidades de seguridad anteriores del 40% de CPU como destino razonable para el host, así como el invitado, es un inicio razonable para la misma razón que antes, la cantidad de puesta en cola.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Apéndice B: Consideraciones sobre las distintas velocidades del procesador y el efecto de la administración de energía del procesador en velocidades de procesador

En las secciones de selección del procesador se supone que el procesador se está ejecutando en el 100% de la velocidad del reloj todo el tiempo que se recopilan los datos y que los sistemas de reemplazo tendrán los mismos procesadores de velocidad. A pesar de que ambas suposiciones en la práctica sean falsas, especialmente con Windows Server 2008 R2 y versiones posteriores, donde se **equilibra**el plan de energía predeterminado, la metodología sigue siendo el enfoque conservador. Aunque el posible índice de errores puede aumentar, solo aumenta el margen de seguridad a medida que aumentan las velocidades del procesador.

- Por ejemplo, en un escenario en el que se solicitan 11,25 CPU, si los procesadores se ejecutaban a mitad de velocidad cuando se recopilaban los datos, la &divide; estimación más precisa podría ser 5,125 2.
- No es posible garantizar que la duplicación de las velocidades de reloj duplique la cantidad de procesamiento que se produce durante un período de tiempo determinado. Esto se debe al hecho de que la cantidad de tiempo que los procesadores invierten en esperar en la RAM u otros componentes del sistema podrían permanecer iguales. El efecto neto es que los procesadores más rápidos podrían gastar un porcentaje mayor del tiempo de inactividad mientras esperan a que se capturen los datos. Una vez más, se recomienda que se adhiere al denominador común más bajo, que sea conservador, y que evite intentar calcular un nivel de precisión potencialmente falso suponiendo una comparación lineal entre las velocidades del procesador.

Como alternativa, si las velocidades del procesador en el hardware de sustitución son inferiores al hardware actual, sería seguro aumentar la estimación de los procesadores necesarios en una cantidad proporcional. Por ejemplo, se calcula que se necesitan 10 procesadores para mantener la carga en un sitio y que los procesadores actuales se ejecuten a 3,3 GHz y los procesadores de reemplazo se ejecuten a 2,6 GHz, lo que es una disminución del 21% de velocidad. En este caso, 12 procesadores sería la cantidad recomendada.

Dicho esto, esta variabilidad no cambiaría los objetivos de utilización del procesador de administración de capacidad. Dado que las velocidades del reloj del procesador se ajustarán dinámicamente en función de la carga exigida, la ejecución del sistema en cargas mayores generará un escenario en el que la CPU dedica más tiempo a un estado de velocidad de reloj superior, lo que permite que el objetivo final tenga una utilización del 40% en un 100%. Estado de velocidad del reloj en el máximo. Cualquier menor que lo que generará el ahorro de energía, ya que la velocidad de la CPU se reducirá durante los escenarios de pico de actividad.

> [!NOTE]
> Una opción sería desactivar la administración de energía en los procesadores (estableciendo el plan de energía en **alto rendimiento**) mientras se recopilan los datos. Esto proporcionaría una representación más precisa del consumo de CPU en el servidor de destino.

Para ajustar las estimaciones de los distintos procesadores, solía ser seguro, excepto otros cuellos de botella del sistema descritos anteriormente, a fin de asumir que la velocidad del procesador dobla la duplicación de la cantidad de procesamiento que se podía realizar.  En la actualidad, la arquitectura interna de los procesadores es lo suficientemente distinta entre los procesadores, ya que una manera más segura de medir los efectos del uso de diferentes procesadores de los que se toman los datos es aprovechar la prueba comparativa de SPECint_rate2006 de evaluación del rendimiento estándar. Corporation.

1. Busque las puntuaciones de SPECint_rate2006 para el procesador que se están usando y que se va a usar.
    1. En el sitio web de Standard Performance Evaluation Corporation, seleccione **resultados**, resalte **cpu2006**y seleccione **Buscar todos los resultados de SPECint_rate2006**.
    1. En **solicitud simple**, escriba los criterios de búsqueda para el procesador de destino; por ejemplo, el **procesador coincide con E5-2630 (baselinetarget)** y el **procesador coincide con E5-2650 (línea de base)** .
    1. Busque la configuración del servidor y del procesador que se va a usar (o algo más cercano, si no hay ninguna coincidencia exacta) y anote el valor de las columnas **resultado** y **# cores** .
1. Para determinar el modificador, use la siguiente ecuación:
   >((*Valor de puntuación por núcleo de plataforma*de &times; destino) (*MHz por núcleo de plataforma de línea de base* &divide; )) ((*valor de puntuación por*núcleo de base) &times; (*MHz por núcleo de plataforma de destino*))  

    Con el ejemplo anterior:
   >(35,83 &times; 2000) &divide; (33,75 &times; 2300) = 0,92
1. Multiplica el número estimado de procesadores por el modificador.  En el caso anterior, para pasar del procesador E5-2650 al procesador E5-2630, multiplique las CPU &times; calculadas 11,25 0,92 = 10,35 procesadores necesarios.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Apéndice C: Aspectos básicos relacionados con el sistema operativo que interactúan con el almacenamiento

Los conceptos de la teoría de puesta en cola que se describen en [tiempo de respuesta/cómo afecta el rendimiento del sistema](#response-timehow-the-system-busyness-impacts-performance) también son aplicables al almacenamiento. Para aplicar estos conceptos, es necesario estar familiarizado con el modo en que el sistema operativo controla la e/s. En el sistema operativo Microsoft Windows, se crea una cola para contener las solicitudes de e/s para cada disco físico. Sin embargo, es necesario realizar una aclaración sobre el disco físico. Los controladores de matriz y San presentan agregaciones de ejes al sistema operativo como discos físicos únicos. Además, los controladores de matriz y las San pueden agregar varios discos en un conjunto de matrices y, a continuación, dividir este conjunto de matrices en varias "particiones", que a su vez se presenta al sistema operativo como varios discos físicos (Ref. figura).

![Ejes de bloque](media/capacity-planning-considerations-block-spindles.png)  

En esta ilustración, los dos ejes están reflejados y divididos en áreas lógicas para el almacenamiento de datos (Data 1 y Data 2). El sistema operativo ve estas áreas lógicas como discos físicos independientes.

Aunque esto puede resultar muy confuso, en este apéndice se usa la terminología siguiente para identificar las distintas entidades:

- **Eje:** el dispositivo que está instalado físicamente en el servidor.
- **Matriz:** una colección de ejes agregados por el controlador.
- **Partición de matriz:** partición de la matriz agregada
- **LUN:** una matriz, que se usa cuando se hace referencia a San
- **Disco:** Lo que el sistema operativo observa que es un único disco físico.
- **Partición:** una partición lógica de lo que el sistema operativo percibe como un disco físico.

### <a name="operating-system-architecture-considerations"></a>Consideraciones sobre la arquitectura del sistema operativo

El sistema operativo crea una cola de e/s de primera y primera salida (FIFO) para cada disco que se observa; Este disco puede representar un eje, una matriz o una partición de matriz. Desde la perspectiva del sistema operativo, con respecto al control de e/s, las colas más activas mejoran. Cuando se serializa una cola FIFO, lo que significa que todas las operaciones de e/s emitidas en el subsistema de almacenamiento deben procesarse en el orden en que llegó la solicitud. Al correlacionar cada disco observado por el sistema operativo con un eje o matriz, el sistema operativo ahora mantiene una cola de e/s para cada conjunto de discos único, con lo que se elimina la contención de los recursos de e/s insuficientes en los discos y el aislamiento de la demanda de e/s a una única discos. Como excepción, Windows Server 2008 presenta el concepto de priorización de e/s y las aplicaciones diseñadas para usar la prioridad "baja" salen de este orden normal y toman un respaldo. Las aplicaciones no codificadas específicamente para aprovechar la prioridad "baja" predeterminada como "normal".

### <a name="introducing-simple-storage-subsystems"></a>Introducción a subsistemas de almacenamiento simples

A partir de un ejemplo simple (una sola unidad de disco duro dentro de un equipo), se proporcionará un análisis componente por componente. Al dividirlo en los principales componentes del subsistema de almacenamiento, el sistema consta de:

- **1 –** 10.000 RPM ultra Fast SCSI HD (ultra Fast SCSI tiene una velocidad de transferencia de 20 MB/s)
- **1:** Bus SCSI (el cable)
- **1:** Adaptador SCSI Ultra Fast
- **1:** 32 bits de bus PCI de 33 MHz

Una vez identificados los componentes, una idea de la cantidad de datos que pueden transitar el sistema o la cantidad de e/s que se puede controlar, se puede calcular. Tenga en cuenta que la cantidad de e/s y la cantidad de datos que pueden transitar el sistema están correlacionadas, pero no es lo mismo. Esta correlación depende de si la e/s de disco es aleatoria o secuencial y el tamaño de bloque. (Todos los datos se escriben en el disco como un bloque, pero diferentes aplicaciones que usan distintos tamaños de bloque). En función de cada componente:

- **El disco duro:** El disco duro promedio de 10.000 RPM tiene un tiempo de búsqueda de 7 milisegundos y un tiempo de acceso de 3 ms. El tiempo de búsqueda es el promedio de tiempo que tarda el cabezal de lectura/escritura en moverse a una ubicación en el platter. El tiempo de acceso es la cantidad media de tiempo que se tarda en leer o escribir los datos en el disco, una vez que el encabezado está en la ubicación correcta. Por lo tanto, el promedio de tiempo para leer un bloque de datos único en una unidad de disco duro 10.000-RPM constituye una búsqueda y un acceso, para un total de aproximadamente 10 ms (o .010 segundos) por bloque de datos.

  Cuando cada acceso al disco requiere el movimiento del encabezado a una nueva ubicación en el disco, el comportamiento de lectura y escritura se conoce como "aleatorio". Por lo tanto, cuando todas las e/s son aleatorias, una HD 10.000-RPM puede controlar aproximadamente 100 e/s por segundo (IOPS) (la fórmula es de 1000 ms por segundo dividido entre 10 ms por e/s o 1000/10 = 100 IOPS).

  Como alternativa, cuando todas las e/s se producen desde sectores adyacentes de la HD, esto se conoce como e/s secuenciales. La e/s secuencial no tiene tiempo de búsqueda porque, cuando se completa la primera e/s, el encabezado de lectura/escritura se encuentra al principio de donde se almacena el siguiente bloque de datos en la unidad de disco duro. Por lo tanto, una HD 10.000-RPM es capaz de controlar aproximadamente 333 e/s por segundo (1000 ms por segundo dividido por 3 ms por e/s).

  >[!NOTE]
  >En este ejemplo no se refleja la memoria caché del disco, donde normalmente se conservan los datos de un cilindro. En este caso, se necesitan los 10 ms en la primera e/s y el disco Lee todo el cilindro. Todas las demás e/s secuenciales se satisfacen desde la memoria caché. Como resultado, las cachés en disco podrían mejorar el rendimiento de e/s secuencial.
  
  Hasta ahora, la velocidad de transferencia del disco duro ha sido irrelevante. Si el disco duro es de 20 MB/s Ultra Wide o un Ultra3 160 MB/s, la cantidad real de IOPS que puede controlar el HD de 10.000-RPM es ~ 100 Random o ~ 300 de e/s secuenciales. A medida que los tamaños de bloque cambian en función de la escritura de la aplicación en la unidad, la cantidad de datos que se extraen por e/s es diferente. Por ejemplo, si el tamaño de bloque es de 8 KB, 100 operaciones de e/s leerán o escribirán en la unidad de disco duro un total de 800 KB. Sin embargo, si el tamaño de bloque es de 32 KB, 100 e/s leerán y escribirán 3.200 KB (3,2 MB) en el disco duro. Siempre que la velocidad de transferencia SCSI supere la cantidad total de datos transferidos, obtener una unidad de velocidad de transferencia "más rápida" no tendrá nada. Consulte las tablas siguientes para obtener una comparación.

  | |7200 RPM 9MS, acceso 4ms estándar|10.000 RPM 7ms, acceso 3ms|15.000 RPM 4ms estándar de búsqueda, 2 ms de acceso
  |-|-|-|-|
  |E/s aleatoria|80|100|150|
  |E/s secuenciales|250|300|500|  
  
  |unidad de 10.000 RPM|tamaño de bloque de 8 KB (Active Directory jet)|
  |-|-|
  |E/s aleatoria|800 KB/s|
  |E/s secuenciales|2400 KB/s|

- **Backplane SCSI (bus):** Entender cómo el "backplane SCSI (bus)", o en este escenario, el cable de la cinta de opciones afecta al rendimiento del subsistema de almacenamiento depende del conocimiento del tamaño de bloque. En esencia, la cuestión sería, ¿cuánta e/s puede controlar el bus si la e/s está en bloques de 8 KB? En este escenario, el bus SCSI es de 20 MB/s o 20480 KB/s. 20480 KB/s divididos por 8 bloques de KB produce un máximo de aproximadamente 2500 IOPS admitidas por el bus SCSI.

  >[!NOTE]
  >Las cifras de la tabla siguiente representan un ejemplo. Actualmente, la mayoría de los dispositivos de almacenamiento conectados usan PCI Express, lo que proporciona un rendimiento mucho mayor.  
  
  |E/s compatible con el bus SCSI por tamaño de bloque|tamaño de bloque de 2 KB|tamaño de bloque de 8 KB (AD jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10.000|2\.500|
  |40 MB/s|20.000|5\.000|
  |128 MB/s|65.536|16.384|
  |320 MB/s|160.000|40.000|

  Como puede determinarse a partir de este gráfico, en el escenario que se presenta, independientemente de cuál sea el uso, el bus nunca será un cuello de botella, ya que el máximo del eje es 100 e/s, por debajo de los umbrales anteriores.

  >[!NOTE]
  >Se supone que el bus SCSI tiene un 100% eficaz.
  
- **Adaptador SCSI:** Para determinar la cantidad de e/s que puede controlar, deben comprobarse las especificaciones del fabricante. Dirigir las solicitudes de e/s al dispositivo adecuado requiere el procesamiento de algún tipo, por lo que la cantidad de e/s que se puede controlar depende del procesador SCSI (o controlador de la matriz).

  En este ejemplo, se supondrá que se realizará la e/s 1.000.

- **Bus PCI:** Se trata de un componente que se suele pasar por alto. En este ejemplo, no será el cuello de botella; sin embargo, a medida que los sistemas se escalan verticalmente, puede convertirse en un cuello de botella. Como referencia, un bus PCI de 32 bits que funciona a 33 MHz puede, en teoría, transferir 133 MB/s de datos. A continuación se encuentra la ecuación:  
  > 32 bits &divide; 8 bits por byte &times; 33 MHz = 133 MB/s.  

  Tenga en cuenta que es el límite teórico; en realidad, en realidad solo se alcanza el 50% del máximo, aunque en ciertos escenarios de ráfaga se puede obtener una eficiencia del 75% durante breves períodos.

  Un bus PCI de 64 bits de 66 Mbps puede admitir un máximo teórico de ( &divide; 64 bits 8 bits &times; por byte 66 MHz) = 528 MB/s. Además, cualquier otro dispositivo (como el adaptador de red, el segundo controlador SCSI, etc.) reducirá el ancho de banda disponible. a medida que se comparte el ancho de banda y los dispositivos compiten por los recursos limitados.

Después del análisis de los componentes de este subsistema de almacenamiento, el eje es el factor de limitación en la cantidad de e/s que se puede solicitar y, por lo tanto, la cantidad de datos que pueden transitar por el sistema. Concretamente, en un escenario de AD DS, es 100 e/s aleatoria por segundo en incrementos de 8 KB, con un total de 800 KB por segundo al tener acceso a la base de datos Jet. Como alternativa, el rendimiento máximo de un eje asignado exclusivamente a los archivos de registro sufrirá las siguientes limitaciones: 300 de e/s secuenciales por segundo en incrementos de 8 KB, para un total de 2400 KB (2,4 MB) por segundo.

Ahora, después de analizar una configuración simple, en la tabla siguiente se muestra dónde se produce el cuello de botella cuando se cambian o agregan componentes en el subsistema de almacenamiento.

|Notas|Análisis de cuellos de botella|Disk|Bus|Adaptador|Bus PCI|
|-|-|-|-|-|-|
|Esta es la configuración del controlador de dominio después de agregar un segundo disco. La configuración de disco representa el cuello de botella en 800 KB/s.|Agregar 1 disco (total = 2)<br /><br />La e/s es aleatoria<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD|200 de e/s en total<br />800 KB/s en total.| | | |
|Después de agregar 7 discos, la configuración del disco sigue representando el cuello de botella en 3200 KB/s.|**Agregar 7 discos (total = 8)**  <br /><br />La e/s es aleatoria<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD|800 e/s en total.<br />3200 KB/s en total| | | |
|Después de cambiar la e/s a secuencial, el adaptador de red se convierte en el cuello de botella porque está limitado a 1000 IOPS.|Agregar 7 discos (total = 8)<br /><br />**La e/s es secuencial**<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD| | |2400 de e/s se puede leer y escribir en el disco, un controlador limitado a 1000 IOPS| |
|Después de reemplazar el adaptador de red por un adaptador SCSI que admita 10.000 IOPS, el cuello de botella vuelve a la configuración del disco.|Agregar 7 discos (total = 8)<br /><br />La e/s es aleatoria<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD<br /><br />**Actualizar Adaptador SCSI (ahora es compatible con 10.000 e/s)**|800 e/s en total.<br />3\.200 KB/s en total| | | |
|Después de aumentar el tamaño de bloque a 32 KB, el bus se convierte en el cuello de botella porque solo admite 20 MB/s.|Agregar 7 discos (total = 8)<br /><br />La e/s es aleatoria<br /><br />**tamaño de bloque de 32 KB**<br /><br />10.000 RPM HD| |800 e/s en total. 25.600 KB/s (25 MB/s) se pueden leer o escribir en el disco.<br /><br />El bus solo admite 20 MB/s| | |
|Después de actualizar el bus y agregar más discos, el disco permanece en el cuello de botella.|**Agregar 13 discos (total = 14)**<br /><br />Agregar segundo Adaptador SCSI con 14 discos<br /><br />La e/s es aleatoria<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD<br /><br />**Actualizar a un bus SCSI de 320 MB/s**|e/s 2800<br /><br />11.200 KB/s (10,9 MB/s)| | | |
|Después de cambiar la e/s a secuencial, el disco sigue siendo el cuello de botella.|Agregar 13 discos (total = 14)<br /><br />Agregar segundo Adaptador SCSI con 14 discos<br /><br />**La e/s es secuencial**<br /><br />tamaño de bloque de 4 KB<br /><br />10.000 RPM HD<br /><br />Actualizar a un bus SCSI de 320 MB/s|e/s 8.400<br /><br />33.600 KB\s<br /><br />(32,8 MB\s)| | | |
|Después de agregar unidades de disco duro más rápidas, el disco permanece en el cuello de botella.|Agregar 13 discos (total = 14)<br /><br />Agregar segundo Adaptador SCSI con 14 discos<br /><br />La e/s es secuencial<br /><br />tamaño de bloque de 4 KB<br /><br />**15.000 RPM HD**<br /><br />Actualizar a un bus SCSI de 320 MB/s|e/s 14.000<br /><br />56.000 KB/s<br /><br />(54,7 MB/s)| | | |
|Después de aumentar el tamaño de bloque a 32 KB, el bus PCI se convierte en el cuello de botella.|Agregar 13 discos (total = 14)<br /><br />Agregar segundo Adaptador SCSI con 14 discos<br /><br />La e/s es secuencial<br /><br />**tamaño de bloque de 32 KB**<br /><br />15.000 RPM HD<br /><br />Actualizar a un bus SCSI de 320 MB/s| | | |e/s 14.000<br /><br />448.000 KB/s<br /><br />(437 MB/s) es el límite de lectura y escritura para el eje.<br /><br />El bus PCI admite un máximo teórico de 133 MB/s (75% de forma eficaz).|

### <a name="introducing-raid"></a>Introducción a RAID

La naturaleza de un subsistema de almacenamiento no cambia drásticamente cuando se introduce un controlador de la matriz. simplemente reemplaza el adaptador SCSI en los cálculos. Lo que cambia es el costo de leer y escribir datos en el disco cuando se usan los distintos niveles de matriz (como RAID 0, RAID 1 o RAID 5).

En RAID 0, los datos se dividen en todos los discos del conjunto de RAID. Esto significa que durante una operación de lectura o de escritura, una parte de los datos se extrae o se inserta en cada disco, lo que aumenta la cantidad de datos que pueden transferir el sistema durante el mismo período de tiempo. Por lo tanto, en un segundo, en cada eje (de nuevo suponiendo que las unidades 10.000-RPM), se pueden realizar las operaciones de e/s de 100. La cantidad total de e/s que se puede admitir es de N ejes en 100 de e/s por segundo por eje (produce 100 * N e/s por segundo).

![Unidad lógica d:](media/capacity-planning-considerations-logical-d-drive.png)

En RAID 1, los datos se reflejan (se duplican) a través de un par de ejes de redundancia. Por lo tanto, cuando se realiza una operación de e/s de lectura, se pueden leer los datos de los dos ejes del conjunto. Esto hace que la capacidad de e/s de ambos discos esté disponible durante una operación de lectura. La advertencia es que las operaciones de escritura no obtienen ninguna ventaja de rendimiento en RAID 1. Esto se debe a que es necesario escribir los mismos datos en ambas unidades con el fin de lograr redundancia. Aunque no tarda más, como la escritura de datos se produce simultáneamente en ambos ejes, ya que ambos ejes están ocupados duplicando los datos, una operación de e/s de escritura en esencia evita que se produzcan dos operaciones de lectura. Por lo tanto, cada e/s de escritura cuesta dos e/s de lectura. Se puede crear una fórmula a partir de esa información para determinar el número total de operaciones de e/s que se están produciendo:  

> E/s de *lectura* + &times; 2 de e/s de *escritura* = total de e/s de*disco consumidas*  

Cuando se conoce la proporción de lecturas en las escrituras y el número de ejes, la siguiente ecuación se puede derivar de la ecuación anterior para identificar la e/s máxima que puede admitir la matriz:  

> *Máximo IOPS por eje* &times; 2 Ejes &times; [( *%Leer* +  *%Escribir*) &divide; ( *%Leer* + 2 &times; *%Escribir*)] = *Total IOPS*

RAID 1 + 0, se comporta exactamente igual que RAID 1 con respecto a los gastos de lectura y escritura. Sin embargo, la e/s ahora se divide en cada conjunto reflejado. Si  

> *Máximo IOPS por eje* &times; 2 Ejes &times; [( *%Leer* +  *%Escribir*) &divide; ( *%Leer* + 2 &times; *%Escribir*)] = *Total IOPS*  

en un conjunto RAID 1, cuando se secciona una multiplicidad (*N*) de conjuntos RAID 1, el total de e/s que se puede procesar se convierte en &times; N e/s por cada conjunto de RAID 1:  

> *N* &times; {*Maximo IOPS por eje* &times; 2 ejes &times; [( *%Leer* +  *%Escribir*) &divide; ( *%Leer* + 2 &times; *%Escribir*)] } = *Total IOPS*

En RAID 5, a veces denominado RAID *n* + 1, los datos se dividen en *n* ejes y la información de paridad se escribe en el eje "+ 1". Sin embargo, RAID 5 es mucho más costoso cuando se realiza una e/s de escritura que RAID 1 o 1 + 0. RAID 5 realiza el siguiente proceso cada vez que se envía una e/s de escritura a la matriz:

1. Leer los datos antiguos
1. Leer la paridad anterior
1. Escribir los datos nuevos
1. Escribir la nueva paridad

Dado que cada solicitud de e/s de escritura que el sistema operativo envía al controlador de la matriz requiere que se completen cuatro operaciones de e/s, las solicitudes de escritura enviadas tardan cuatro veces en completarse como una sola e/s de lectura. Para derivar una fórmula para traducir las solicitudes de e/s desde la perspectiva del sistema operativo a las que experimentan los ejes:  

> *E/* s de e/ &times; s de e */s de* *escritura* = + 4  

De forma similar en un conjunto de RAID 1, cuando se conoce la proporción de lecturas en las escrituras y el número de ejes, la siguiente ecuación se puede derivar de la ecuación anterior para identificar la e/s máxima que puede admitir la matriz (tenga en cuenta que el número total de ejes no incluye e la "unidad" se perdió en la paridad):  

> *IOPS por eje* &times; (*Ejes* – 1) &times; [( *%Leer* +  *%Escribir*) &divide; ( *%Leer* + 4 &times; *%Escribir*)] = *Total IOPS*

### <a name="introducing-sans"></a>Presentación de redes San

Al ampliar la complejidad del subsistema de almacenamiento, cuando se introduce una SAN en el entorno, los principios básicos descritos no cambian, pero el comportamiento de e/s de todos los sistemas conectados a la SAN debe tenerse en cuenta. Como una de las principales ventajas de usar una SAN es una cantidad adicional de redundancia sobre el almacenamiento conectado interna o externamente, el planeamiento de la capacidad ahora debe tener en cuenta las necesidades de tolerancia a errores. Además, se introducen más componentes que deben evaluarse. Dividir una SAN en las partes del componente:

- Unidad de disco duro SCSI o Canal de fibra
- Backplane de canal de unidad de almacenamiento
- Unidades de almacenamiento
- Módulo de controlador de almacenamiento
- Conmutadores SAN
- HBA (s)
- El bus PCI

Al diseñar un sistema para la redundancia, se incluyen componentes adicionales para dar cabida al potencial de errores. Es muy importante, cuando el planeamiento de la capacidad, excluir el componente redundante de los recursos disponibles. Por ejemplo, si la red SAN tiene dos módulos de controlador, la capacidad de e/s de un módulo de controlador es todo lo que se debe usar para el rendimiento total de e/s disponible para el sistema. Esto se debe al hecho de que, si se produce un error en un controlador, el controlador restante deberá procesar toda la carga de e/s que requieren todos los sistemas conectados. Como todo el planeamiento de la capacidad se realiza para períodos de uso máximo, los componentes redundantes no se deben factorizar en los recursos disponibles y el uso máximo planeado no debe superar el 80% de la saturación del sistema (con el fin de dar cabida a ráfagas o sistemas anómalos). comportamiento). Del mismo modo, el conmutador SAN redundante, la unidad de almacenamiento y los ejes no se deben factorizar en los cálculos de e/s.

Al analizar el comportamiento de la unidad de disco duro SCSI o Canal de fibra, el método de analizar el comportamiento tal y como se ha descrito anteriormente no cambia. Aunque existen ciertas ventajas y desventajas de cada protocolo, el factor de limitación en cada disco es la limitación mecánica del disco duro.

El análisis del canal en la unidad de almacenamiento es exactamente el mismo que el cálculo de los recursos disponibles en el bus SCSI o el ancho de banda (por ejemplo, 20 MB/s) dividido por el tamaño de bloque (por ejemplo, 8 KB). Donde esto se desvía del ejemplo anterior simple está en la agregación de varios canales. Por ejemplo, si hay 6 canales, cada uno de los cuales admite una velocidad de transferencia máxima de 20 MB/s, la cantidad total de e/s y la transferencia de datos que está disponible es 100 MB/s (esto es correcto, no es 120 MB/s). De nuevo, la tolerancia a errores es un importante reproductor en este cálculo, en el caso de la pérdida de un canal completo, el sistema se deja solo con 5 canales en funcionamiento. Por lo tanto, para garantizar que se sigan cumpliendo las expectativas de rendimiento en caso de error, el rendimiento total de todos los canales de almacenamiento no debe superar los 100 MB/s (esto supone que la carga y la tolerancia a errores se distribuyen uniformemente entre todos los canales). Convertirlo en un perfil de e/s depende del comportamiento de la aplicación. En el caso de Active Directory e/s de jet, esto correlacionaría aproximadamente 12.500 e/s por segundo (100 MB/s &divide; 8 KB por e/s).

A continuación, se necesita obtener las especificaciones del fabricante para los módulos de controlador con el fin de comprender el rendimiento que puede admitir cada módulo. En este ejemplo, la red SAN tiene dos módulos de controlador que admiten la e/s de 7.500. El rendimiento total del sistema puede ser 15.000 IOPS si no se desea redundancia. En el cálculo del rendimiento máximo en caso de error, la limitación es el rendimiento de un controlador o 7.500 IOPS. Este umbral está por debajo del máximo de 12.500 e/s por segundo (suponiendo un tamaño de bloque de 4 KB) que puede ser compatible con todos los canales de almacenamiento y, por lo tanto, es actualmente el cuello de botella en el análisis. Sin embargo, para fines de planeamiento, la e/s máxima deseada para la planeación sería de 10.400 e/s.

Cuando los datos salen del módulo de controlador, transmite una conexión Canal de fibra clasificada en 1 GB/s (o 1 Gigabit por segundo). Para correlacionarlo con las otras métricas, 1 GB/s se convierte en 128 MB/s (1 GB/ &divide; s 8 bits/byte). Dado que esto supera el ancho de banda total en todos los canales de la unidad de almacenamiento (100 MB/s), esto no afectará al sistema. Además, como esto es solo uno de los dos canales (la conexión adicional de 1 GB/s Canal de fibra para la redundancia), si se produce un error en una conexión, la conexión restante todavía tiene suficiente capacidad para controlar toda la transferencia de datos solicitada.

En ruta al servidor, lo más probable es que los datos atraviesen un conmutador SAN. A medida que el conmutador SAN tiene que procesar la solicitud de e/s entrante y reenviarla al puerto adecuado, el conmutador tendrá un límite en la cantidad de e/s que se puede administrar; sin embargo, se necesitarán las especificaciones del fabricante para determinar cuál es el límite. Por ejemplo, si hay dos modificadores y cada conmutador puede controlar 10.000 IOPS, el rendimiento total será de 20.000 IOPS. Una vez más, la tolerancia a errores es un problema, si se produce un error en un conmutador, el rendimiento total del sistema será 10.000 IOPS. Como se desea que no supere la utilización del 80% en el funcionamiento normal, el uso de no más de 8000 e/s debe ser el destino.

Por último, el HBA instalado en el servidor también tendrá un límite en la cantidad de e/s que puede administrar. Normalmente, se instala un segundo HBA para la redundancia, pero al igual que con el conmutador SAN, al calcular la e/s máxima que se puede controlar, el rendimiento total de *N* &ndash; 1 HBA es el que es la máxima escalabilidad del sistema.

### <a name="caching-considerations"></a>Consideraciones de almacenamiento en caché

Las cachés son uno de los componentes que pueden afectar significativamente al rendimiento general en cualquier punto del sistema de almacenamiento. El análisis detallado de los algoritmos de almacenamiento en caché está fuera del ámbito de este artículo. sin embargo, merece la pena iluminar algunas instrucciones básicas sobre el almacenamiento en caché en subsistemas de disco:

- El almacenamiento en caché mejora la e/s de escritura secuencial sostenida, ya que puede almacenar en búfer muchas operaciones de escritura más pequeñas en bloques de e/s más grandes y detenerlos en el almacenamiento en menos tamaños de bloque, pero más grandes. Esto reducirá el total de e/s aleatorias y el total de e/s, lo que proporciona más disponibilidad de recursos para otras operaciones de e/s.
- El almacenamiento en caché no mejora el rendimiento de e/s de escritura sostenido del subsistema de almacenamiento. Solo permite almacenar en búfer las escrituras hasta que los ejes estén disponibles para confirmar los datos. Cuando toda la e/s disponible de los ejes del subsistema de almacenamiento está saturada durante largos períodos, la memoria caché se llenará en el futuro. Para vaciar la memoria caché, se debe asignar un tiempo suficiente entre ráfagas o ejes adicionales, con el fin de proporcionar suficiente e/s para permitir que se vacíe la memoria caché.

  Las cachés de mayor tamaño solo permiten que se almacenen más datos en el búfer. Esto significa que se pueden acomodar períodos más largos de saturación.

  En un subsistema de almacenamiento operativo normal, el sistema operativo experimentará un rendimiento de escritura mejorado, ya que los datos solo deben escribirse en la memoria caché. Una vez que el medio subyacente está saturado con e/s, la memoria caché se llenará y el rendimiento de escritura volverá a la velocidad del disco.

- Al almacenar en caché e/s de lectura, el escenario en el que la memoria caché es más ventajoso es cuando los datos se almacenan secuencialmente en el disco y la memoria caché puede ser de lectura previa (se supone que el siguiente sector contiene los datos que se solicitarán a continuación).
- Cuando la e/s de lectura es aleatoria, no es probable que el almacenamiento en caché en el controlador de la unidad proporcione ninguna mejora en la cantidad de datos que se pueden leer desde el disco. Cualquier mejora no existe si el tamaño de la caché basada en el sistema operativo o la aplicación es mayor que el tamaño de la caché basada en hardware.

  En el caso de Active Directory, la memoria caché solo está limitada por la cantidad de RAM.

### <a name="ssd-considerations"></a>Consideraciones sobre SSD

Las SSD son un animal completamente diferente que los discos duros basados en el cabezal. Todavía quedan los dos criterios clave: "¿Cuántas IOPS puede controlar?" y "¿Cuál es la latencia de esas IOPS?" En comparación con los discos duros basados en el eje, las SSD pueden administrar mayores volúmenes de e/s y pueden tener latencias más bajas. En general y en el momento de redactar este documento, aunque las SSD siguen siendo costosas en comparación de costos por Gigabyte, son muy baratas en términos de costo por e/s y merecen una consideración significativa en cuanto al rendimiento del almacenamiento.

Consideraciones:

- Tanto la e/s por segundo como las latencias son muy subjetivas a los diseños del fabricante y, en algunos casos, se han observado un rendimiento más deficiente que las tecnologías basadas en el eje. En Resumen, es más importante revisar y validar las especificaciones del fabricante unidad por unidad y no asumir ninguna generalidad.
- Los tipos de IOPS pueden tener números muy diferentes en función de si son de lectura o escritura. AD DS servicios, en general, que están principalmente basados en el modo de lectura, serán menos afectados que otros escenarios de aplicaciones.
- "Resistencia a la escritura": este es el concepto de que las celdas SSD se gastarán finalmente. Varios fabricantes abordan este desafío de maneras diferentes. Como mínimo, para la unidad de base de datos, el perfil de e/s de lectura en su mayor medida permite downplaying la importancia de este problema, ya que los datos no son muy volátiles.

### <a name="summary"></a>Resumen

Una manera de pensar en el almacenamiento es representar la fontanería de los hogares. Imagine que la IOPS del medio en el que se almacenan los datos es el drenaje principal del hogar. Cuando está atascado (como las raíces en la canalización) o limitado (está contraído o demasiado pequeño), se hace una copia de seguridad de todos los receptores de la familia cuando se usa demasiado agua (demasiados invitados). Esto es perfectamente análogo a un entorno compartido en el que uno o varios sistemas aprovechan el almacenamiento compartido en una SAN/NAS/iSCSI con los mismos medios subyacentes. Se pueden realizar diferentes enfoques para resolver los distintos escenarios:

- Una purga contraída o de tamaño reducido requiere un reemplazo y una corrección de escala completa. Esto sería similar a agregar hardware nuevo o redistribuir los sistemas mediante el almacenamiento compartido en toda la infraestructura.
- Una canalización "atascada" normalmente significa la identificación de uno o más problemas infractores y la eliminación de dichos problemas. En un escenario de almacenamiento, podría tratarse de copias de seguridad del nivel de almacenamiento o del sistema, exámenes antivirus sincronizados en todos los servidores y software de desfragmentación sincronizado que se ejecuta durante los períodos de máxima actividad.

En cualquier diseño de fontanería, el consumo de varias desagües se alimenta en el desagüe principal. Si algo detiene una de esas purgas o un punto de Unión, solo se realizará una copia de seguridad de las cosas que hay detrás de ese punto de Unión. En un escenario de almacenamiento, podría tratarse de un conmutador sobrecargado (escenario SAN/NAS/iSCSI), problemas de compatibilidad de controladores (combinación incorrecta de controlador/firmware de HBA/Storport. sys) o copia de seguridad/antivirus/desfragmentación. Para determinar si la "canalización" de almacenamiento es lo suficientemente grande, es necesario medir las IOPS y el tamaño de e/s. En cada conjunto, agréguelos juntos para garantizar el "diámetro de la canalización" adecuado.

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Apéndice D: Análisis de la solución de problemas de almacenamiento: entornos en los que se proporciona al menos la cantidad de RAM que el tamaño de la base de datos no es una opción viable

Resulta útil comprender por qué existen estas recomendaciones para adaptarse a los cambios en la tecnología de almacenamiento. Estas recomendaciones existen por dos motivos. La primera es el aislamiento de e/s, de modo que los problemas de rendimiento (es decir, la paginación) en el eje del sistema operativo no afectan al rendimiento de la base de datos y los perfiles de e/s. La segunda es que los archivos de registro para AD DS (y la mayoría de las bases de datos) son secuenciales por naturaleza, y las unidades de disco duro y las memorias caché basadas en el eje tienen una ventaja de rendimiento enorme cuando se usan con e/s secuenciales en comparación con los patrones de e/s más aleatorios del sistema operativo y casi patrones de e/s puramente aleatorios de la unidad de base de datos AD DS. Al aislar la e/s secuencial en una unidad física independiente, se puede aumentar el rendimiento. El desafío presentado por las opciones de almacenamiento de hoy en día es que las suposiciones fundamentales detrás de estas recomendaciones ya no son verdaderas. En muchos escenarios de almacenamiento virtualizado, como iSCSI, SAN, NAS y archivos de imagen de disco virtual, el medio de almacenamiento subyacente se comparte entre varios hosts, por lo que se niega completamente el "aislamiento de e/s" y los aspectos "optimización de e/s secuencial". En realidad, estos escenarios agregan una capa adicional de complejidad en la que otros hosts que acceden a los medios compartidos pueden degradar la capacidad de respuesta del controlador de dominio.

En la planeación del rendimiento del almacenamiento, hay tres categorías a tener en cuenta: estado de la caché en frío, estado de caché activa y copia de seguridad/restauración. El estado de la caché en frío se produce en escenarios como el momento en que el controlador de dominio se reinicia inicialmente o el servicio Active Directory se reinicia y no hay Active Directory datos en RAM. El estado de caché semiactiva es donde el controlador de dominio está en un estado estable y la base de datos se almacena en caché. Es importante tener en cuenta a medida que se van a generar perfiles de rendimiento muy diferentes y tener suficiente RAM para almacenar en caché toda la base de datos no ayuda al rendimiento cuando la memoria caché está en frío. Uno puede considerar el diseño del rendimiento para estos dos escenarios con la siguiente analogía, el calentamiento de la caché en frío es un "Sprint" y la ejecución de un servidor con una caché activa es un "Marathon".

En el caso de la caché en frío y en el escenario de caché activa, la pregunta se convierte en la velocidad con que el almacenamiento puede trasladar los datos del disco a la memoria. Preparar la memoria caché es un escenario en el que, con el tiempo, el rendimiento mejora a medida que se reutilizan los datos de más consultas, se aumenta la tasa de aciertos de caché y se reduce la frecuencia de necesidad de ir al disco. Como resultado, se reduce el impacto negativo en el rendimiento del disco. Cualquier degradación del rendimiento solo es transitoria mientras se espera a que la memoria caché se caliente y crezca hasta el tamaño máximo permitido dependiente del sistema. La conversación se puede simplificar con la rapidez con la que se pueden obtener los datos del disco, y es una medida simple de IOPS disponibles para Active Directory, que es subjetivo a IOPS disponibles en el almacenamiento subyacente. Desde el punto de vista de la planificación, dado que el calentamiento de la memoria caché y los escenarios de copia de seguridad y restauración se producen de forma excepcional, se suelen producir horas de inactividad y dependen de la carga del controlador de dominio, las recomendaciones generales no existen excepto en que se programan estas actividades para horas que no son de pico.

AD DS, en la mayoría de los escenarios, es principalmente de lectura e/s, normalmente una proporción del 90% de lectura/10% de escritura. La e/s de lectura suele ser el cuello de botella de la experiencia del usuario y, con la e/s de escritura, hace que el rendimiento de escritura se reduzca. Como la e/s de NTDS. dit es principalmente aleatoria, las memorias caché suelen proporcionar una ventaja mínima para la e/s de lectura, por lo que es mucho más importante configurar correctamente el almacenamiento para el perfil de e/s de lectura.

En cuanto a las condiciones de funcionamiento normales, el objetivo de planeamiento del almacenamiento es minimizar los tiempos de espera para que una solicitud de AD DS se devuelva desde el disco. Básicamente, esto significa que el número de e/s pendientes y pendientes es menor o igual que el número de rutas al disco. Hay varias maneras de medir esto. En un escenario de supervisión del rendimiento, la recomendación general es que LogicalDisk ( *\<unidad\>de base de datos Ntds*) \Avg en segundos/lectura sea inferior a 20 ms. El umbral de funcionamiento deseado debe ser mucho menor, preferiblemente lo más cerca posible de la velocidad del almacenamiento, en el intervalo de 2 a 6 milisegundos (. 002 a .006 segundo) en función del tipo de almacenamiento.

Ejemplo:

![Gráfico de latencia de almacenamiento](media/capacity-planning-considerations-storage-latency.png)

Analizar el gráfico:

- **Óvalo verde a la izquierda:** La latencia sigue siendo coherente en 10 ms. La carga aumenta de 800 IOPS a 2400 IOPS. Esta es la parte absoluta de la rapidez con la que el almacenamiento subyacente puede procesar una solicitud de e/s. Esto está sujeto a los detalles de la solución de almacenamiento.
- **Burdeos ovalada a la derecha:** El rendimiento sigue siendo plano desde la salida del círculo verde hasta el final de la recopilación de datos mientras la latencia continúa aumentando. Esto demuestra que cuando los volúmenes de solicitudes superan las limitaciones físicas del almacenamiento subyacente, más tiempo gastan las solicitudes en la cola en espera de enviarse al subsistema de almacenamiento.

Aplicando esta información:

- **Impacto en un usuario que consulta la pertenencia de un grupo grande:** Supongamos que esto requiere la lectura de 1 MB de datos del disco, la cantidad de e/s y el tiempo que se tarda en evaluarse de la siguiente manera:
  - Active Directory páginas de base de datos tienen un tamaño de 8 KB. 
  - Se debe leer un mínimo de 128 páginas desde el disco. 
  - Suponiendo que no haya nada almacenado en caché, en el plano inferior (10 ms) se tardará un mínimo de 1,28 segundos en cargar los datos del disco para devolverlos al cliente. En 20 MS, donde el rendimiento en el almacenamiento tiene mucho tiempo desde que se ha agotado y también es el máximo recomendado, se tardarán 2,5 segundos en obtener los datos del disco para devolverlos al usuario final.  
- **En qué frecuencia se calienta la caché:** Al suponer que la carga del cliente va a maximizar el rendimiento en este ejemplo de almacenamiento, la memoria caché se calienta a una velocidad &times; de 2400 IOPS 8 KB por e/s. O bien, aproximadamente 20 MB/s por segundo, carga aproximadamente 1 GB de base de datos en la RAM cada 53 segundos.

> [!NOTE]
> Es normal que los períodos cortos observen las latencias con concordancia cuando los componentes leen o escriben de forma agresiva en el disco, como cuando se realiza una copia de seguridad del sistema o cuando AD DS ejecuta la recolección de elementos no utilizados. Se debe proporcionar una sala principal adicional sobre los cálculos para dar cabida a estos eventos periódicos. El objetivo es proporcionar un rendimiento suficiente para dar cabida a estos escenarios sin afectar a la función normal.

Como se puede ver, hay un límite físico basado en el diseño de almacenamiento para la rapidez con la que la memoria caché puede estar caliente. Lo que calentará la memoria caché son las solicitudes de cliente entrantes hasta la velocidad que puede proporcionar el almacenamiento subyacente. La ejecución de scripts para "precalentamiento" en la memoria caché durante las horas punta proporcionará a la competencia la carga controlada por solicitudes de clientes reales. Esto puede afectar negativamente a la entrega de datos que los clientes necesitan en primer lugar porque, por diseño, generará una competición para los pocos recursos del disco, ya que los intentos artificiales de poner en caliente la memoria caché cargarán los datos que no son relevantes para los clientes que se pongan en contacto con el controlador de dominio.
