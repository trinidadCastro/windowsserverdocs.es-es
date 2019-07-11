---
title: Planear la capacidad de los servicios de dominio de Active Directory
description: Descripción detallada de los factores a tener en cuenta durante el planeamiento de capacidad para AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799837"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planear la capacidad de los servicios de dominio de Active Directory

En este tema se escribió originalmente por Ken Brumfield, ingeniero de campo principal en Microsoft y ofrece recomendaciones para planear la capacidad de los servicios de dominio de Active Directory (AD DS).

## <a name="goals-of-capacity-planning"></a>Objetivos de planeamiento de capacidad

Planificación de capacidad no es igual que la solución de problemas de incidentes de rendimiento. Están estrechamente relacionados, pero es bastante diferente. Los objetivos de planeamiento de capacidad son:  

- Implementar y operar un entorno correctamente 
- Minimizar los problemas de rendimiento para solucionar problemas de tiempo invertido.  
  
En el planeamiento de capacidad, una organización podría tener un destino de línea base 40% de utilización del procesador durante los períodos de actividad con el fin de cumplir los requisitos de rendimiento de cliente y ajustar el tiempo necesario para actualizar el hardware del centro de datos. Mientras que, para recibir una notificación de incidentes de rendimiento anómalo, se podría establecer un umbral de alerta de supervisión en 90% a través de un intervalo de 5 minutos.

La diferencia es que cuando continuamente se supera un umbral de administración de capacidad (un evento único no constituye un problema), agregando capacidad (es decir, al agregar procesadores más rápidos o más) sería una solución o escala el servicio en varios servidores sería un solución. Umbrales de alerta de rendimiento indican que actualmente está experimentando la experiencia del cliente y se necesitan pasos inmediatos para solucionar el problema.

Como una analogía: administración de capacidad consiste en prevenir un accidente de automóvil (defensivas de conducción, asegurándose de que están trabajando los frenos correctamente, y así sucesivamente), mientras que la solución de problemas de rendimiento es lo que hacen la policía, departamento de bomberos y emergencias profesionales médicos Después de un accidente. Se trata sobre "defensiva driving" estilo de Active Directory.

Durante los últimos años, la Guía de planeación de capacidad para sistemas de escalado vertical ha cambiado radicalmente. Los siguientes cambios en las arquitecturas de sistema han cuestionado suposiciones sobre cómo diseñar y escalar un servicio:

- plataformas de servidor de 64 bits  
- Virtualización  
- Una mayor atención al consumo de energía  
- Almacenamiento SSD  
- Escenarios de nube  

Además, el enfoque está cambiando de una ejercicio un ejercicio de planeamiento de capacidad basada en servicios de planeamiento de capacidad basada en servidor. Active Directory Domain Services (AD DS), un servicio distribuido maduro que muchos productos de Microsoft y terceros que se usan como un back-end, se convierte en uno los productos más importantes para planear correctamente garantizar la capacidad necesaria ejecutar otras aplicaciones.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisitos básicos para una guía de planeamiento de capacidad

En este artículo, se esperan que los requisitos básicos siguientes:

- Los lectores leído y está familiarizado con [Performance Tuning directrices para Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La plataforma de Windows Server es un x64 basado en la arquitectura. Pero incluso si su entorno de Active Directory se instala en Windows Server 2003 x86 (ahora después del final del ciclo de vida de soporte técnico) y tiene un árbol de información de directorio (DIT) que es menos 1,5 GB de tamaño y que fácilmente se pueden mantener en memoria, las instrucciones de este artículo siguen siendo aplicables.
- Planeamiento de capacidad es un proceso continuo y deben revisar regularmente también cómo el entorno cumple las expectativas.
- Se producirá la optimización a través de varios ciclos de vida de hardware como cambio de los costos de hardware. Por ejemplo, memoria, se convierte en más barata, disminuye el costo por núcleo o el precio de almacenamiento diferentes opciones de cambio.
- Plan para el período de mucha actividad máxima del día. Se recomienda para examinarlo en intervalos de 30 minutos o de hora. Cualquier valor mayor puede ocultar los picos reales y cualquier cosa que menos podría distorsionarse por "picos transitorios."
- Planee el crecimiento en el transcurso del ciclo de vida de hardware para la empresa. Esto puede incluir una estrategia de actualización o agregar hardware de manera escalonada o una actualización completa cada tres a cinco años. Cada uno requiere una "estimación" cómo crecerá mucho la carga en Active Directory. Datos históricos, si se recopilan, le ayudará con esta evaluación. 
- Plan para la tolerancia a errores. Una vez una estimación *N* se deriva, plan para escenarios que incluyen *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Agregar servidores adicionales según las necesidades organizativas para asegurarse de que la pérdida de un único o varios servidores no supere las estimaciones de capacidad máxima.
  - También considere la posibilidad de que el plan de crecimiento y el plan de tolerancia de error tienen que integrarse. Por ejemplo, si es necesario un controlador de dominio para admitir la carga, pero la estimación es la carga se duplicarán en el próximo año y se requieren dos controladores de dominio totales, no habrá capacidad suficiente para admitir la tolerancia a errores. La solución sería comenzar con tres controladores de dominio. Uno también podría planear agregar el controlador de dominio de tercer después de 3 ó 6 meses si son ajustados presupuestos.

    >[!NOTE]
    >Adición de aplicaciones compatibles con Active Directory podría tener un impacto perceptible en la carga del controlador de dominio, si la carga procede de los servidores de aplicaciones o clientes.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Proceso de tres pasos para el ciclo de planeamiento de capacidad

Planear la capacidad, primero debe decidir qué calidad de servicio es necesario. Por ejemplo, un centro de datos core admite un mayor nivel de simultaneidad y requiere experiencia más coherente para los usuarios y consumir las aplicaciones, lo que requiere mayor atención a la redundancia y minimizar los cuellos de botella del sistema y la infraestructura. En cambio, una ubicación de satélite con unos cuantos usuarios no necesitan el mismo nivel de simultaneidad o la tolerancia a errores. Por lo tanto, la oficina filial quizás no necesite tanta atención a la optimización de la infraestructura, que puede dar lugar a ahorros de costos y hardware subyacente. Todas las recomendaciones e instrucciones en el presente documento son para un rendimiento óptimo y se pueden relajadas selectivamente para escenarios con requisitos menos exigentes.

¿La siguiente pregunta es: virtualizado o físico? Desde una perspectiva de planeación de capacidad, no hay ninguna respuesta correcta o incorrecta; hay solo un diferente conjunto de variables para que funcione con. Escenarios de virtualización bajando a uno de dos opciones:

- "Asignación directa" con un invitado por host (donde virtualización existe únicamente para abstraer el hardware físico del servidor)
- "Host compartido"

Escenarios de producción y pruebas indican que el escenario de "asignación directa" puede tratarse de forma idéntica a un host físico. Sin embargo, se "Compartida host," presenta una serie de cuestiones deletreado con más detalle más adelante. El escenario de "host compartido" significa que AD DS también es competencia por los recursos, y hay penalizaciones y consideraciones acerca del ajuste para hacerlo.

Con estas consideraciones en mente, el ciclo de planeamiento de capacidad es un proceso iterativo de tres pasos:

1. Medir el entorno existente, determinar dónde los cuellos de botella del sistema actualmente son y obtención los conceptos básicos del entorno necesaria planear la capacidad necesaria.
1. Determinar el hardware necesario según los criterios que se describen en el paso 1.
1. Supervisar y validar que la infraestructura implementada funciona dentro de las especificaciones. Algunos datos recopilados en este paso se convierte en la línea de base para el próximo ciclo de planeamiento de capacidad.

### <a name="applying-the-process"></a>Aplicación del proceso

Para optimizar el rendimiento, asegúrese de estos componentes principales se seleccionan y se atento a la aplicación carga correctamente:

1. Memoria
1. Red
1. Almacenamiento
1. Procesador
1. Inicio de sesión de red

Entornos con hasta 10.000 a 20.000 usuarios omitan importantes inversiones en lo que respecta a hardware físico, como casi cualquier servidor moderno de planeamiento de capacidad de permitir que los requisitos de almacenamiento básico de AD DS y el comportamiento general del software cliente bien escrito sistema de la clase va a controlar la carga. Es decir, la tabla siguiente resume cómo evaluar un entorno existente con el fin de seleccionar el hardware adecuado. Cada componente se analiza en detalle en las secciones siguientes para ayudar a los administradores de AD DS a evaluar su infraestructura mediante recomendaciones de línea base y las entidades de seguridad específicos del entorno.

En general:

- Cualquier cambio de tamaño en función de los datos actuales solo serán preciso para el entorno actual.
- Para ver las estimaciones, esperar demanda aumente con el ciclo de vida del hardware.
- Determinar si se encuentra en exceso hoy mismo y crecer en el entorno de mayor tamaño o agregue capacidad durante el ciclo de vida.
- Para la virtualización, las mismas planear entidades de seguridad y las metodologías de la capacidad se aplica, salvo que debe agregarse a cualquier dominio relacionado con la sobrecarga de la virtualización.
- Planificación de capacidad, al igual que cualquier cosa que intenta predecir, no es una ciencia exacta. No espere las cosas para calcular perfectamente y con una precisión de 100%. Las instrucciones aquí son la recomendación contarán; Agregue capacidad por motivos de seguridad adicional y continuamente validar que el entorno permanece en el destino.

### <a name="data-collection-summary-tables"></a>Tablas de resumen de recopilación de datos

#### <a name="new-environment"></a>Nuevo entorno

| Componente | Estimaciones |
|-|-|
|Tamaño de almacenamiento y base de datos|De 40 KB a 60 KB para cada usuario|
|RAM|Tamaño de la base de datos<br />Recomendaciones del sistema operativo base<br />Aplicaciones de terceros|
|Red|1 GB|
|CPU|1000 usuarios simultáneos para cada núcleo|

#### <a name="high-level-evaluation-criteria"></a>Criterios de evaluación de alto nivel

| Componente | Criterios de evaluación | Consideraciones sobre planeación |
|-|-|-|
|Tamaño de almacenamiento y base de datos|La sección titulada "para activar el registro de espacio en disco que se libera por la desfragmentación" en [los límites de almacenamiento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Almacenamiento / rendimiento de la base de datos|<ul><li>"Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg de disco/lectura," "disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg de disco/escritura," " Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg Disk sec/Transfer "</li><li>"Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Reads/sec," "disco lógico ( *\<unidad de base de datos NTDS\>* ) \Writes/sec," "disco lógico ()  *\<Unidad de base de datos NTDS\>* ) \Transfers/sec "</li></ul>|<ul><li>El almacenamiento tiene dos inquietudes<ul><li>Espacio disponible, lo que con el tamaño de eje en función de hoy y SSD de almacenamiento basado en es irrelevante para la mayoría de los entornos de AD.</li> <li>Las operaciones de entrada/salida (E/S) disponibles: en muchos entornos, se trata a menudo se pasa por alto. Pero es importante evaluar solo en entornos donde no hay suficiente RAM para cargar la base de datos NTDS completa en la memoria.</li></ul><li>Almacenamiento puede ser un tema complejo y debe implicar la experiencia de proveedores de hardware para ajustar el tamaño adecuado. Especialmente con escenarios más complejos, como los escenarios de iSCSI, NAS y SAN. Sin embargo, en general, costo por Gigabyte de almacenamiento es a menudo en directo oposición al costo por E/S:<ul><li>RAID 5 tiene el menor costo por Gigabyte que Raid 1, pero Raid 1 tiene un costo menor por E/S</li><li>Unidades de disco duro basada en el eje tienen menor costo por Gigabyte, pero SSDs tienen un costo menor por E/S</li></ul><li>Después de reiniciar el equipo o el servicio de Active Directory Domain Services, la caché del motor de almacenamiento Extensible (ESE) está vacía y el rendimiento será enlazado mientras atiende cada vez en la memoria caché de disco.</li><li>En la mayoría de los entornos AD es E/S intensivas en un patrón aleatorio a los discos, negando gran parte de la ventaja de almacenamiento en caché de lectura y lectura estrategias de optimización.  Además, AD tiene una caché de forma más grande en la memoria que almacena en memoria caché en la mayoría de los sistema de almacenamiento.</li></ul>
|RAM|<ul><li>Tamaño de la base de datos</li><li>Recomendaciones del sistema operativo base</li><li>Aplicaciones de terceros</li></ul>|<ul><li>Almacenamiento es el componente más lento en un equipo. Más que puede ser residente en la memoria RAM, menos es necesario ir al disco.</li><li>Asegúrese de que se asigna suficiente RAM para almacenar el sistema operativo, agentes (antivirus, copia de seguridad, supervisión), base de datos NTDS y crecimiento en el tiempo.</li><li>Para entornos donde maximizar la cantidad de RAM no es costo efectivo (por ejemplo, una ubicación de satélite) o no es factible (DIT es demasiado grande), referencia de la sección de almacenamiento para asegurarse de que el almacenamiento es el tamaño correcto.</li></ul>|
|Red|<ul><li>"Interfaz de red (\*) \Bytes recibidos/seg."</li><li>"Interfaz de red (\*) \Bytes enviados/seg."|<ul><li>En general, el tráfico enviado desde un controlador de dominio ahora supera el tráfico enviado a un controlador de dominio.</li><li>Como una conexión Ethernet conmutada es dúplex completo, tráfico de red entrante y saliente debe tener un tamaño de forma independiente.</li><li>Consolidar el número de controladores de dominio aumentarán la cantidad de ancho de banda utilizado para enviar las respuestas a las solicitudes de cliente para cada controlador de dominio, pero será lo suficientemente cerca lineal para el sitio como un todo.</li><li>Si quita la ubicación de satélite controladores de dominio, no olvide de agregar el ancho de banda de los DC de satélite en el centro de los controladores de dominio, así como para usarlo para evaluar cuánta allí el tráfico WAN será.</li></ul>|
|CPU"Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura"descripti"Process(lsass)\\' tiempo de procesador"tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>Después de la eliminación de almacenamiento como un cuello de botella, dirección de la cantidad de potencia de proceso necesaria.</li><li>Mientras no perfectamente lineal, el número de núcleos de procesador consumido en todos los servidores dentro de un ámbito específico (por ejemplo, un sitio) se puede usar para evaluar cuántos procesadores son necesarios para admitir la carga total del cliente. Agregue el mínimo necesario para mantener el nivel actual del servicio a través de todos los sistemas dentro del ámbito.</li><li>Cambios en la velocidad del procesador, incluida la administración de energía relacionados con los cambios, los números del impacto derivados desde el entorno actual. Por lo general, es imposible evaluar con precisión cómo pasar de un procesador de 2,5 GHz a un procesador a 3 GHz reducirá el número de CPU necesitado.</li></ul>|
|NetLogon|<ul><li>"Netlogon (\*) adquiere \Semaphore"</li><li>"Netlogon (\*) los tiempos de espera \Semaphore"</li><li>"Netlogon (\*) tiempo de espera de semáforo \Average"</li></ul>|<ul><li>Canal seguro de Net Logon/MaxConcurrentAPI solo afecta a los entornos con Autenticaciones NTLM o la validación de PAC. Validación de PAC está activada de forma predeterminada en las versiones de sistema operativo anterior a Windows Server 2008. Se trata de un configuración de cliente, por lo que se verán afectados los controladores de dominio hasta que esto se ha desactivado en todos los sistemas cliente.</li><li>Entornos con la autenticación de confianza entre considerable, que incluye los confianzas entre bosques, tienen mayor riesgo si no es un tamaño correctamente.</li><li>Consolidaciones de servidor aumentará la simultaneidad de confianza entre la autenticación.</li><li>Los aumentos repentinos deben incluirse, por ejemplo, conmutaciones por error de clúster, como los usuarios volver a autentican en masa en el nuevo nodo de clúster.</li><li>Los sistemas cliente individual (por ejemplo, un clúster) necesite demasiado ajuste.</li></ul>|

## <a name="planning"></a>Planificación

Durante mucho tiempo, recomendación de la Comunidad para ajustar el tamaño de AD DS ha sido "put tanta RAM como el tamaño de la base de datos". En su mayor parte, que la recomendación es todo lo que la mayoría de los entornos es necesario preocuparse. Pero el ecosistema de consumo de AD DS se puso mucho mayor, ya tienen los entornos de AD DS por sí mismos, desde su presentación en 1999. Aunque el aumento de capacidad de proceso y el conmutador de x86 arquitecturas para x64 arquitecturas ha realizado los aspectos más sutiles de ajuste de tamaño para tiene rendimiento irrelevante para un conjunto mayor de clientes que ejecutan AD DS en hardware físico, el crecimiento de la virtualización Vuelva a insertar los problemas de optimización a un público más amplio que antes.

La siguiente orientación es, por tanto, acerca de cómo determinar y planeación de las exigencias de Active Directory como un servicio, independientemente de si se implementa en un escenario puramente virtualizado, una combinación de virtuales o físicas o físico. Por lo tanto, se interrumpirá hacia abajo de la evaluación a cada uno de los cuatro componentes principales: procesador, memoria, red y almacenamiento. En resumen, con el fin de maximizar el rendimiento en AD DS, el objetivo es obtener lo más cerca de procesador dependiente como sea posible.

## <a name="ram"></a>RAM

Menos simplemente, más que pueden almacenarse en caché en la memoria RAM, es necesario ir al disco. Para maximizar la escalabilidad del servidor de la cantidad mínima de memoria RAM es la suma del tamaño de base de datos actual, el tamaño total de SYSVOL, el sistema operativo recomendable importe y las recomendaciones del proveedor para los agentes (antivirus, supervisión, copia de seguridad etc. ). Una cantidad adicional debe agregarse para adaptarse al crecimiento durante el ciclo de vida del servidor. Este será el medio ambiente subjetiva según las previsiones de crecimiento de la base de datos en función de los cambios en el entorno.

Para entornos donde maximizar la cantidad de RAM no es costo efectivo (por ejemplo, una ubicación de satélite) o no es factible (DIT es demasiado grande), referencia de la sección de almacenamiento para asegurarse de que el almacenamiento esté diseñada correctamente.

Ajustar el tamaño de un corolario que aparece en el contexto general en memoria de tamaño del archivo de página. En el mismo contexto que todo lo demás relacionados con la memoria, el objetivo es minimizar va el mucho más lentamente disco. Por lo tanto debe ir la pregunta, "¿cómo debería el archivo de paginación ajustarse?" a "Cuánta RAM es necesario para minimizar la paginación?" La respuesta a la pregunta de este última se muestra en el resto de esta sección. Esto deja la mayoría de la discusión para ajustar el tamaño del archivo de paginación al dominio Kerberos de las recomendaciones generales del sistema operativo y la necesidad de configurar el sistema para los volcados de memoria, que no están relacionadas con rendimiento de AD DS.

### <a name="evaluating"></a>Evaluar

La cantidad de RAM que necesita un controlador de dominio (DC) es realmente un ejercicio complejo por estos motivos:

- Alta probabilidad de error al intentar usar un sistema existente para medir la cantidad de RAM es necesario ya se recortan LSASS en situaciones de presión de memoria, artificialmente desinflándose la necesidad.
- El hecho de subjetivo que solo necesita un controlador de dominio individual para almacenar en caché lo que es "interesante" a sus clientes. Esto significa que los datos que deba almacenarse en caché en un controlador de dominio en un sitio con solo un servidor de Exchange será muy diferentes a los datos que deba almacenarse en caché en un controlador de dominio que autentica a los usuarios solo.
- El trabajo que se evalúa de RAM para cada controlador de dominio en el caso por caso es prohibitivo y cambia a medida que cambia el entorno.
- Los criterios de la recomendación le ayudará a tomar decisiones informadas: 
- Más que pueden almacenarse en caché en la memoria RAM, menos es necesario ir al disco. 
- Almacenamiento, sin duda es el componente más lento de un equipo. Acceso a datos basados en el eje y SSD medios de almacenamiento es del orden de 1 000 000 veces más lenta que el acceso a datos en la memoria RAM.

Por lo tanto, con el fin de maximizar la escalabilidad del servidor, la cantidad mínima de memoria RAM de la suma del tamaño de base de datos actual, el tamaño total de SYSVOL, el sistema operativo, conviene importe y las recomendaciones del proveedor para los agentes (antivirus, supervisión, copia de seguridad, y así sucesivamente). Agregar cantidades adicionales para adaptarse al crecimiento durante el ciclo de vida del servidor. Este será el medio ambiente subjetiva basándose en las estimaciones de crecimiento de la base de datos. Sin embargo, las ubicaciones de satélite con un pequeño conjunto de usuarios finales, estos requisitos se pueden relajar como estos sitios no necesitarán caché tanto para la mayoría de las solicitudes de servicio.

Para entornos donde maximizar la cantidad de RAM no es costo efectivo (por ejemplo, una ubicación de satélite) o no es factible (DIT es demasiado grande), referencia de la sección de almacenamiento para asegurarse de que el almacenamiento es el tamaño correcto.

> [!NOTE]
> Un corolario al ajustar el tamaño de memoria es ajustar el tamaño del archivo de página. Dado que el objetivo es minimizar va el mucho más lentamente disco, la pregunta va de "¿cómo debería el archivo de paginación ajustarse?" a "Cuánta RAM es necesario para minimizar la paginación?" La respuesta a la pregunta de este última se muestra en el resto de esta sección. Esto deja la mayoría de la discusión para ajustar el tamaño del archivo de paginación al dominio Kerberos de las recomendaciones generales del sistema operativo y la necesidad de configurar el sistema para los volcados de memoria, que no están relacionadas con rendimiento de AD DS.

### <a name="virtualization-considerations-for-ram"></a>Consideraciones sobre la virtualización de RAM

Evite la confirmación excesiva de memoria en el host. El objetivo fundamental detrás de optimizar la cantidad de RAM es minimizar la cantidad de tiempo invertido en el disco. En escenarios de virtualización, el concepto de confirmación excesiva de memoria existe donde se asigna más memoria RAM a los invitados, entonces existe en el equipo físico. Esto es en sí mismo no es un problema. Se convierte en un problema cuando la memoria total usada activamente por todos los invitados supera la cantidad de RAM en el host y el host subyacente inicia la paginación. Rendimiento se convierte en enlazada al disco en casos donde el archivo NTDS.dit para obtener datos, será el controlador de dominio o el controlador de dominio será el archivo de paginación para obtener datos o el host se va a disco para obtener los datos que considera el invitado en la memoria RAM.

### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

|Componente|Memoria estimada (ejemplo)|
|-|-|
|Sistema operativo base recomienda RAM (Windows Server 2008)|2 GB|
|Tareas internas de LSASS|200 MB|
|Agente de supervisión|100 MB|
|Antivirus|100 MB|
|Base de datos (catálogo Global)|8,5 GB ¿está seguro?|
|Protección para la copia de seguridad ejecutar los administradores para iniciar sesión sin impacto|1 GB|
|Total|12 GB|

**Recomendado: 16 GB**

Con el tiempo, se puede realizar la suposición de que se agregarán más datos a la base de datos y el servidor estará probablemente en producción durante 3 a 5 años. En función de una estimación de crecimiento del 33%, 16 GB sería una cantidad razonable de RAM para colocar en un servidor físico. En una máquina virtual, dada la facilidad con la que puede modificar la configuración y RAM puede agregarse a la máquina virtual, a partir del 12 GB con el plan para supervisar y actualizar en el futuro es razonable.

## <a name="network"></a>Red

### <a name="evaluating"></a>Evaluar
En esta sección es menor acerca de la evaluación de las demandas sobre el tráfico de replicación, que se centra en el tráfico que atraviesa la red WAN y se tratan exhaustivamente en [tráfico de replicación de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), que es acerca de la evaluación total capacidad de ancho de banda y la red es necesaria, incluidas las consultas de cliente, las aplicaciones de la directiva de grupo y así sucesivamente. Para los entornos existentes, se pueden recopilar mediante el uso de contadores de rendimiento "interfaz de red (\*) \Bytes recibidos/seg.," y "interfaz de red (\*) \Bytes enviados/seg." Intervalos de muestra para los contadores de la interfaz de red en 15, 30 o 60 minutos. Cualquier cosa menos suele ser demasiado volátil para buenas medidas; cualquier valor superior se suaviza diario ejecuta el método Peek excesivamente.

> [!NOTE]
> Por lo general, la mayoría del tráfico de red en un controlador de dominio es de salida como el controlador de dominio responde a consultas de cliente. Esta es la razón para el foco en el tráfico saliente, aunque es recomendable para evaluar cada entorno para el tráfico entrante también. Los mismos métodos que pueden usarse para solucionar y revisar los requisitos de tráfico de red entrante. Para obtener más información, vea el artículo de Knowledge Base [929851: El intervalo de puertos dinámicos predeterminado para TCP/IP ha cambiado en Windows Vista y Windows Server 2008](http://support.microsoft.com/kb/929851).

### <a name="bandwidth-needs"></a>Necesidades de ancho de banda

Planear la escalabilidad de red abarca dos categorías distintas: la cantidad de tráfico y la CPU de carga del tráfico de red. Cada uno de estos escenarios es sencillo en comparación con algunos de los otros temas en este artículo.

En la evaluación de la cantidad de tráfico debe ser compatibles, hay dos categorías únicas de planear la capacidad de AD DS en términos de tráfico de red. El primero es el tráfico de replicación que atraviese entre controladores de dominio y se trata exhaustivamente en la referencia [tráfico de replicación de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) y sigue siendo pertinente para las versiones actuales de AD DS. El segundo es el tráfico de cliente a servidor entre sitios. Uno de los escenarios más sencillos para planear principalmente para el tráfico dentro de un sitio recibe las solicitudes pequeñas de clientes con respecto a las grandes cantidades de datos que se envían a los clientes. Suele ser adecuada para entornos de hasta 5.000 usuarios por servidor, en un sitio de 100 MB. Con un adaptador de red de 1 GB y escalado de lado de recepción (RSS) se recomienda soporte para todo lo anterior a 5.000 usuarios. Para validar este escenario, especialmente en el caso de escenarios de consolidación de servidores, examine la interfaz de red (\*) \Bytes/sec entre todos los controladores de dominio en un sitio, sumarlos y dividir por el número objetivo de los controladores de dominio para asegurarse de que no existe es la capacidad adecuada. La manera más fácil de hacerlo es utilizar la vista "Área apilada" en Windows Monitor de confiabilidad y rendimiento (conocido anteriormente como Perfmon), asegurándose de que todos los contadores son escalan el mismo.

Considere el ejemplo siguiente (también conocido como un realmente, realmente complejo forma de validar que la regla general es aplicable a un entorno específico). Se realizan las suposiciones siguientes:

- El objetivo es reducir la superficie para el menor número de servidores como sea posible. Idealmente, efectuará la carga en un servidor y se implementa un servidor adicional para la redundancia (*N* + 1 escenario). 
- En este escenario, el adaptador de red actual admite solo 100 MB y se encuentra en un entorno se ha cambiado.  
  El uso de ancho de banda de red de destino máximo es 60% en un escenario N (pérdida de un controlador de dominio).
- Cada servidor tiene alrededor de 10 000 clientes conectados a él.

Conocimiento adquirido a partir de los datos en el gráfico (interfaz de red (\*) \Bytes enviados/seg.):

1. El día laborable comienza refuerza aproximadamente 5:30 y vientos hacia abajo a las 7:00.
1. El período pico más ocupado es de 8:00 A.M. a 8:15 A.M., con más de 25 Bytes enviados/seg. en el controlador de dominio más ocupado.  
   > [!NOTE]
   > Todos los datos de rendimiento son históricos. Por lo que el punto de datos máxima a las 8:15 indica la carga de 8:00 a las 8:15.
1. Hay picos antes de 4:00 A.M., con más de 20 Bytes enviados/seg. en el controlador de dominio más ocupado, lo que podría indicar cualquier carga de zonas horarias diferentes o en segundo plano de la actividad de la infraestructura, como las copias de seguridad. Puesto que el pico a las 8:00 A.M. supera esta actividad, no es relevante.
1. Hay cinco controladores de dominio en el sitio.
1. La carga máxima es de unos 5.5 MB/s por el controlador de dominio, que representa un 44% de la conexión de 100 MB. Con estos datos, se puede calcular que el ancho de banda total necesario entre las 8:00 A.M. y 8:15 A.M. es 28 MB/s.
   >[!NOTE]
   >Tenga cuidado con el hecho de que son los contadores de interfaz de red enviados o recibidos en bytes y el ancho de banda de red se mide en bits. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusiones:

1. Este entorno actual cumple la N + 1 y nivel de tolerancia a errores al 60% de utilización de destino. Desconectar un sistema desplazará el ancho de banda por cada servidor de unos 5.5 MB/s (44%) unos 7 MB/s (56%).
1. Según el objetivo de consolidar a un servidor se indicó anteriormente, esto supera el uso máximo de destino y, en teoría, el posible uso de una conexión de 100 MB.
1. Con una conexión de 1 GB, esto representará 22% de la capacidad total.
1. Bajo normal operativo condiciones en la *N* + 1 escenario, carga del cliente se distribuirán uniformemente relativamente a aproximadamente 14 MB/s por servidor o un 11% de la capacidad total.
1. Para garantizar que la capacidad es adecuada durante la falta de disponibilidad de un controlador de dominio, los destinos de funcionamiento normales por servidor sería aproximadamente 30% red utilización o 38 MB/s por servidor. Destinos de conmutación por error sería el 60% de uso de red o 72 MB/s por servidor.  
  
En resumen, la implementación de sistemas final debe tener un adaptador de red de 1 GB y estar conectada a una infraestructura de red que admita dicha carga. Una nota adicional es que la cantidad de tráfico de red generado, la carga de CPU de las comunicaciones de red puede tener un impacto significativo y limitar la máxima escalabilidad de AD DS. Este mismo proceso se puede usar para calcular la cantidad de las comunicaciones entrantes para el controlador de dominio. Pero dado el predominio de tráfico saliente en relación con el tráfico entrante, es un ejercicio académico para la mayoría de los entornos. Garantizar que soporte técnico de hardware para RSS es importante en entornos con más de 5.000 usuarios por servidor. Para escenarios con mucho tráfico de red, equilibrio de carga de interrupción puede ser un cuello de botella. Esto se puede detectar por procesador (\*)\% de tiempo de interrupción que se distribuyen desigualmente entre las CPU. RSS habilitado NIC puede suavizar esta limitación y aumentar la escalabilidad.

> [!NOTE]
> Se puede usar un enfoque similar para estimar la capacidad adicional es necesaria al consolidar los centros de datos, o la retirada de un controlador de dominio en una ubicación de satélite. Simplemente recopilar el tráfico entrante y saliente a los clientes y que será la cantidad de tráfico que ahora estará presente en los vínculos WAN.  
>  
> En algunos casos, puede experimentar más tráfico normal, ya que el tráfico es más lento, por ejemplo, cuando la comprobación de certificado no cumple agresivos tiempos de espera en la red WAN. Por este motivo, utilización y WAN tamaño deben ser un proceso iterativo y en curso.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Consideraciones sobre la virtualización de ancho de banda de red

Es fácil hacer recomendaciones para un servidor físico: 1 GB para servidores que admiten más de 5000 usuarios. Una vez que varios invitados comience a compartir una infraestructura subyacente de conmutador virtual, es necesario para asegurarse de que el host tiene ancho de banda de red adecuada para admitir a todos los invitados en el sistema y, por tanto, requiere el rigor adicional atención adicional. Esto no es nada más que una extensión de garantizar la infraestructura de red en el equipo host. Se trata sin importar si la red es favorecer el controlador de dominio que se ejecuta como un invitado de máquina virtual en un host con el tráfico de red a través de un conmutador virtual, o si se conecta directamente a un conmutador físico. El conmutador virtual es sólo un componente más donde el vínculo superior debe admitir la cantidad de datos que se transmiten. Por lo tanto, el adaptador de red físico del host físico vinculado al conmutador debe ser capaz de admitir la carga del controlador de dominio además de todos los otros invitados compartir el conmutador virtual conectado al adaptador de red físico.

### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

|Sistema|Ancho de banda máximo|
|-|-|
DC 1|6.5 MB/s|
DC 2|6,25 MB/s|
|DC 3|6,25 MB/s|
|DC 4|5,75 MB/s|
|DC 5|4,75 MB/s|
|Total|28,5 MB/s|

**Recomendado: MB/s 72** (MB/s 28,5 dividido entre un 40%)

|Número de sistemas de destino|Ancho de banda total (anterior)|
|-|-|
|2|28,5 MB/s|
|Comportamiento normal resultante|28,5 &divide; 2 = 14.25 MB/s|

Como siempre, con el tiempo, se pueden hacer la suposición de que aumentará la carga del cliente y este crecimiento debe planearse para lo mejor posible. Permitiría la cantidad recomendada para planear un crecimiento estimado de tráfico de red del 50%.

## <a name="storage"></a>Almacenamiento

Planeación de almacenamiento constituye dos componentes:

- Capacidad o el tamaño de almacenamiento
- Rendimiento

Una gran cantidad de tiempo y la documentación se dedica a la capacidad de planeación, dejando rendimiento que completamente a menudo se pasa por alto. Con los costos de hardware actuales, mayoría de los entornos no es grandes suficiente que cualquiera de estos es realmente un problema y la recomendación "poner en tanta RAM como el tamaño de la base de datos" normalmente abarca el resto, aunque puede ser una exageración para las ubicaciones de satélite en mayor entornos.

### <a name="sizing"></a>Ajuste de tamaño

#### <a name="evaluating-for-storage"></a>Evaluar para el almacenamiento

En comparación con 13 años atrás cuando se introdujo Active Directory, una vez cuando las unidades de 4 GB y 9 GB eran los tamaños de unidad más comunes, ajuste de tamaño para Active Directory no es par, pero una consideración para todos los entornos más grandes. Con la menor unidad de disco duro tamaños en el intervalo de 180 GB, todo el sistema operativo, SYSVOL y NTDS.dit pueden ajustar fácilmente en una unidad. Por lo tanto, se recomienda dejar de utilizar importantes inversiones en esta área.

La recomendación sola deben tener en cuenta es para asegurarse de que está disponible con el fin de habilitar la desfragmentación 110% del tamaño del archivo NTDS.dit. Además, se deben realizar ajustes para el crecimiento durante la vida útil del hardware.

La consideración primera y más importante es evaluar el tamaño NTDS.dit y SYSVOL será. Estas mediciones daría lugar a la asignación de memoria RAM y disco de tamaño fijada. Debido a la (relativamente) bajo costo de estos componentes, los cálculos matemáticos no necesitan ser rigurosas y precisos. Contenido acerca de cómo evaluar este para entornos nuevos y existentes se pueden encontrar en el [almacenamiento de datos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) serie de artículos. En concreto, consulte los artículos siguientes:

- **Para los entornos existentes &ndash;**  la sección titulada "para activar el registro de espacio en disco que se libera por la desfragmentación" en el artículo [los límites de almacenamiento](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Para los nuevos entornos &ndash;**  el artículo titulado [las estimaciones de crecimiento para usuarios de Active Directory y unidades organizativas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Los artículos se basan en las estimaciones de tamaño de datos realizadas en el momento de la versión de Active Directory en Windows 2000. Usar tamaños de objeto que reflejan el tamaño real de los objetos en su entorno.

Al revisar los entornos existentes con varios dominios, puede haber variaciones en los tamaños de base de datos. Cuando esto ocurre, utilice el menor catálogo global (GC) y tamaños no GC.  

El tamaño de la base de datos puede variar entre las versiones de sistema operativo. Controladores de dominio que ejecutan sistemas operativos anteriores como Windows Server 2003 tiene un tamaño de base de datos menor que un controlador de dominio que ejecuta un sistema operativo posterior, como Windows Server 2008 R2, especialmente cuando las características de reciclaje de tal Active Directory Bin o las credenciales móviles están habilitadas.

> [!NOTE]  
  >
>- Para entornos de nuevo, tenga en cuenta que las estimaciones de crecimiento de las estimaciones de los usuarios de Active Directory y unidades organizativas indican que 100.000 usuarios (en el mismo dominio) consumen aproximadamente 450 MB de espacio. Tenga en cuenta que los atributos que se rellena pueden tener un impacto enorme en la cantidad total. Los atributos se rellenará en muchos objetos por terceros y productos de Microsoft, incluidos Microsoft Exchange Server y Lync. Se prefiere una evaluación basada en la cartera de los productos en el entorno, pero el ejercicio de detalla los cálculos y las pruebas para los entornos más grandes, estimaciones precisas para todos, pero no puede ser realmente vale la pena mucho tiempo y esfuerzo.
>- Asegúrese de que 110% del archivo NTDS.dit tamaño está disponible como espacio disponible con el fin de habilitar sin conexión de desfragmentación y planear el crecimiento durante un periodo de vida de hardware de tres a cinco años. Dado que lo económica almacenamiento es, estimar almacenamiento a un 300% del tamaño del DIT como asignación de almacenamiento es segura acomodar el crecimiento y la necesidad potencial de sin conexión de la desfragmentación.

#### <a name="virtualization-considerations-for-storage"></a>Consideraciones sobre la virtualización de almacenamiento

En un escenario donde se asignan varios archivos de disco duro Virtual (VHD) en un único volumen use fijo disco de tamaño de al menos 210% (100% del DIT + 110% de espacio libre) el tamaño del DIT para asegurarse de que hay suficiente espacio reservado.  

#### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

|Datos recopilados por la fase de evaluación| |
|-|-|
|Tamaño de NTDS.dit|35 GB|
|Desfragmentación de modificador para permitir sin conexión|2.1|
|Espacio total necesario|73,5 GB|

> [!NOTE]
> Este almacenamiento necesario es además el almacenamiento necesario para SYSVOL, sistema operativo, archivo de paginación, los archivos temporales, los datos almacenados en caché locales (por ejemplo, los archivos del instalador) y las aplicaciones.

### <a name="storage-performance"></a>Rendimiento del almacenamiento

#### <a name="evaluating-performance-of-storage"></a>Evaluar el rendimiento de almacenamiento

Como el componente más lento en cualquier equipo, almacenamiento puede tener el mayor impacto negativo en la experiencia del cliente. Para los entornos de gran tamaño suficiente para que las recomendaciones de ajuste de tamaño de RAM no son factibles, las consecuencias de omisión de planificación de rendimiento del almacenamiento pueden ser devastadoras.  Además, las complejidades y variedades de tecnología de almacenamiento más aumentan el riesgo de error tal como está limitada la relevancia de eterno de procedimientos recomendados de "poner el sistema operativo, registros y bases de datos" en discos físicos independientes en sus escenarios útiles.  Esto es porque el eterno procedimiento recomendado se basa en la suposición de que un "disco" es un disco dedicado y esto permite aislar la E/S.  Ya no son relevantes con la introducción de este suposiciones que lo hacen true:

- Nuevos tipos de almacenamiento y escenarios de almacenamiento compartido y virtualizados
- Compartido ejes en una red de área de almacenamiento (SAN)
- Archivo VHD en una SAN o almacenamiento conectado a la red
- Unidades de estado sólido
- Arquitecturas de almacenamiento en capas (es decir, capa de almacenamiento SSD almacenamiento mayor de eje en función de almacenamiento en caché)

En resumen, es el objetivo final de todos los esfuerzos de rendimiento de almacenamiento, independientemente de la arquitectura de almacenamiento subyacente y el diseño garantizar la cantidad necesaria de las operaciones de entrada/salida por segundo (IOPS) están disponibles y que se producen esas IOPS dentro de un período de tiempo aceptable . En esta sección se explica cómo evaluar qué demandas de AD DS del almacenamiento subyacente para garantizar soluciones de almacenamiento están diseñadas correctamente.  Dada la variabilidad de tecnologías de almacenamiento de hoy en día, es mejor trabajar con los proveedores de almacenamiento para asegurarse de IOPS suficiente.  Para esos escenarios con almacenamiento conectado localmente, haga referencia en el apéndice C para los aspectos básicos de cómo diseñar escenarios de almacenamiento local tradicional.  Las entidades de este son generalmente se aplica a las capas de almacenamiento más complejas y ayudará también en el cuadro de diálogo con los proveedores que admiten soluciones de almacenamiento de back-end.

- Dada la amplia variedad de opciones de almacenamiento disponibles, se recomienda para interactuar con la experiencia de los equipos de soporte técnico de hardware o de proveedores para asegurarse de que la solución satisfaga las necesidades de AD DS. Los números siguientes son la información que se les ofrecerá a los especialistas de almacenamiento.

Para entornos donde la base de datos es demasiado grande para incluirse en la memoria RAM, utilice los contadores de rendimiento para determinar cuánto debe admitirse E/S:

- Disco lógico (\*) \Avg segundos de disco/lectura (por ejemplo, si NTDS.dit está almacenado en la unidad D: / unidad, ruta de acceso completa sería \Avg LogicalDisk (D:) segundos de disco/lectura)
- Disco lógico (\*) \Avg de disco/escritura
- Disco lógico (\*) \Avg segundos de disco/transferencia
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- Disco lógico (\*) \Transfers/sec

Estos se deben muestrear en intervalos de 15, 30 o 60 minutos para realizar pruebas comparativas de las demandas del entorno actual.

#### <a name="evaluating-the-results"></a>Evaluar los resultados

> [!NOTE]
> El foco está en las lecturas de la base de datos, como suele ser el componente más exigente, se puede aplicar la misma lógica que escribe en el archivo de registro al sustituir el disco lógico ( *\<NTDS registro\>* ) \Avg segundos de disco/escritura y el disco lógico ( *\<NTDS registro\>* ) \Writes/sec):
>  
> - Disco lógico ( *\<NTDS\>* ) \Avg segundos de disco/lectura indica si el almacenamiento actual tiene el tamaño adecuado.  Si los resultados son aproximadamente iguales a la hora de acceso de disco para el tipo de disco, disco lógico ( *\<NTDS\>* ) \Reads/sec es una medida válida.  Compruebe las especificaciones del fabricante para el almacenamiento en el back-end, pero la buena intervalos para el disco lógico ( *\<NTDS\>* ) \Avg segundos de disco/lectura aproximadamente sería:
>   - 7200 – 9 a 12,5 milisegundos (ms)
>   - 10 000 – 6 a 10 ms
>   - 15 000 – 4 a 6 ms  
>   - SSD – 1 a 3 ms  
>   - >[!NOTE]
>     >Recomendaciones de existen que indica que se degrada el rendimiento del almacenamiento a 15 ms a 20 ms (función de origen).  La diferencia entre los valores anteriores y la otra orientación es que los valores anteriores son el rango operativo normal.  Las demás recomendaciones están solucionando problemas de una guía para identificar cuándo la experiencia del cliente se degrada considerablemente y pasa a ser importante.  Apéndice C de referencia para obtener una explicación más detallada.
> - Disco lógico ( *\<NTDS\>* ) \Reads/sec es la cantidad de E/S que se está realizando.
>   - Si LogicalDisk ( *\<NTDS\>* ) \Avg segundos de disco/lectura está dentro del intervalo óptimo para el almacenamiento de back-end, disco lógico ( *\<NTDS\>* ) \Reads/ s pueden usarse directamente para cambiar el tamaño de almacenamiento.
>   - Si LogicalDisk ( *\<NTDS\>* ) \Avg segundos de disco/lectura no está dentro del intervalo óptimo para el almacenamiento de back-end, se necesita E/S adicional según la siguiente fórmula:
>     > (Disco lógico ( *\<NTDS\>* ) \Avg segundos de disco/lectura) &divide; (tiempo de acceso de disco de medios físicos) &times; (disco lógico ( *\<NTDS\>* ) \Avg de disco/lectura)

Consideraciones:

- Tenga en cuenta que si el servidor está configurado con una cantidad inferior al óptimo de memoria RAM, estos valores serán incorrectos para fines de planificación.  Que será de forma errónea en el lado superior y aún pueden usarse como un escenario del peor caso.
- Agregar/optimización RAM específicamente impulsará una disminución en la cantidad de E/S de lectura (disco lógico ( *\<NTDS\>* ) \Reads/Sec.  Esto significa que la solución de almacenamiento que no tenga que ser tan sólida como calculada inicialmente.  Lamentablemente, nada más específico que esta instrucción general es el medio ambiente depende de la carga del cliente y la orientación general no se puede proporcionar.  Es la mejor opción Ajustar el tamaño de almacenamiento después de optimizar la memoria RAM.

#### <a name="virtualization-considerations-for-performance"></a>Consideraciones sobre la virtualización para el rendimiento

Al igual que todas las discusiones de virtualización anteriores, la clave aquí es para asegurarse de que la infraestructura compartida subyacente puede admitir la carga de controlador de dominio además de los otros recursos mediante subyacente comparten multimedia y todas las rutas a él. Esto es cierto si un controlador de dominio físico comparte el mismo medio subyacente en una SAN, NAS o infraestructura iSCSI que otros servidores o aplicaciones, ya sea un invitado mediante el acceso directo a través del acceso a una infraestructura de SAN, NAS o iSCSI que comparte el subyacente media, o si el invitado está usando un archivo VHD que reside en los archivos multimedia compartidos localmente o en una infraestructura de SAN, NAS o iSCSI. El ejercicio de planificación consiste en asegurarse de que los elementos multimedia subyacentes pueden admitir la carga total de todos los consumidores.

Además, desde una perspectiva de invitado, ya que hay rutas de acceso de código adicionales que deben recorrerse, hay un impacto en el rendimiento al tener que pasar por un host para tener acceso a cualquier almacenamiento. Sin embargo, las pruebas de rendimiento de almacenamiento indican que la virtualización tiene un impacto de rendimiento es subjetivo a la utilización del procesador del sistema host (consulte el apéndice A: Criterios de ajuste de tamaño de CPU), que obviamente se ve afectada por los recursos del host exigidos por el invitado. Esto contribuye a las consideraciones de virtualización con respecto a las necesidades de procesamiento en un escenario virtualizado (consulte [consideraciones de virtualización para el procesamiento](#virtualization-considerations-for-processing)).

Por lo que es más complejo es que hay una variedad de opciones de almacenamiento diferentes que están disponibles todos tienen diferentes repercusiones de rendimiento. Como un cálculo segura cuando la migración de máquinas físicas a virtuales, use un multiplicador de 1,10 para ajustarse a las distintas opciones de almacenamiento para los invitados virtualizados en Hyper-V, como el almacenamiento de acceso directo, un adaptador SCSI o IDE. Los ajustes que deben realizarse cuando se transfieren entre los escenarios de almacenamiento diferentes son irrelevantes en cuanto a si el almacenamiento es local, SAN, NAS o iSCSI.

#### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

Determinar la cantidad de E/S necesaria para un sistema en buen estado en condiciones normales de funcionamiento:

- Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Transfers/sec durante el período máximo 15 período por minuto 
- Para determinar la cantidad de E/S necesaria para el almacenamiento donde se supera la capacidad del almacenamiento subyacente:
  >*Es necesario IOPS* = (LogicalDisk ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura &divide; *\<promedio en segundos/lectura de destino\>* ) &times; LogicalDisk ( *\<unidad de base de datos NTDS\>* ) \Read/sec

|Contador|Valor|
|-|-|
|LogicalDisk real ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/transferencia|0,02 segundos (de 20 milisegundos)|
|Disco lógico de destino ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/transferencia|.01 segundos|
|Multiplicador para el cambio de E/S disponible|0.02 &divide; 0.01 = 2|  
  
|Nombre de valor|Valor|
|-|-|
|Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Transfers/sec|400|
|Multiplicador para el cambio de E/S disponible|2|
|Número total de IOPS necesarios durante el período máximo|800|

Para determinar la velocidad a la que se desea estar preparado la memoria caché:

- Determinar el tiempo máximo aceptable para preparar la memoria caché. Es cualquier cantidad de tiempo que tarda en cargar toda la base de datos desde el disco o para escenarios donde no se puede cargar toda la base de datos en la memoria RAM, esto sería el tiempo máximo para rellenar la memoria RAM.
- Determinar el tamaño de la base de datos, excepto los espacios en blanco.  Para obtener más información, consulte [evaluar para el almacenamiento](#evaluating-for-storage).  
- Divida el tamaño de la base de datos de 8 KB; ese será el total de IOs necesarios para cargar la base de datos.
- Divida el total de IOs por el número de segundos en el período de tiempo determinado.

Tenga en cuenta que la tasa de calculada mientras precisos, no será exacta porque previamente cargadas páginas se expulsan Si ESE no está configurado para tener un tamaño de caché fijo y AD DS de forma predeterminada usa el tamaño de caché de variable.

|Para recopilar los puntos de datos|Valores
|-|-|
|Tiempo máximo aceptable de preparación|10 minutos (600 segundos)
|Tamaño de la base de datos|2 GB|  
  
|Paso de cálculo|Fórmula|Resultado|
|-|-|-|
|Calcular tamaño de base de datos en páginas|(2 GB &times; 1024 &times; 1024) = *tamaño de base de datos en KB*|2097152 KB|
|Calcular el número de páginas de base de datos|KB 2.097.152 &divide; 8 KB = *número de páginas*|262 144 páginas|
|Calcular la IOPS necesarias preparar totalmente la memoria caché|262 144 páginas &divide; = 600 segundos *operaciones IOPS necesarias*|437 IOPS|

## <a name="processing"></a>En proceso

### <a name="evaluating-active-directory-processor-usage"></a>Evaluación del uso de procesador de Active Directory

Para la mayoría de los entornos, después de almacenamiento, RAM y red se optimizan correctamente como se describe en la sección de planeamiento, administración de la cantidad de capacidad de procesamiento será el componente que merece más atención. Existen dos desafíos en la evaluación de la capacidad de CPU necesaria:

- Si las aplicaciones en el entorno se están con buen comportamiento sacarán en una infraestructura de servicios compartidos y se describe en la sección titulada "Seguimiento costosa e ineficaz búsquedas" en el artículo creación más eficaz Microsoft activa Favorece el SAM de nivel inferior o aplicaciones habilitadas para directorio llamadas a las llamadas LDAP.  
  
  En entornos más grandes, el motivo de que esto es importante es que aplicaciones mal codificadas pueden unidad volatilidad en la carga de CPU, "robar" una cantidad excesiva de tiempo de CPU de otras aplicaciones, artificialmente elevar los requisitos de capacidad y distribuir de forma desigual carga frente a los controladores de dominio.  
- Como AD DS es un entorno distribuido mediante una gran variedad de clientes potenciales, calcular que los gastos de "solo cliente" es el medio ambiente subjetivo debido a los patrones de uso y el tipo o la cantidad de aplicaciones para aprovechar AD DS. En resumen, muy similar a la sección de redes, para mayor disponibilidad, esto mejor nos aproximamos a desde la perspectiva de la evaluación de la capacidad total necesaria en el entorno.

Para los entornos existentes, como el ajuste de tamaño de almacenamiento se ha descrito anteriormente, se asume que el almacenamiento ahora es el tamaño correcto y, por tanto, los datos relacionados con la carga del procesador están válidos. Nuevamente, es fundamental para garantizar que el cuello de botella en el sistema no es el rendimiento del almacenamiento. Cuando existe un cuello de botella y el procesador está esperando, hay estados de inactividad desaparecerán una vez eliminado el cuello de botella.  Como se quitan Estados de espera de procesador, por definición, uso de CPU aumenta a medida que ya no tiene que esperar en los datos. Por lo tanto, recopilar contadores de rendimiento "disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura" y "Process(lsass)\\% Processor Time". Los datos de "Process(lsass)\\% Processor Time" suele ser bajo artificialmente si "disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura" supera los 10 a 15 ms, que es un umbral general que Soporte técnico de Microsoft usa para solucionar problemas de rendimiento relacionados con el almacenamiento. Como antes, se recomienda que los intervalos de muestra sea 15, 30 o 60 minutos. Cualquier cosa menos suele ser demasiado volátil para buenas medidas; cualquier valor superior se suaviza diario ejecuta el método Peek excesivamente.

### <a name="introduction"></a>Introducción

Con el fin previsto planear la capacidad de los controladores de dominio, la potencia de procesamiento requiere más atención y descripción. Al cambiar el tamaño de los sistemas para garantizar un rendimiento máximo, siempre es un componente que es el cuello de botella y en un controlador de dominio correctamente con tamaño será el procesador.

Similar a la sección de red donde se revisa la demanda del entorno en el sitio por sitio, la misma debe hacerse para la capacidad de proceso solicitada. A diferencia de la sección de redes, donde las tecnologías de redes disponibles superan en gran medida la demanda normal, prestan más atención a ajustar el tamaño de capacidad de CPU.  Como cualquier entorno de tamaño moderado incluso; nada sobre unos miles de usuarios simultáneos puede colocar una carga considerable en la CPU.

Desafortunadamente, debido a la variabilidad enorme de aplicaciones de cliente que aprovechan AD, una estimación general de los usuarios por CPU es tristemente no aplicable a todos los entornos. En concreto, las demandas de proceso están sujetos a perfil de comportamiento y la aplicación de usuario. Por lo tanto, cada entorno debe tener un tamaño de forma individual.

#### <a name="target-site-behavior-profile"></a>Perfil de comportamiento del sitio de destino

Como se mencionó anteriormente, al planear la capacidad para todo el sitio, el objetivo es seleccionar como destino un diseño con una *N* + el diseño de capacidad de 1, que permitirá error del sistema durante el período máximo de continuación del servicio en razonable nivel de calidad. Esto significa que en un "*N*" escenario, la carga a través de todos los cuadros de debe ser inferior al 100% (mejor aún, menos del 80%) durante los períodos de máxima actividad.

Además, si las aplicaciones y los clientes en el sitio están usando los procedimientos recomendados para la localización de controladores de dominio (es decir, usando la [función DsGetDcName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), los clientes deben distribuirse forma relativamente uniforme con menores picos transitorios debido a una serie de factores.

En el ejemplo siguiente, se realizan las suposiciones siguientes:

- Cada uno de los cinco controladores de dominio en el sitio tiene cuatro de las CPU.
- Uso de CPU durante el horario comercial de destino total es del 40% en condiciones de funcionamiento normales ("*N* + 1") y el 60% de lo contrario ("*N*"). Durante el horario no comercial, el uso de CPU de destino es el 80% porque el software de copia de seguridad y otras tareas de mantenimiento se prevé consumir todos los recursos disponibles.

![Gráfico de uso de CPU](media/capacity-planning-considerations-cpu-chart.png)

Analizar los datos en el gráfico (procesador Information(_Total)\% utilidad procesador) para cada uno de los controladores de dominio:

- En su mayor parte, la carga se distribuye uniformemente relativamente que es lo que se esperaría cuando los clientes usan ubicador de DC y han escrito bien las búsquedas. 
- Hay una serie de cinco minutos picos del 10%, con algunas tan grande como el 20%. Por lo general, a menos que hacen que el destino del plan de capacidad que se supere, investigar estos no es valga la pena.  
- Es el período máximo para todos los sistemas entre aproximadamente 8:00 A.M. y las 9:15 A.M. Con la transición sin problemas desde aproximadamente 5:00 A.M. a través de aproximadamente 5:00 p. M., esto es generalmente indicativo del ciclo de negocio. Los picos de uso de CPU en un escenario de cuadro cuadro entre 5:00 p. M. y 4:00 AM más sería fuera de la capacidad.

  >[!NOTE]
  >En un sistema bien administrado, podría ser son los picos de dicha ejecución de software de copia de seguridad, exámenes de antivirus de todo el sistema, inventario de hardware o software, software o una revisión de implementación y así sucesivamente. Dado que se encuentran fuera del ciclo de negocio de usuario de pico, no se superan los objetivos.

- A medida que cada sistema es aproximadamente el 40% y todos los sistemas tienen los mismos números de CPU, debería uno producirá un error o se desconecta, los sistemas restantes se ejecutaría en un estimado del 53% (carga de un 40% del sistema D es uniformemente divide y agrega carga existente en un 40% del sistema A y del sistema C). Para una serie de motivos, esta suposición lineal no es perfectamente exactas, pero proporciona suficiente precisión para el medidor.  

  **Escenario alternativo:** dos controladores de dominio que se ejecuta en un 40%: Un dominio controlador falla, CPU estimada en uno restante sería un 80% estimado. Esto ahora supera los umbrales descritos anteriormente para el plan de capacidad y también comienza a limitar seriamente la cantidad de espacio principal para el 10% y 20% en el perfil de carga anterior, lo que significa que los picos de actividad podrían controlar el controlador de dominio en el 90% al 100% durante el "*N*"escenario y definitivamente degradar la capacidad de respuesta.

### <a name="calculating-cpu-demands"></a>Calcular las demandas de CPU

El "proceso\\% Processor Time" objeto contador de rendimiento suma la cantidad total de tiempo transcurrido que todos los subprocesos de una aplicación de gastos en la CPU y la hora divide por la cantidad total del sistema. El efecto de esto es que una aplicación multiproceso en un sistema con varias CPU puede superar el 100% de tiempo de CPU y se interpretaría muy distinta de "información de procesador\\% utilidad de procesador". En la práctica el "Process(lsass)\\% de tiempo de procesador" puede verse como el recuento de CPU al 100% que son necesarios para admitir las demandas del proceso. Un valor de 200% significa que se necesitan 2 CPU, cada uno al 100%, para admitir la carga completa de AD DS. Aunque una CPU que se ejecuta en el 100% de la capacidad es más rentable desde la perspectiva del dinero invertido en las CPU y el consumo de potencia y energía, por una serie de motivos detallada en el apéndice A, una mejor capacidad de respuesta en un sistema de varios subprocesos se produce cuando el sistema no se está ejecutando al 100%.

Para dar cabida a picos transitorios en la carga del cliente, se recomienda una CPU período máximo de entre un 40% y un 60% de la capacidad del sistema de destino. Trabajar con el ejemplo anterior, eso significaría que entre 3.33 (60% de destino) y 5 (40% de destino) CPU se necesitan para la carga de AD DS (proceso lsass). En, se debe agregar capacidad adicional según las demandas del sistema operativo base y otros agentes necesarios (por ejemplo, antivirus, copia de seguridad, supervisión y así sucesivamente). Aunque el impacto de los agentes debe evaluarse en una base por entorno, se puede realizar una estimación de entre 5 y 10% de una sola CPU. En el ejemplo actual, esto sugeriría que entre 3.43 (60% de destino) y 5.1 (40% de destino) CPU son necesarias durante períodos de máxima actividad.

La manera más fácil de hacerlo es utilizar la vista del "Área apilada" en Windows Monitor de confiabilidad y rendimiento (perfmon), asegurándose de que todos los contadores son escalan el mismo.

Se asume que:

- Objetivo es reducir la superficie para el menor número de servidores como sea posible. Idealmente, llevaría un servidor de la carga y un servidor adicional que se agrega para redundancia (*N* + 1 escenario).

![Gráfico de tiempo de procesador para el proceso lsass (a través de todos los procesadores)](media/capacity-planning-considerations-proc-time-chart.png)

Conocimiento adquirido a partir de los datos en el gráfico (Process(lsass)\\% de tiempo de procesador):

- El día laborable comienza refuerza aproximadamente 7:00 y disminuye a las 5:00.
- El período pico más ocupado es de 9:30 A.M. a 11:00 AM. 
  > [!NOTE]
  > Todos los datos de rendimiento son históricos. El punto de datos máxima a las 9:15 indica la carga de 9:00 a 9:15.
- Hay picos antes de 7:00 A.M. Esto podría indicar una carga de zonas horarias diferentes o actividad de infraestructura en segundo plano, por ejemplo, las copias de seguridad. Dado que el pico a las 9:30 A.M. supera esta actividad, no es relevante.
- Hay tres controladores de dominio en el sitio.

En la carga máxima, lsass consume aproximadamente 485% de una CPU o 4.85 CPU a 100%. Según los cálculos matemáticos anteriormente, esto significa que el sitio necesita aproximadamente 12,25 CPU para AD DS. Agregar en las sugerencias anteriores del 5 al 10% para los procesos en segundo plano y eso significa reemplazar el servidor de hoy en día necesitaría aproximadamente 12.30 a 12.35 CPU para admitir la carga misma. Una estimación del entorno para el crecimiento Ahora debe tenerse en cuenta.

### <a name="when-to-tune-ldap-weights"></a>Cuándo se debe ajustar los pesos LDAP

Hay varios escenarios donde la optimización [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) deben tenerse en cuenta. En el contexto de planeamiento de capacidad, esto se haría cuando las cargas de usuario o aplicación no están equilibradas o los sistemas subyacentes no están equilibrados en términos de capacidad. Razones para hacerlo más allá de planeamiento de capacidad están fuera del ámbito de este artículo.

Hay dos razones comunes para optimizar las ponderaciones de LDAP:

- El emulador de PDC es un ejemplo que afecta a todos los entornos de qué aplicación o usuario el comportamiento de carga no se distribuye uniformemente. Como determinadas herramientas y las acciones tienen como destino el emulador de PDC, como las herramientas de administración de directiva de grupo, los intentos de segundo en el caso de errores de autenticación, establecimiento de confianza y así sucesivamente, recursos de CPU en el emulador de PDC pueden ser mucho más solicitados que en otro lugar de el sitio.
  - Sólo es útil optimizar esto si hay una diferencia notable en el uso de CPU con el fin de reducir la carga en el emulador de PDC y aumenta la carga en otros controladores de dominio permitirá una distribución más uniforme de carga.
  - En este caso, establezca LDAPSrvWeight entre 50 y 75 para el emulador de PDC.
- Servidores con diferentes recuentos de CPU (y velocidades) en un sitio.  Por ejemplo, supongamos que hay dos servidores de ocho núcleos y un servidor de cuatro núcleos.  El último servidor tiene la mitad de los procesadores de los otros dos servidores.  Esto significa que una carga de cliente distribuida también aumentará la carga promedio de CPU en el cuadro de cuatro núcleos a aproximadamente dos veces que el de los cuadros de ocho núcleos.
  - Por ejemplo, los dos cuadros de ocho núcleos se estarían ejecutando en un 40% y el cuadro de cuatro núcleos se ejecutaría en un 80%.
  - Además, tenga en cuenta el impacto de la pérdida de un cuadro de ocho núcleos en este escenario, específicamente el hecho de que el cuadro de cuatro núcleos podría estar ahora sobrecargado.

#### <a name="example-1---pdc"></a>Ejemplo 1: PDC

| |Uso con los valores predeterminados|Nuevo LdapSrvWeight|Uso estimado de nuevo|
|-|-|-|-|
|1 de DC (emulador de PDC)|53%|57|40 %|
|DC 2|33%|100|40 %|
|DC 3|33%|100|40 %|

El truco es que si se transfiere o asumir, especialmente a otro controlador de dominio en el sitio, el rol de emulador PDC habrá un aumento fenomenal en el nuevo emulador PDC.

Con el ejemplo de la sección [perfil de comportamiento del sitio de destino](#target-site-behavior-profile), se ha realizado una suposición de que todos los tres controladores de dominio en el sitio tenían cuatro CPU. ¿Qué debe suceder, en condiciones normales, si uno de los controladores de dominio tenía ocho CPU? Habrá dos controladores de dominio al 40% de uso y otra al 20% de uso. Aunque esto no es incorrecto, hay una oportunidad para equilibrar la carga de un poco mejor. Aproveche las ponderaciones LDAP para realizar esta acción.  Un escenario de ejemplo sería:

#### <a name="example-2---differing-cpu-counts"></a>Recuentos de CPU que se diferencia de ejemplo 2:

| |Información de procesador\\ %&nbsp;Utility(_Total) de procesador<br />Uso con los valores predeterminados|Nuevo LdapSrvWeight|Uso estimado de nuevo|
|-|-|-|-|
|4-CPU DC 1|40|100|30 %|
|4-CPU DC 2|40|100|30 %|
|8-CPU DC 3|20|200|30 %|

Tenga mucho cuidado con estos escenarios. Como puede verse anteriormente, los cálculos matemáticos busca realmente bueno y más bonito del mundo en papel. Pero en este artículo, planeación de una "*N* + escenario 1" es de gran importancia. El impacto de un controlador de dominio está quedándose sin conexión se debe calcular para cada escenario. En el escenario inmediatamente anterior donde es par, la distribución de carga con el fin de garantizar un 60% de carga durante un "*N*" escenario, con la carga equilibrada uniformemente en todos los servidores, la distribución estará bien las proporciones permanezca coherente. Buscando en el emulador de PDC escenario de optimización y en general en cualquier escenario donde no está equilibrada de carga de usuario o aplicación, el efecto es muy diferente:

| |Uso optimizado|Nuevo LdapSrvWeight|Uso estimado de nuevo|
|-|-|-|-|
|1 de DC (emulador de PDC)|40 %|85|47%|
|DC 2|40 %|100|53%|
|DC 3|40 %|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Consideraciones sobre la virtualización para su procesamiento

Hay dos capas de planeamiento de capacidad que deben realizarse en un entorno virtualizado. En el nivel de host, similar a la identificación del ciclo de negocio descrito para procesar previamente, el controlador de dominio deben identificarse umbrales durante el período pico. Dado que las entidades de seguridad subyacentes son los mismos para un equipo host de programación de subprocesos de invitado en la CPU en cuanto a la introducción de subprocesos de AD DS en la CPU en un equipo físico, se recomienda el mismo objetivo de 40 y 60% en el host subyacente. En el siguiente nivel, el nivel de invitado, desde los principios de programación de subprocesos no han cambiado, el objetivo dentro del invitado permanece en el intervalo de 40 y 60%.

En un escenario directo asignado, un invitado por host, todos los procedimientos de planeamiento de capacidad realizado hasta este momento deben agregarse a los requisitos (RAM, disco, red) del sistema operativo host subyacente. En un escenario de host compartido, las pruebas indican que hay 10% de impacto en la eficacia de los procesadores subyacentes. Esto significa que si un sitio necesita 10 CPU en un destino del 40%, la cantidad recomendada de CPU virtuales para asignar en todos los "*N*" invitados sería 11. En un sitio con una distribución mixta de servidores virtuales y servidores físicos, el modificador solo se aplica a las máquinas virtuales. Por ejemplo, si un sitio tiene un "*N* + 1" escenario, un servidor físico o asignado a direct con 10 CPU sería sobre equivalente a un invitado con 11 CPU en un host, con 11 CPU reservadas para el controlador de dominio.

Durante el análisis y cálculo de las cantidades de CPU necesarias para admitir la carga de AD DS, los números de CPU que se asignan a lo que puede adquirirse en hardware físico de términos no necesariamente se asignan correctamente. Virtualización elimina la necesidad de redondear hacia arriba. La virtualización reduce el esfuerzo necesario para agregar capacidad de proceso a un sitio, dada la facilidad con la que se puede agregar una CPU a una máquina virtual. No se elimina la necesidad de evaluar con precisión la capacidad de proceso necesaria para que el hardware subyacente está disponible cuando la CPU adicionales deben agregarse a los invitados.  Como siempre, no olvide planear y supervisar para el crecimiento de la demanda.

### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

|Sistema|CPU máxima|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total de CPU que se va a usar|485%|  
  
|Número de sistemas de destino|Ancho de banda total (anterior)|
|-|-|
|Es necesarios en el destino de un 40% de CPU|4.85 &divide; .4 = 12.25|

Repetir debido a la importancia de este punto, *acordarse de planear el crecimiento*. Suponiendo que el crecimiento del 50% en los próximos tres años, este entorno será necesario 18.375 CPU (12,25 &times; 1.5) en la marca de tres años. Un plan alternativo sería en revisar después del primer año y agregar capacidad adicional según sea necesario.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carga de cliente de confianza entre la autenticación de NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Evaluar la carga de autenticación de cliente de confianza entre

Muchos entornos pueden tener uno o varios dominios conectados mediante una relación de confianza. Una solicitud de autenticación para una identidad de otro dominio que no utiliza la autenticación Kerberos debe atravesar una confianza mediante el canal seguro del controlador de dominio a otro controlador de dominio en el dominio de destino o en el dominio siguiente de la ruta de acceso en el dominio de destino. El número de llamadas simultáneas mediante el canal seguro que un controlador de dominio puede realizar en un controlador de dominio en un dominio de confianza se controla mediante una configuración conocida como **MaxConcurrentAPI**. Controladores de dominio, lo que garantiza que el canal seguro puede controlar la cantidad de carga se logra mediante uno de estos dos enfoques: optimización **MaxConcurrentAPI** o dentro de un bosque, crear confianzas directas. Para medir el volumen de tráfico a través de una confianza individual, consulte [cómo optimizar el rendimiento de la autenticación NTLM mediante el uso de la configuración de MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante la recopilación de datos, esto, al igual que con todos los demás escenarios, se debe recopilar durante los períodos de ocupado del día para los datos sean útiles.

> [!NOTE]
> En el mismo bosque y escenarios entre bosques pueden hacer que la autenticación recorrer varias relaciones de confianza y cada fase tendría que se deben optimizar.

#### <a name="planning"></a>Planificación

Hay una serie de aplicaciones que usan la autenticación NTLM de forma predeterminada, o usarlo en un escenario de configuración determinados. Los servidores de aplicaciones aumentan de capacidad y creciente número de clientes activos del servicio. También hay una tendencia que los clientes mantener sesiones abierto durante un tiempo limitado y en su lugar, volver a conectarse de forma periódica (por ejemplo, la sincronización de la incorporación de cambios de correo electrónico). Otro ejemplo común para una carga elevada NTLM es servidores proxy web que requieren autenticación para acceder a Internet.

Estas aplicaciones pueden causar una carga significativa para la autenticación NTLM, que puede colocar un esfuerzo significativo en los controladores de dominio, especialmente cuando los usuarios y recursos que se encuentran en dominios diferentes.

Hay varios enfoques para la administración de confianza entre de carga, que en la práctica, se utilizan conjuntamente en lugar de en un exclusivo o / o escenario. Las opciones posibles son:

- Reducir la autenticación del cliente de confianza entre localizando los servicios que consume un usuario en el mismo dominio que reside en el usuario.
- Aumentar el número del seguro canales disponibles. Esto es relevante para el mismo bosque y el tráfico entre bosques y se conocen como las confianzas directas.
- Ajustar la configuración predeterminada para **MaxConcurrentAPI**.

Para la optimización **MaxConcurrentAPI** en un servidor existente, la ecuación es:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Para obtener más información, consulte [2688798 artículo de Knowledge Base: Cómo realizar la optimización del rendimiento de la autenticación NTLM mediante el uso de la configuración de MaxConcurrentApi](http://support.microsoft.com/kb/2688798).

## <a name="virtualization-considerations"></a>Consideraciones de virtualización

Ninguno, esto es un sistema operativo, la configuración de ajuste.

### <a name="calculation-summary-example"></a>Ejemplo de resumen de cálculo

|Tipo de datos|Valor|
|-|-|
|Semáforo adquiere (mínimo)|6,161|
|Semáforo adquiere (máximo)|6,762|
|Tiempos de espera de semáforo|0|
|Tiempo de espera promedio de semáforo|0.012|
|Duración de la colección (segundos)|1:11 minutos (71 segundos)|
|Fórmula (desde 2688798 KB)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Valor mínimo de **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

Para este sistema para este período de tiempo, los valores predeterminados son aceptables.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Supervisión del cumplimiento con los objetivos de planeamiento de capacidad

En este artículo, ya se trató que planear y el escalado van hacia los destinos de uso. Este es un gráfico de resumen de los umbrales recomendados que deben supervisarse para asegurarse de que los sistemas están trabajando dentro de los umbrales de la capacidad adecuada. Tenga en cuenta que estos no son los umbrales de rendimiento, pero los umbrales de planeamiento de capacidad. Un servidor operativo superiores a estos umbrales funcionará, pero es el momento de iniciar la validación de que todas las aplicaciones están bien se comportaban. Si dice que las aplicaciones están bien formadas, es momento de empezar a evaluar las actualizaciones de hardware u otros cambios de configuración.

|Category|Contador de rendimiento|O el intervalo de muestreo|Destino|Advertencia|
|-|-|-|-|-|
|Procesador|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.Procesador Information(_Total)\\"utilidad de procesador Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40 %)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60% otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 o versiones anteriores)|MB de memoria|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long término medio Lifetime(s) en espera de caché|30 min|Se deben probar|Se deben probar|
|Red|Interfaz de red (\*) \Bytes enviados/seg.<br /><br />Interfaz de red (\*) \Bytes recibidos/seg.|30 min|40 %|60%|
|Almacenamiento|Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura<br /><br />Disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg de disco/escritura|60 min|10 ms|15 ms|
|Servicios de AD|Netlogon (\*) tiempo de espera de semáforo \Average|60 min|0|1 segundo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Apéndice A: Criterios de ajuste de tamaño de CPU

### <a name="definitions"></a>Definiciones

**Procesador (microprocesador) –** un componente que lee y ejecuta instrucciones de programa  

**CPU:** unidad Central de procesamiento  

**Procesador de núcleo múltiple:** varias CPU en el mismo circuito integrado  

**Con varias CPU –** varias CPU, no en el mismo circuito integrado  

**Procesador lógico –** un motor informático lógico desde la perspectiva del sistema operativo  

Esto incluye hyper-threading, un núcleo de procesador multinúcleo, o un procesador de núcleo único.  

Como los sistemas de servidor de hoy en día tienen varios procesadores, varios procesadores multinúcleo y hyper-threading, esta información se ha generalizado para cubrir ambos escenarios. Por lo tanto, se usará el procesador lógico del término que representa el sistema operativo y la perspectiva de la aplicación de la informáticos motores disponibles.

### <a name="thread-level-parallelism"></a>Paralelismo de nivel de subproceso

Cada subproceso es una tarea independiente, como cada subproceso tiene su propia pila e instrucciones. Dado que AD DS es multiproceso y el número de subprocesos disponibles se puede ajustar mediante el uso de [cómo ver y configurar la directiva LDAP en Active Directory mediante Ntdsutil.exe](http://support.microsoft.com/kb/315071), se escala bien a través de varios procesadores lógicos.

### <a name="data-level-parallelism"></a>Paralelismo de nivel de datos

Esto implica compartir datos entre varios subprocesos dentro de un proceso (en el caso en el proceso por sí solo en AD DS) y a través de varios subprocesos en varios procesos (en general). Con preocupación para simplificar el caso de exceso, esto significa que los cambios a los datos se reflejan en todos los subprocesos en ejecución en todos los distintos niveles de caché (L1, L2, L3) en todos los núcleos dichos subprocesos en ejecución, así como actualizar la memoria compartida. Puede degradar el rendimiento durante las operaciones de escritura mientras se incorporan todas las ubicaciones de memoria coherente para que pueda continuar el procesamiento de instrucciones.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Velocidad de la CPU frente a las consideraciones de varios núcleos

La regla general es más rápida procesadores lógicos reducen la duración que se tarda en procesar una serie de instrucciones mientras más lógica significa de procesadores que se pueden ejecutar más tareas al mismo tiempo. Estas reglas generales para desglosar los escenarios se vuelven inherentemente más complejos con las consideraciones sobre la obtención de datos de memoria compartida, en el paralelismo de nivel de datos y la sobrecarga de administración de varios subprocesos en espera. Esto también es ¿por qué no es lineal escalabilidad en sistemas de varios núcleos.

Tenga en cuenta la siguientes analogías en estas consideraciones: piense en una carretera, con cada subproceso que sea un automóvil individual, cada calle que se va a un núcleo y el límite de velocidad que se va a la velocidad de reloj.

1. Si hay solo un coche en carretera, no importa si hay dos carriles o 12 calles. Ese automóvil sólo se va a van tan rápido como lo permita el límite de velocidad.
1. Se supone que los datos que necesita el subproceso no están inmediatamente disponibles. La analogía sería que un segmento de carretera está apagada. Si hay solo un coche en carretera, no importa lo que es el límite de velocidad hasta que se vuelve a abrir la pista (se capturan datos de la memoria).
1. A medida que aumenta el número de automóviles, aumenta la sobrecarga para administrar el número de automóviles. Compare la experiencia de conducción y la cantidad de atención es necesario cuando la carretera está prácticamente vacío (por ejemplo, como por la noche) frente a cuando el tráfico es pesado (por ejemplo, la mitad de la tarde, pero no a la hora punta). Además, considere la cantidad de atención es necesario cuando se conduce en una carretera dos carriles, donde hay solo una pista de otro que preocuparse por lo que hacen los controladores, están haciendo frente a una autopista seis pista donde tiene que preocuparse por qué una gran cantidad de otros controladores.
   > [!NOTE]
   > En la sección siguiente, se amplía la analogía sobre el escenario de hora punta: Tiempo de respuesta/cómo los asuntos de sistema afecta al rendimiento.

Como resultado, información específica acerca de los procesadores más rápidos o más se convierten en muy subjetivo al comportamiento de la aplicación, que en el caso de AD DS es medioambiental muy específico e incluso varía en función del servidor dentro de un entorno. Se trata de por qué las referencias anteriormente en este artículo no realizando grandes inversiones en que se va a demasiado precisos y un margen de seguridad se incluye en los cálculos. Al tomar decisiones de compras controlado por el presupuesto, se recomienda que optimizar el uso de los procesadores en un 40% (o el número deseado para el entorno) aparece en primer lugar, antes de pensando en comprar procesadores más rápidos. La sincronización entre los procesadores más mayor reduce la verdadera ventaja de más procesadores de la progresión lineal (2&times; el número de procesadores proporciona menos de 2&times; potencia de proceso adicional disponible).

> [!NOTE]
> Leyes de Amdahl y Ley de Gustafson son los conceptos relevantes aquí.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tiempo de respuesta/cómo los asuntos de sistema afecta al rendimiento

Teoría de puesta en cola es el estudio matemático de líneas de espera (colas). En teoría de puesta en cola, la ley de uso está representada por la ecuación:

*U* k = *B* &divide; *T*

Donde *U* k es el porcentaje de uso, *B* está ocupado, la cantidad de tiempo y *T* es el tiempo total que se ha observado el sistema. Se traduce en el contexto de Windows, esto significa que al número de 100 nanosegundos (ns) subprocesos de intervalo que se encuentran en un estado en ejecución dividido por el número de intervalos de 100 ns, no estaba disponible en dado el intervalo de tiempo. Esto es exactamente la fórmula para calcular la utilidad de procesador % (referencia [objeto procesador](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) y [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Teoría de puesta en cola también proporciona la fórmula: *N* = *U* k &divide; (1 &ndash; *U* k) para calcular el número de elementos en espera en función del uso ( *N* es el longitud de la cola). Esto gráficos de todos los intervalos de uso proporciona las siguientes estimaciones a la cola del procesador en cuánto está en cualquier carga de CPU determinada.

![Longitud de cola](media/capacity-planning-considerations-queue-length.png)

Se observa que después de la carga de CPU del 50%, en promedio siempre hay una espera de otro elemento en la cola, con una rápida considerablemente aumentar después de aproximadamente el 70% de uso de CPU.

Volver a la analogía de conducción usada anteriormente en esta sección:

- Horas de disponibilidad de "mitad de la tarde" hipotéticamente, haría, se dividen en algún lugar en el intervalo de un 40% al 70%. No hay suficiente tráfico que no sea majorly restringe la capacidad de seleccionar cualquier pista y la posibilidad de que otro controlador está en la forma, al alto, no requiere el nivel de esfuerzo para "Buscar" una brecha segura entre otros automóviles en la carretera.
- Se puede observar que como el tráfico aproxima a hora punta, el sistema de carretera aproxima a 100% de su capacidad. Cambiar carriles puede ser muy complicado porque automóviles son tan cerca entre sí que un aumento debe tener precaución para realizar esta acción.

Es por esta razón se calcula el promedio a largo plazo para capacidad conservadora estimado en 40% permite sala principal los picos anómalos en carga, si dice a picos transitorios (por ejemplo, consultas mal codificadas que se ejecutan durante unos minutos) o anómala en general ráfagas (por la mañana de carga el primer día después de un fin de semana largo).

La instrucción anterior que se refiere a calcular el tiempo de procesador % que es el mismo como el derecho de uso es un poco de una simplificación de la facilidad del lector general. Para esos matemáticamente más rigurosos:  
- Traducir el [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = el número de intervalos de 100 ns "Inactivo" subproceso emplea en el procesador lógico. El cambio en el "*X*" de la variable en el [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) cálculo
  - *T* = el número total de intervalos de 100 ns de un intervalo de tiempo determinado. El cambio en el "*Y*" de la variable en el [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) cálculo.
  - *U* k = el porcentaje de utilización del procesador lógico por "Subproceso inactivo" o % de tiempo de inactividad.  
- Elaborar las matemáticas:
  - *U* k = 1: % tiempo de procesador
  - % De tiempo de procesador = 1 – *U* k
  - % De tiempo de procesador = 1 – *B* / *T*
  - % De tiempo de procesador = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Aplicar los conceptos de planeamiento de capacidad

Pueden realizar los cálculos anteriores determinaciones acerca del número de procesadores lógicos necesarios en un sistema parecen demasiado complicadas. Es por esta razón el enfoque para ajustar el tamaño de los sistemas se centra en determinar la utilización máxima de destino basándose en la actual carga y calcular el número de procesadores lógicos necesarios para llegar hasta allí. Además, mientras velocidades del procesador lógico tendrán un impacto significativo en el rendimiento, las eficiencias de memoria caché, los requisitos de coherencia de la memoria, programación de subprocesos y la sincronización y el cliente no entienden bien con equilibrio de carga todos tendrán un impacto significativo rendimiento variará según el servidor a servidor. Con un costo relativamente barato de potencia de proceso, al intentar analizar y determinar el número ideal de CPU necesitado se vuelve más ejercicio académico que aporta valor empresarial.

Cuarenta por ciento no es un requisito absoluta, es un inicio razonable. Los diversos consumidores de Active Directory requieren distintos niveles de capacidad de respuesta. Puede haber escenarios donde pueden ejecutar entornos al 80% o 90% de uso como una media sostenida, como los tiempos de espera mayor para el acceso al procesador no afectará notablemente al rendimiento del cliente. Es importante volver a recorrer en iteración que hay muchas áreas en el sistema que son mucho más lento que el procesador lógico en el sistema, incluido el acceso a la memoria RAM, el acceso a disco y transmitir la respuesta a través de la red. Todos estos elementos deben optimizarse conjuntamente. Ejemplos:

- Agregar más procesadores a un sistema que ejecuta el 90% que está enlazada a un disco probablemente no va a mejorar significativamente el rendimiento. Un análisis más profundo del sistema probablemente identificará que hay una gran cantidad de subprocesos que no se benefician incluso en el procesador porque están esperando finalice la E/S.
- Resolver los problemas de disco enlazados potencialmente significa que los subprocesos que anteriormente estaban dedicar mucho tiempo en estado de espera dejarán de estar en estado de espera de E/S y habrá más competición para el tiempo de CPU, lo que significa que el uso del 90% en el anterior en el ejemplo se pasará al 100% (porque no pueden ir más alto). Es necesario ajustar junto ambos componentes.
  > [!NOTE]
  > Procesador Information(*)\\% utilidad procesador puede superar el 100% con sistemas que tienen un modo "Turbo".  Esto es donde la CPU supera la velocidad del procesador nominal durante breves períodos.  Documentación del fabricante de CPU de referencia y una descripción del contador para una perspectiva más amplia.  

También se tratan consideraciones de uso de todo el sistema pone en los controladores de dominio de la conversación como invitados virtualizados. [Tiempo de respuesta/cómo los asuntos de sistema afecta al rendimiento](#response-timehow-the-system-busyness-impacts-performance) se aplica en el host y el invitado en un escenario virtualizado. Por ello en un host con sólo un invitado, un controlador de dominio (y por lo general, cualquier sistema) tiene casi el mismo rendimiento que lo hace en el hardware físico. Incorporación de invitados adicionales a los hosts, aumenta la utilización del host subyacente, con lo que aumenta los tiempos de espera para obtener acceso a los procesadores, como se explicó anteriormente. En resumen, utilización del procesador lógico debe administrarse en host y en los niveles de invitado.

Extender el anteriores analogías, dejando la autopista como el hardware físico, el Invitado VM se se analogized con un bus (un bus de express que va directamente al destino del piloto desea). Imagine que los cuatro escenarios siguientes:

- Está fuera del horario laboral, obtiene un conductor en un bus que está casi vacío y obtiene el bus en un camino que también está casi vacío. Como no hay ningún tráfico debe lidiar, motociclista tiene una buena andar fácil y llegarán tan rápido como si el conductor había conducido en su lugar. Horas de viaje del conductor aún están restringidas por el límite de velocidad.
- Es fuera del horario laboral para el bus está casi vacío, pero la mayoría de las calles de viaje se cierra, por lo que todavía está congestionada carretera. El conductor está en un bus de casi vacía en una carretera congestionada. Al conductor no tiene una gran cantidad de la competencia en el bus de dónde se encuentran, el tiempo total y todavía está determinado por el resto del tráfico fuera.
- Es por lo que se congestionan carretera y el bus de hora punta. No sólo el recorrido lleva más tiempo, pero llegar y desactivar el bus es una pesadilla porque las personas son hombro a hombros y carretera no es mucho mejor. Agregar más buses (procesadores lógicos del invitado) no significa que caben en la carretera cualquiera más fácilmente, o que el recorrido se truncará.
- El último escenario, aunque puede que se ajuste la analogía un poco, es donde el bus está llena, pero no está de viaje congestionan. Mientras que el conductor seguirás teniendo problemas para obtener y desactivar el bus, será la ida y vuelta eficaz después de que el bus está de viaje. Este es el único escenario donde agregar más buses (procesadores lógicos del invitado) mejorará el rendimiento del invitado.

A partir de ahí es relativamente fácil extrapolar que hay una serie de escenarios entre el 0% utilizado y el estado utilizado por el 100% de la carretera y el estado 0 utilizan % y 100% del bus que tienen distintos grados de impacto.

Aplicar las entidades de seguridad por encima del 40% CPU como objetivo razonable para el host, así como el invitado es un inicio razonable para el mismo razonamiento como anteriormente, la cantidad de puesta en cola.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Apéndice B: Consideraciones relativas a la velocidad de procesador diferente y el efecto de la administración de energía de procesador en las velocidades del procesador

A lo largo de las secciones sobre la selección del procesador se asume que el procesador está ejecutando al 100% de la velocidad del reloj de todo el tiempo que se están recopilando los datos y que los sistemas de reemplazo tendrá los mismos procesadores de velocidad. A pesar de que ambos suposiciones en la práctica que se va a false, especialmente con Windows Server 2008 R2 y versiones posteriores, donde es el plan de energía predeterminado **equilibrado**, la metodología es el acrónimo aún ya que es el enfoque conservador. Aunque puede aumentar la tasa de errores potenciales, solo aumenta el margen de seguridad que aumente la velocidad de procesador.

- Por ejemplo, en un escenario donde 11.25 CPU exigidas, si los procesadores ejecutaban a Media velocidad cuando se recopilaron los datos, la estimación más precisa podría ser 5.125 &divide; 2.
- Es imposible garantizar que duplicar las velocidades de reloj podría duplicar la cantidad de procesamiento que se produce durante un período de tiempo determinado. Esto es debido al hecho que la cantidad de tiempo que los procesadores emplean en esperar RAM u otros componentes del sistema podrían permanecer igual. El efecto neto es que los procesadores más rápidos podrían dedicar un porcentaje mayor que el tiempo de inactividad mientras se espera en los datos que se capturará. De nuevo, se recomienda seguir con el mínimo común denominador, que se va a conservador, y evite tratar de calcular un nivel de exactitud potencialmente false, suponiendo una comparación lineal entre velocidades del procesador.

Como alternativa, si en el hardware de reemplazo de velocidades del procesador son inferiores a hardware actual, sería seguro aumentar la estimación de procesadores que se necesita una cantidad proporcional. Por ejemplo, se calcula que 10 procesadores son necesarios para admitir la carga en un sitio y los procesadores actuales se ejecutan en 3.3 Ghz y procesadores de reemplazo se ejecutarán a 2,6 Ghz, se trata de una disminución del 21% en la velocidad. En este caso, 12 procesadores sería la cantidad recomendada.

Es decir, esta variabilidad no cambiaría los destinos de utilización del procesador de administración de capacidad. Como las velocidades de reloj de procesador se ajustará dinámicamente según la carga exigida, que ejecutan el sistema cargas más altas, generará un escenario donde la CPU dedica más tiempo en un estado de velocidad de reloj superior, hacer que el objetivo final sea al 40% de uso en un 100% estado de la velocidad de reloj en hora punta. Nada menos que generará el ahorro de energía como velocidades de CPU se limitarán volver durante desactivar escenarios de pico.

> [!NOTE]
> Sería una opción desactivar la administración de energía de los procesadores (establecer el plan de energía en **de alto rendimiento**) mientras se recopilan los datos. En el servidor de destino que proporcionaría una representación más precisa del consumo de CPU.

Para ajustar las estimaciones para diferentes procesadores, solía ser seguro, sin incluir otros cuellos de botella del sistema se ha descrito anteriormente, suponga que duplicar las velocidades del procesador duplica la cantidad de procesamiento que se pudo realizar.  Hoy en día, la arquitectura interna de procesadores es bastante diferente entre los procesadores, que es una forma más segura para medir los efectos del uso de diferentes procesadores de datos se ha tomado de aprovechar el banco de pruebas SPECint_rate2006 de evaluación de rendimiento estándar Corporation.

1. Encuentre las puntuaciones SPECint_rate2006 del procesador que están en uso y que se va a utilizarse.
    1. En el sitio Web de la corporación de evaluación de rendimiento estándar, seleccione **resultados**, resalte **CPU2006**y seleccione **buscar todos los resultados de SPECint_rate2006**.
    1. En **solicitud Simple**, escriba los criterios de búsqueda para el procesador de destino, por ejemplo **coincidencias de procesador E5-2630 (baselinetarget)** y **coincidencias de procesador E5-2650(líneadebase)** .
    1. Encontrar la configuración de servidor y el procesador que se usará (o algo cierre, si una coincidencia exacta no está disponible) y anote el valor de la **resultado** y **# núcleos** columnas.
1. Para determinar el modificador de utilizar la siguiente ecuación:
   >((*Valor de puntuación por núcleo de plataforma de destino*) &times; (*MHz por núcleo de la plataforma de línea base*)) &divide; ((*Baseline valor de puntuación por núcleo*) &times; (*MHz por núcleo de la plataforma de destino*))  

    Siguiendo el ejemplo anterior:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiplique al número estimado de procesadores por el modificador.  En el caso anterior para ir desde el E5-2650 procesador del procesador E5-2630 multiplicar las CPU calculadas 11.25 &times; 0,92 = 10,35 procesadores necesitados.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Apéndice C: Aspectos básicos sobre el sistema operativo interactuar con el almacenamiento

Que se describen los conceptos de teoría puesta en cola en [tiempo de respuesta/cómo los asuntos de sistema afecta al rendimiento](#response-timehow-the-system-busyness-impacts-performance) también son aplicables al almacenamiento. Tener un conocimiento de cómo la E/S de identificadores del sistema operativo es necesaria para aplicar estos conceptos. En el sistema operativo Microsoft Windows, se crea una cola para almacenar las solicitudes de E/S para cada disco físico. Sin embargo, debe realizarse una aclaración en disco físico. Los controladores de matriz y SAN presentan las agregaciones de ejes al sistema operativo discos físicos como una sola. Además, SANs y controladores de la matriz pueden agrupar varios discos en el conjunto de una matriz y, a continuación, dividir esta matriz se ha establecido en varias "particiones", que a su vez se presenta al sistema operativo como varios discos físicos (figura ref).

![Ejes de bloque](media/capacity-planning-considerations-block-spindles.png)  

En esta ilustración los dos ejes reflejos y dividir en áreas lógicas de almacenamiento de datos (Data 1 y 2 de datos). Estas áreas lógicas se ven por el sistema operativo como discos físicos independientes.

Aunque esto puede resultar muy confuso, la siguiente terminología se utiliza a lo largo de este apéndice para identificar las diferentes entidades:

- **Eje:** el dispositivo que está instalado físicamente en el servidor.
- **Matriz:** una colección de ejes agregadas por controlador.
- **Matriz de partición:** una partición de la matriz de agregados
- **LUN:** una matriz, que se usa cuando se hace referencia a redes SAN
- **Disco:** lo que observa el sistema operativo a un único disco físico.
- **Partición:** una partición lógica de lo que el sistema operativo percibe como un disco físico.

### <a name="operating-system-architecture-considerations"></a>Consideraciones sobre la arquitectura del sistema operativo

El sistema operativo crea una cola de la primera In/First salir (FIFO) E/S para cada disco que se observa; Este disco puede ser que representa un eje, una matriz o una partición de la matriz. Desde la perspectiva del sistema operativo, con respecto al control de E/S, cuanto más activo se pone en cola con el mejor. Como una cola FIFO se serializa, lo que significa que se deben procesar todas las operaciones de E/s emitidas para el subsistema de almacenamiento en el orden de llegada de la solicitud. Mediante la correlación de cada disco observada por el sistema operativo con una matriz de eje, el sistema operativo ahora mantiene una cola de E/S para cada conjunto único de discos, por tanto, lo que elimina la contención de recursos insuficientes de E/S entre los discos y el aislamiento de la demanda de E/S a un único disco. Como excepción, Windows Server 2008 introduce el concepto de asignación de prioridades de E/S y las aplicaciones diseñadas para usar el "Baja" prioridad quedan fuera de este orden normal y tomar un asiento trasero. Las aplicaciones codificadas no específicamente para aprovechar los predeterminados "Baja" prioridad "Normal".

### <a name="introducing-simple-storage-subsystems"></a>Introducción a los subsistemas de almacenamiento simple

A partir de un ejemplo sencillo (un solo disco duro dentro de un equipo) le ofrecerá un análisis del componente a componente. Dividir en los componentes del subsistema de almacenamiento principal, el sistema consta de:

- **1 –** 10.000 RPM Ultra rápido SCSI HD (Fast Ultra SCSI tiene una velocidad de transferencia de 20 MB/s)
- **1 –** Bus SCSI (el cable)
- **1 –** adaptador SCSI rápido ultra
- **1 –** bus PCI de 33 MHz de 32 bits

Una vez identificados los componentes, una idea de la cantidad de datos puede enrutar el sistema o se pueden controlar la cantidad de E/S, se puede calcular. Tenga en cuenta que se correlacionan la cantidad de E/S y la cantidad de datos que pueden enrutar el sistema, pero no es el mismo. Esta correlación depende de si la E/S del disco es aleatorio o secuencial y el tamaño de bloque. (Todos los datos se escriben en el disco como un bloque, pero diferentes aplicaciones con diferentes tamaños de bloque). Según el componente a componente:

- **La unidad de disco duro** la unidad de disco duro promedio de 10.000 RPM tiene 7 milisegundos (ms) tiempo de búsqueda y una hora de acceso de ms 3. Búsqueda de tiempo es la cantidad media de tiempo que tarda el cabezal de lectura/escritura para desplazarse a una ubicación en el disco. Tiempo de acceso es la cantidad media de tiempo necesario para leer o escribir los datos en disco, una vez que el encabezado está en la ubicación correcta. Por lo tanto, el tiempo medio para leer un único bloque de datos en un HD 10.000 RPM constituye un acceso, para un total de aproximadamente 10 ms (o.010 segundos) y realizar una búsqueda por el bloque de datos.

  Cuando cada acceso al disco requiere movimiento de la cabeza a una nueva ubicación en el disco, el comportamiento de lectura/escritura se conoce como "aleatorios". Por lo tanto, cuando todas las E/S es aleatorio, una HD 10.000 RPM puede controlar aproximadamente 100 E/S por segundo (IOPS) (la fórmula es 1000 ms por segundo dividido entre 10 ms por E/S o 1000/10 = 100 IOPS).

  Como alternativa, cuando se produce toda la E/S de sectores adyacentes en el disco duro, esto se conoce como E/S secuenciales. E/S secuencial no tiene ningún tiempo de búsqueda porque una vez completada la primera E/S, el cabezal de lectura/escritura está al principio de donde se almacena el siguiente bloque de datos en el disco duro. Por lo tanto es capaz de controlar 333 aproximadamente un HD 10.000 RPM E/S por segundo (1000 ms por segundo dividido entre 3 ms por E/S).

  >[!NOTE]
  >En este ejemplo no refleja la caché de disco, que normalmente se conservan los datos de un cilindro. En este caso, se necesitan los 10 ms en la primera E/S y el disco lee el cilindro todo. Todas las demás E/S secuencial se atienden desde la caché. Como resultado, las cachés de disco pueden mejorar el rendimiento de E/S secuencial.
  
  Hasta ahora, la velocidad de transferencia del disco duro ha sido irrelevante. Si la unidad de disco duro es 20 MB/s Ultra amplia o una Ultra3 160 MB/s, la cantidad real de e/s por segundo puede controlarse mediante el disco duro de 10.000 RPM es ~ 100 aleatorio o E/S secuenciales de ~ 300. Cambian los tamaños de bloque según la aplicación escribiendo en la unidad, la cantidad de datos que se extraen por E/S es diferente. Por ejemplo, si el tamaño de bloque es de 8 KB, 100 operaciones E/S leer o escribir en el disco duro de un total de 800 KB. Sin embargo, si el tamaño de bloque es 32 KB, 100 E/S se lectura/escritura 3.200 KB (3,2 MB) en el disco duro. Siempre que sea la velocidad de transferencia de SCSI que exceda el importe total de los datos transferidos, obtener a una transferencia "más rápida" unidad de frecuencia obtendrán nada. Consulte las siguientes tablas para la comparación.

  | |Acceso de búsqueda de 7200 RPM 9ms 4ms estándar|Acceso de 10.000 seek 7 ms. RPM, 3 ms.|Acceso de búsqueda de 15.000 RPM 4ms 2 ms
  |-|-|-|-|
  |E/S aleatorias|80|100|150|
  |E/S secuencial|250|300|500|  
  
  |Unidad de 10.000 RPM|Tamaño de bloque de 8 KB (Jet de Active Directory)|
  |-|-|
  |E/S aleatorias|800 KB/s|
  |E/S secuencial|2400 KB/s|

- **Backplane de SCSI (bus):** comprender cómo el "SCSI backplane (bus)", o en este escenario, el cable de la cinta de opciones, el rendimiento de impactos del subsistema de almacenamiento depende de conocimiento del tamaño del bloque. Sería esencialmente la pregunta, ¿cuánto E/S puede el controlador de bus si es la E/S en bloques de 8 KB? En este escenario, el bus SCSI es 20 MB/s o 20480 KB/seg. 20480 KB/s dividida por bloques de 8 KB, se produce un máximo de aproximadamente las IOPS de 2500 compatibles con el bus SCSI.

  >[!NOTE]
  >Las cifras de la tabla siguiente representan un ejemplo. Más dispositivos de almacenamiento conectados actualmente usan PCI Express, que proporciona un rendimiento mucho mayor.  
  
  |Compatible con bus SCSI por tamaño de bloque de E/S|Tamaño de bloque de 2 KB|Tamaño de bloque de 8 KB (AD Jet) (SQL Server 7.0 o SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  Como se puede determinar en este gráfico, en el escenario presentado, independientemente de lo que el uso, el bus nunca será un cuello de botella, como el eje máximo es 100 operaciones de E/S, acercarse a cualquiera de los umbrales anteriores.

  >[!NOTE]
  >Esto supone que el bus SCSI es 100% eficaz.
  
- **Adaptador SCSI:** para determinar la cantidad de E/S que puede controlar esto, deben comprobarse las especificaciones del fabricante. Dirigir las solicitudes de E/S para el dispositivo adecuado requiere el proceso de algún tipo, por lo tanto es dependiente en el adaptador SCSI (o el controlador de la matriz) la cantidad de E/S que se pueden administrar procesador.

  En este ejemplo, la suposición de que puede E/S 1.000 controlarse se realizarán.

- **Bus PCI-** se trata de un componente a menudo se pasa por alto. En este ejemplo, esto no será el cuello de botella; Sin embargo como sistemas de escalan verticalmente, puede convertirse en un cuello de botella. Como referencia, un bus PCI de 32 bits funciona a 33Mhz puede transferencia teoría 133 MB/s de datos. La siguiente es la ecuación:  
  > 32 bits &divide; 8 bits por byte &times; 33 MHz = 133 MB/s.  

  Tenga en cuenta que es el límite teórico; en realidad sólo alrededor del 50% de los máximos que realmente se alcance, aunque en ciertos escenarios de ráfaga, se puede obtener la eficacia del 75% durante períodos cortos.

  Un bus PCI de 66 Mhz 64-bit puede admitir un máximo teórico de (64 bits &divide; 8 bits por byte &times; 66 Mhz) = 528 MB/seg. Además, cualquier otro dispositivo (por ejemplo, el adaptador de red, segundo controlador de SCSI etc.) reducirá el ancho de banda disponible Cuando se comparte el ancho de banda y se enfrentarán los dispositivos para los recursos limitados.

Después del análisis de los componentes de este subsistema de almacenamiento, el eje es el factor limitador en la cantidad de E/S que se puede solicitar y, por consiguiente, la cantidad de datos que pueden enrutar el sistema. En concreto, en un escenario de AD DS, esto es 100 E/S aleatorias por segundo en incrementos de 8 KB, para un total de 800 KB por segundo cuando accede a la base de datos Jet. Como alternativa, el rendimiento máximo para un eje que está asignado exclusivamente para los archivos de registro se verían afectadas por las siguientes limitaciones: 300 E/S secuenciales por segundo en incrementos de 8 KB, con un total de 2400 KB (2,4 MB) por segundo.

Ahora, tener analizar una configuración simple, en la tabla siguiente se muestra donde se producirá un cuello de botella cuando se cambia o agrega los componentes en el subsistema de almacenamiento.

|Notas|Análisis de cuello de botella|Disk|Bus|Adaptador|Bus PCI|
|-|-|-|-|-|-|
|Se trata de la configuración del controlador de dominio después de agregar un segundo disco. La configuración del disco representa el cuello de botella en 800 KB/seg.|Agregar 1 disco (Total = 2)<br /><br />E/S es aleatoria<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM|200 E/s total<br />Total de 800 KB/seg.| | | |
|Después de agregar 7 discos, la configuración del disco sigue representando el cuello de botella en KB/seg. 3200.|**Agregar 7 discos (Total = 8)**  <br /><br />E/S es aleatoria<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM|Total de 800 E/s.<br />Total de 3200 KB/seg.| | | |
|Después de cambiar la E/S secuencial, el adaptador de red se convierte en el cuello de botella porque está limitado a 1000 IOPS.|Agregar 7 discos (Total = 8)<br /><br />**E/S es secuencial**<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM| | |2400 E/S s pueden ser leídos/escritos en el disco, controlador limitado a 1000 IOPS| |
|Después de reemplazar el adaptador de red con un adaptador SCSI que es compatible con 10 000 IOPS, el cuello de botella se devuelve a la configuración del disco.|Agregar 7 discos (Total = 8)<br /><br />E/S es aleatoria<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM<br /><br />**Actualizar adaptador SCSI (ahora es compatible con 10 000 E/S)**|Total de 800 E/s.<br />Total de 3.200 KB/seg.| | | |
|Después de aumentar el tamaño de bloque a 32 KB, el bus de cuello de botella, ya que sólo admite 20 MB/s.|Agregar 7 discos (Total = 8)<br /><br />E/S es aleatoria<br /><br />**Tamaño de bloque de 32 KB**<br /><br />HD A 10.000 RPM| |Total de 800 E/s. 25.600 KB/s (25 MB/s) pueden ser leídos/escritos en el disco.<br /><br />El bus solo admite 20 MB/s| | |
|Después de actualizar el bus y agregar más discos, el disco sigue siendo el cuello de botella.|**Agregar 13 discos (Total = 14)**<br /><br />Agregar el segundo adaptador SCSI con 14 discos<br /><br />E/S es aleatoria<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM<br /><br />**Actualizar a 320 bus SCSI MB/s**|Operaciones de E/s 2800<br /><br />11,200 KB/seg. (10.9 MB/s)| | | |
|Después de cambiar la E/S secuencial, el disco sigue siendo el cuello de botella.|Agregar 13 discos (Total = 14)<br /><br />Agregar el segundo adaptador SCSI con 14 discos<br /><br />**E/S es secuencial**<br /><br />Tamaño de bloque de 4 KB<br /><br />HD A 10.000 RPM<br /><br />Actualizar a 320 bus SCSI MB/s|8,400 E/s<br /><br />33.600 KB\s<br /><br />(32.8 MB\s)| | | |
|Después de agregar unidades de disco duro con mayor rapidez, el disco sigue siendo el cuello de botella.|Agregar 13 discos (Total = 14)<br /><br />Agregar el segundo adaptador SCSI con 14 discos<br /><br />E/S es secuencial<br /><br />Tamaño de bloque de 4 KB<br /><br />**HD A 15.000 RPM**<br /><br />Actualizar a 320 bus SCSI MB/s|14.000 E/s<br /><br />56.000 KB/seg.<br /><br />(54,7 MB/s)| | | |
|Después de aumentar el tamaño de bloque a 32 KB, el bus PCI se convierte en el cuello de botella.|Agregar 13 discos (Total = 14)<br /><br />Agregar el segundo adaptador SCSI con 14 discos<br /><br />E/S es secuencial<br /><br />**Tamaño de bloque de 32 KB**<br /><br />HD A 15.000 RPM<br /><br />Actualizar a 320 bus SCSI MB/s| | | |14.000 E/s<br /><br />448,000 KB/seg.<br /><br />(437 MB/s) es el límite de lectura/escritura para el eje.<br /><br />El bus PCI admite un máximo teórico de 133 MB/s (75% eficaz en el mejor).|

### <a name="introducing-raid"></a>Presentación de RAID

La naturaleza de un subsistema de almacenamiento no cambia drásticamente cuando se introduce un controlador de la matriz; simplemente reemplaza el adaptador SCSI en los cálculos. ¿Cambia lo que es el costo de leer y escribir datos en el disco al usar los distintos niveles de matriz (por ejemplo, RAID 0, RAID 1 o RAID 5).

En RAID 0, los datos se dividen en todos los discos en RAID establecidos. Esto significa que, durante una lectura o una operación de escritura, una parte de los datos se extraen de o insertan en cada disco, aumentar la cantidad de datos que pueden enrutar el sistema durante el mismo período de tiempo. Por lo tanto, en un segundo en cada eje (suponiendo que vuelva a unidades de 10.000 RPM), 100 operaciones de E/S se pueden realizar. La cantidad total de E/S que se admiten es N ejes x 100 E/S por segundo por el eje (100 * N produce E/S por segundo).

![Unidad lógica d:](media/capacity-planning-considerations-logical-d-drive.png)

En RAID 1, los datos está reflejados (duplica) en un par de ejes para redundancia. Por lo tanto, cuando se realiza una operación de E/S de lectura, se pueden leer datos desde ambos de los ejes del conjunto. Esto lo convierte la capacidad de E/S desde ambos discos disponibles durante una operación de lectura. La advertencia es que no escribir operaciones ganancia ninguna ventaja de rendimiento en un RAID 1. Esto es porque deben escribirse en ambas unidades por motivos de redundancia de los mismos datos. Aunque no tarda más largo, como la escritura de datos tiene lugar simultáneamente en ambos ejes, porque ambos ejes están ocupadas duplicar los datos, una operación de E/S de escritura en esencia impide que dos operaciones de lectura se produzcan. Por lo tanto, todos los costos de E/S de escritura dos E/S de lectura. Una fórmula puede crearse desde esa información para determinar el número total de operaciones de E/S que se producen:  

> *E/S de lectura* + 2 &times; *E/S de escritura* = *Total de E/S disponible en disco consumido*  

Cuando la proporción de lecturas de escrituras, y se conoce el número de ejes, la siguiente ecuación se puede derivar de la ecuación anterior para identificar la E/S máxima que pueden ser compatibles con la matriz:  

> *Número máximo de IOPS por eje* &times; 2 ejes &times; [( *% de lecturas* +  *% de escrituras*) &divide; ( *% de lecturas* + 2 &times; *% de escrituras*)] = *IOPS totales*

RAID 1 + 0, se comporta exactamente igual que RAID 1 con respecto a los gastos de lectura y escritura. Sin embargo, la E/S ahora se secciona en cada conjunto reflejado. Si  

> *Número máximo de IOPS por eje* &times; 2 ejes &times; [( *% de lecturas* +  *% de escrituras*) &divide; ( *% de lecturas* + 2 &times; *% de escrituras*)] = *Total de E/S*  

en un conjunto RAID 1, cuando una multiplicidad (*N*) de RAID 1 se seccionan conjuntos, se convierte en el Total de E/S que se pueden procesar N &times; E/S por conjunto RAID 1:  

> *N* &times; {*IOPS máximo por eje* &times; 2 ejes &times; [( *% de lecturas* +  *% de escrituras*) &divide; ( *% De lecturas* + 2 &times; *% de escrituras*)]} = *IOPS totales*

RAID 5, conocido también como *N* + 1 RAID, los datos se seccionan en varios *N* ejes y paridad se escribe información en el eje "+ 1". Sin embargo, RAID 5 es mucho más costoso cuando se realiza una E/S de escritura que RAID 1 o 1 + 0. RAID 5 lleva a cabo el siguiente proceso cada vez que una E/S de escritura se envía a la matriz:

1. Leer los datos antiguos
1. Leer la paridad antigua
1. Escribir los nuevos datos
1. Escribir la paridad nuevo

Dado que cada solicitud de E/S de escritura que se envía al controlador de la matriz por el sistema operativo requiere cuatro operaciones de E/S para completar, las solicitudes de escritura enviadas tardan cuatro veces para completar como E/S de lectura de una sola. Para derivar una fórmula para traducir las solicitudes de E/S desde la perspectiva del sistema operativo para experimentan los ejes:  

> *E/S de lectura* + 4 &times; *la E/S de escritura* = *Total de E/S*  

De forma similar en un conjunto RAID 1, cuando la proporción de lecturas, escrituras y el número de ejes se conocen, la siguiente ecuación se puede derivar de la ecuación anterior para identificar la E/S máxima que pueden ser compatibles con la matriz (tenga en cuenta que no incluyen el número total de ejes la "unidad" e perdido con paridad):  

> *IOPS por eje* &times; (*ejes* – 1) &times; [( *% de lecturas* +  *% de escrituras*) &divide; ( *% De lecturas* + 4 &times; *% de escrituras*)] = *IOPS totales*

### <a name="introducing-sans"></a>Introducción a SAN

Al expandir la complejidad del subsistema de almacenamiento, cuando se introduce una SAN en el entorno, los principios básicos descritos no cambian, pero el comportamiento de E/S para todos los sistemas conectados a la SAN deben tenerse en cuenta. Como una de las principales ventajas de usar una SAN es una cantidad adicional de redundancia a través de almacenamiento de conexión interna o externamente, ahora planeamiento de capacidad debe tener en cuenta necesidades de tolerancia a errores. Además, se presentan varios componentes que deben evaluarse. División de una red SAN en los componentes:

- Unidad de disco duro SCSI o Fibre Channel
- Backplane de canal de unidad de almacenamiento
- Unidades de almacenamiento
- Módulo de controlador de almacenamiento
- Conmutadores de SAN
- HBA(s)
- El bus PCI

Al diseñar cualquier sistema para la redundancia, componentes adicionales se incluyen para dar cabida a la posibilidad de error. Es muy importante, cuando planee la capacidad, para excluir el componente redundante de los recursos disponibles. Por ejemplo, si la red SAN tiene dos módulos de controlador, la capacidad de E/S del módulo de un controlador es todo lo que debe usarse para el rendimiento total de E/S disponible para el sistema. Esto es debido al hecho de que si se produce un error en un controlador, toda la carga de E/S exigida por todos los sistemas conectados deberá ser procesados por el otro controlador. Como todo el planeamiento de capacidad se realiza para los períodos de uso, los componentes redundantes no deben tenerse en cuenta en los recursos disponibles y planeado pico de uso no debe superar los 80% de saturación del sistema (para dar cabida a ráfagas o comportamientos anómalos sistema comportamiento). De forma similar, el modificador, unidad de almacenamiento y ejes redundantes de SAN no pueden incluirse en los cálculos de E/S.

Al analizar el comportamiento de la unidad de disco duro SCSI o Fibre Channel, el método de análisis del comportamiento, tal como se describe anteriormente no cambia. Aunque hay ciertas ventajas y desventajas para cada protocolo, el factor limitador por disco es la limitación mecánica de la unidad de disco duro.

Analizar el canal en la unidad de almacenamiento es exactamente el mismo que calcular los recursos disponibles en el bus SCSI o ancho de banda (por ejemplo, 20 MB/s) dividido por el tamaño de bloque (por ejemplo, 8 KB). Donde ésta se desvía del ejemplo anterior se encuentra en la agregación de varios canales. Por ejemplo, si hay 6 canales, cada uno admitiendo la velocidad de transferencia máxima de 20 MB/s, la cantidad total de E/S y transferencia de datos que está disponible es 100 MB/s (Esto es correcto, es no 120 MB/s). Nuevamente, tolerancia a errores es un reproductor principal en este cálculo, si se produce la pérdida de un canal completo, el sistema está único que queda con 5 canales funcione. Por lo tanto, para asegurarse de que continúe satisfacer las expectativas de rendimiento en caso de error, el rendimiento total para todos los canales de almacenamiento no debe superar 100 MB/s (Esto supone carga y tolerancia a errores se distribuye uniformemente entre todos los canales). Convertir esto en un perfil de E/S es dependiente del comportamiento de la aplicación. En el caso de E/S de Jet de Active Directory, esto estaría relacionado con aproximadamente 12.500 E/S por segundo (100 MB/s &divide; 8 KB por E/S).

A continuación, se requiere la obtención de las especificaciones del fabricante para los módulos de controlador con el fin de comprender el rendimiento que puede admitir cada módulo. En este ejemplo, la red SAN tiene dos módulos de controlador esa E/S 7500 de soporte técnico. El rendimiento total del sistema puede ser de 15 000 e/s por segundo, si no se desea redundancia. Para calcular el rendimiento máximo en el caso de error, la limitación es el rendimiento de un controlador o 7500 e/s por segundo. Este umbral está muy por debajo de la máxima 12.500 de e/s por segundo (suponiendo que el tamaño de bloque de 4 KB) que puede ser compatibles con todos los canales de almacenamiento y, por lo tanto, está el cuello de botella en el análisis. Todavía para fines de planificación, la E/S máxima deseada para planear, sería 10.400 E/S.

Cuando los datos cierra el módulo del controlador, transita una conexión de canal de fibra clasificada con 1 GB/s (o de 1 Gigabit por segundo). Para correlacionar esto con las demás métricas, 1 GB/s se convierte en 128 MB/s (1 GB/s &divide; 8 bits/bytes). Como esto es que supere el ancho de banda total en todos los canales en la unidad de almacenamiento (100 MB/s), esto algún no cuello de botella del sistema. Además, ya que es sólo uno de los dos canales (el 1 GB/s Fibre Channel conexión adicionales para redundancia), si se produce un error en una conexión, la conexión restante todavía tiene suficiente capacidad para controlar todas la transferencia de datos solicitada.

En tránsito al servidor, los datos se tránsito más probable es que un conmutador de SAN. Como el conmutador de SAN tiene que procesar la solicitud entrante de E/S y reenviar al puerto correspondiente, el conmutador tendrá un límite para la cantidad de E/S que se pueden administrar, sin embargo, las especificaciones de los fabricantes será necesarias para determinar qué es ese límite. Por ejemplo, si hay dos conmutadores y cada modificador puede controlar 10 000 IOPS, el rendimiento total será 20 000 IOPS. De nuevo, la tolerancia a errores que se va a un problema, si un conmutador se produce un error, el rendimiento total del sistema será 10 000 IOPS. Como se desee no debe superar los 80% de utilización en el funcionamiento normal, con no más de 8000 E/S debe ser el destino.

Por último, el HBA instalado en el servidor también tendría un límite para la cantidad de E/S que puede controlar. Normalmente, se instala una segunda HBA para redundancia, pero al igual que con el modificador de SAN, al calcular el E/S máximo que pueden controlarse en el rendimiento total de *N* &ndash; HBA 1 es ¿cuál es la máxima escalabilidad del sistema.

### <a name="caching-considerations"></a>Consideraciones de almacenamiento en caché

Las memorias caché son uno de los componentes que pueden afectar significativamente al rendimiento global en cualquier momento en el sistema de almacenamiento. Análisis detallado sobre algoritmos de almacenamiento en caché está fuera del ámbito de este artículo; Sin embargo, algunas instrucciones básicas sobre el almacenamiento en caché en subsistemas de disco valen ilumina:

- Almacenamiento en caché realiza E/S de escritura secuencial sostenida mejorado tal como se puede muchas operaciones de escritura más pequeñas en bloques más grandes de E/S del búfer y ensayar al almacenamiento en tamaños de bloque de menos, pero más grandes. Esto reducirá total E/S aleatorias y total de E/S secuenciales, lo que proporciona más disponibilidad de recursos para otra E/S.
- Almacenamiento en caché no mejora el rendimiento de E/S de escritura sostenida del subsistema de almacenamiento. Solo se permite para que las escrituras se almacenen en búfer hasta que los ejes que están disponibles para confirmar los datos. Cuando todas las E/S disponible de los ejes en el subsistema de almacenamiento está saturado durante largos períodos, finalmente se llenará la memoria caché. Para vaciar la memoria caché, suficiente tiempo entre ráfagas o ejes adicionales, debe ser asignado para proporcionar suficientes E/S para permitir que la memoria caché vaciar.

  Las cachés de mayor solo quepan más datos se almacenen en búfer. Esto significa que se pueden incluir períodos más largos de saturación.

  En un subsistema de almacenamiento funcione de manera normal, el sistema operativo experimentarán un rendimiento de escritura mejorado como los datos solo deben escribirse en la memoria caché. Una vez que los elementos multimedia subyacentes está saturado con E/S, rellenará la memoria caché y rendimiento de escritura volverá a la velocidad del disco.

- Cuando el almacenamiento en caché de E/S de lectura, el escenario donde la memoria caché es más conveniente es cuando los datos se almacenan secuencialmente en el disco y la memoria caché puede anticipadas (lo que hace la suposición de que el sector siguiente contiene los datos que se solicitará a continuación).
- Cuando se leen E/S aleatoria, almacenamiento en caché en el controlador de la unidad es probablemente no proporcionen ninguna mejora de la cantidad de datos que se pueden leer desde el disco. Cualquier mejora es que no existe si el sistema operativo o el tamaño de caché basada en la aplicación es mayor que el tamaño de caché basada en hardware.

  En el caso de Active Directory, la memoria caché solo está limitada por la cantidad de RAM.

### <a name="ssd-considerations"></a>Consideraciones de SSD

SSDs son un animal completamente diferente que los discos duros basados en el eje. Aunque se siguen los dos criterios clave: "¿Cuántas IOPS puede controlar?" y "¿Qué es la latencia de esas IOPS?" En comparación con discos duros basados en el eje, SSDs pueden controlar mayores volúmenes de E/S y pueden tener una menor latencia. En general y cuando se redactó este documento, aunque SSDs son aún más costosos en una comparación del costo por Gigabyte, son muy baratos en términos de costo por-I/Os y merece una consideración importante en términos de rendimiento de almacenamiento.

Consideraciones:

- E/s por segundo y las latencias son muy subjetivas a los diseños de fabricante y en algunos casos se han observado un rendimiento deficiente de las tecnologías en función del cabezal. En resumen, es más importante para revisar y validar las especificaciones del fabricante por unidad y no suponen los aspectos generales.
- Tipos de e/s por segundo pueden tener un número muy diferente dependiendo de si es de lectura o escritura. Servicios de AD DS, por lo general, fundamentalmente aporta para lectura, será menos afectados que otros escenarios de aplicación.
- "Escribir resistencia:" Este es el concepto que las celdas SSD finalmente se desgastan. Varios fabricantes tratan este desafío distintas maneras. Al menos para la unidad de base de datos, el perfil fundamentalmente de lectura de E/S permite downplaying la importancia de este problema, como los datos no son muy volátiles.

### <a name="summary"></a>Resumen

Una manera de pensar en almacenamiento es representar fontanería doméstico. Imagine las IOPS de los medios que los datos se almacenan en es la purga principal doméstica. Cuando esto está tapado (por ejemplo, las raíces de la canalización) o limitado (es demasiado pequeño o contraído), todos los receptores en el hogar copia de seguridad cuando se está demasiado agua utilizado (muchos invitados). Esto es análogo perfectamente en un entorno compartido, donde uno o más sistemas aprovechan el almacenamiento compartido en una SAN/NAS/iSCSI con el mismo medio subyacente. Diferentes enfoques que pueden realizar para solucionar los distintos escenarios:

- Una purga contraído o tamaño insuficiente requiere una sustitución completa de escala y la corrección. Esto sería similar a agregar en hardware nuevo o redistribución de los sistemas que utilizan el almacenamiento compartido en toda la infraestructura.
- Una canalización "obstruida" normalmente significa que la identificación de uno o más problemas infractor y eliminación de esos problemas. En un escenario de almacenamiento podría tratarse de almacenamiento o sistema nivel copias de seguridad, sincronizados exámenes antivirus en todos los servidores y ejecución de software de desfragmentación sincronizado durante períodos de máxima actividad.

En cualquier diseño de fontanería ínfima varios ingresar en el consumo principal. Si algo detiene uno de esos ínfima o un punto de unión, solo las cosas detrás de esa unión punto de copia de seguridad. En un escenario de almacenamiento, podría tratarse de un conmutador sobrecargado (escenario de SAN/NAS/iSCSI), problemas de compatibilidad de controlador (mal controlador/HBA combinación Firmware/storport.sys) o la copia de seguridad/antivirus/desfragmentación. Para determinar si el almacenamiento de "canalización" es lo suficientemente grande, debe medirse tamaño de E/S y e/s por segundo. En cada junta agregarlos juntos para garantizar el buen nivel "diámetro de canalización".

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Apéndice D - discusión sobre cómo solucionar problemas de almacenamiento - entornos donde proporcionar al menos tanta memoria RAM según el tamaño de la base de datos no es una opción viable

Es útil comprender por qué estas recomendaciones existen para que se pueden incluir los cambios en la tecnología de almacenamiento. Estas recomendaciones existen por dos motivos. El primero es el aislamiento de operaciones de E/S, tal que los problemas de rendimiento (paginación) en el eje del sistema operativo no afectar al rendimiento de la base de datos y perfiles de E/S. El segundo es que los archivos de registro para AD DS (y la mayoría de las bases de datos) son secuenciales por naturaleza y las memorias caché y unidades de disco duro basada en el eje tienen una ventaja enorme en el rendimiento cuando se usa con E/S secuenciales en comparación con los patrones más aleatorios de E/S del sistema operativo y casi unidad de base de datos puramente aleatorios patrones de E/S de AD DS. Al aislar la E/S secuencial a una unidad física independiente, se puede aumentar el rendimiento. El desafío que presenta las opciones de almacenamiento de hoy en día es que las suposiciones fundamentales detrás de estas recomendaciones ya no son true. En muchos virtualizar los escenarios de almacenamiento, como iSCSI, los archivos de imagen de disco Virtual, NAS y SAN, la media se comparte entre varios hosts de almacenamiento subyacente, completamente negando, por tanto, el "aislamiento de E/S" y los aspectos de "optimización de E/S secuencial". De hecho estos escenarios agregan una capa adicional de complejidad en que otros hosts obtener acceso a lo elementos multimedia compartidos pueden degradar la capacidad de respuesta para el controlador de dominio.

En la planificación de rendimiento del almacenamiento, hay tres categorías que tener en cuenta: estado de la caché en frío, estado de la caché calienten y copia de seguridad y restauración. El estado de la memoria caché en frío se produce en escenarios como cuando inicialmente se reinicia el controlador de dominio o se reinicia el servicio de Active Directory y no hay ningún dato de Active Directory en la memoria RAM. Estado de la caché activa es donde el controlador de dominio está en un estado estable y se almacena en caché de la base de datos. Estos son importantes a tener en cuenta que determinará los perfiles de rendimiento muy diferentes y tener suficiente RAM para almacenar en caché de la base de datos completa no mejorar el rendimiento cuando la memoria caché está fría. Se puede considerar el diseño de rendimiento para estos dos escenarios con la analogía siguiente, preparación de la memoria caché en frío es un "sprint" y la ejecución de un servidor con una memoria caché activa es "marathon".

Para la memoria caché en frío y escenario de caché activa, la pregunta es ¿con qué rapidez el almacenamiento puede mover los datos desde el disco en la memoria. Preparando la memoria caché es un escenario donde, con el tiempo, mejora el rendimiento mientras más consultas reutilización los datos, aumenta la tasa de aciertos de la caché y se reduce la frecuencia de tener que ir al disco. Como resultado se reduce el impacto de rendimiento adversas de va en el disco. Cualquier degradación del rendimiento sólo es transitorio mientras se espera para que la memoria caché preparar y crecer hasta el tamaño permitido máximo, dependiente del sistema. La conversación se puede simplificar a rapidez pueden ser recibidos los datos fuera de disco y es una medida simple de la IOPS disponibles para Active Directory, que es subjetiva IOPS disponibles desde el almacenamiento subyacente. Desde una perspectiva de diseño, porque Preparando los escenarios de caché y copia de seguridad y restauración se producen en casos excepcionales, normalmente se producen fuera del horario laboral y son subjetiva a la carga del controlador de dominio, las recomendaciones generales no existen, excepto en que se pueden programar estas actividades para las horas de poca actividad.

AD DS, en la mayoría de los escenarios, es fundamentalmente de lectura IO, normalmente una proporción de escritura read/10% 90%. E/S de lectura a menudo suele ser el cuello de botella para la experiencia del usuario, y con E/S de escritura, las causas escribir negativamente al rendimiento. Como el archivo NTDS.dit la E/S es principalmente aleatorio, las memorias caché tienden a proporcionar apenas beneficio para leer la E/S, lo que mucho más importante para configurar el almacenamiento para lee el perfil de E/S correctamente.

Para las condiciones de funcionamiento normales, el objetivo de diseño de almacenamiento es minimizar los tiempos de espera para una solicitud de AD DS que se devuelve desde el disco. Básicamente, esto significa que el número de pendientes y en espera de E/S es menor o equivalente al número de rutas en el disco. Hay una variedad de maneras de medir esto. En un escenario de supervisión de rendimiento, la recomendación general es ese disco lógico ( *\<unidad de base de datos NTDS\>* ) \Avg segundos de disco/lectura ser inferior a 20 ms. El umbral operativo deseado debe ser mucho menor, preferiblemente como cerca de la velocidad del almacenamiento de información como sea posible, en el intervalo de 2 a 6 milisegundos (segundo.002 a.006) según el tipo de almacenamiento.

Ejemplo:

![Gráfico de latencia de almacenamiento](media/capacity-planning-considerations-storage-latency.png)

Analizar el gráfico:

- **Elipse verde a la izquierda:** sigue siendo coherente a 10 ms la latencia. Aumenta la carga de 800 IOPS a 2400 e/s por segundo. Este es el límite inferior absoluto a la rapidez con una solicitud de E/S puede ser procesada por el almacenamiento subyacente. Esto está sujeto a los detalles de la solución de almacenamiento.
- **Burdeos elipse de la derecha:** el rendimiento sigue siendo plano desde la salida del círculo verde a través al final de la recopilación de datos mientras continúa aumentando la latencia. Esto demuestra que cuando los volúmenes de la solicitud superan las limitaciones físicas del almacenamiento subyacente, las solicitudes de más tiempo dedican sentado en la cola de espera de enviarse al subsistema de almacenamiento.

Aplicar este conocimiento:

- **Impacto en una suscripción de consultas de usuario de un grupo grande:** supone esto requiere la lectura de 1 MB de datos desde el disco, la cantidad de E/S y cuánto tiempo tarda pueda evaluarse como sigue:
  - Páginas de base de datos de Active Directory tienen un tamaño de 8 KB. 
  - Un mínimo de 128 páginas debe leerse desde el disco. 
  - Suponiendo que nada se almacena en caché, en el límite inferior (10 ms) Esto va a realizar un mínimo 1,28 segundos para cargar los datos desde el disco para devolverlo al cliente. A 20 ms, donde el rendimiento de almacenamiento que sobrepasa y también es el máximo recomendado, tardará 2.5 segundos para obtener los datos del disco para devolverlo al usuario final.  
- **¿A qué velocidad se calienta la memoria caché:** realizar la suposición de que la carga del cliente se va a maximizar el rendimiento en este ejemplo de almacenamiento, la memoria caché caliente a una tasa de IOPS 2400 &times; 8 KB por E/S. O, aproximadamente 20 MB por segunda, la carga de 1 GB de base de datos en RAM cada 53 segundos.

> [!NOTE]
> Es normal durante breves períodos observar las latencias trepar cuando los componentes de forma agresiva leer o escriben en el disco, por ejemplo, cuando el sistema de la copia de seguridad o cuando AD DS ejecuta la recolección de elementos. Debe proporcionarse la sala principal adicional sobre los cálculos para dar cabida a estos eventos periódicos. El objetivo que se va a proporcionar la capacidad suficiente para dar cabida a estos escenarios sin afectar a la función normal.

Como puede ver, hay un límite físico en función del diseño de almacenamiento para la rapidez con la memoria caché, posiblemente, puede preparar. ¿Qué preparar la memoria caché son entrante las solicitudes de cliente hasta la velocidad que puede proporcionar el almacenamiento subyacente. Ejecutar secuencias de comandos de "previamente preparación" la memoria caché durante las horas punta proporcionará compiten por cargar controlado por las solicitudes de cliente real. Entrega de los datos que los clientes necesitan en primer lugar porque, por diseño, se generará competición por los recursos de disco insuficiente como artificiales intentos de preparación de la memoria caché cargará los datos que no es relevantes para los clientes de ponerse en contacto con el controlador de dominio que puede afectar negativamente.
