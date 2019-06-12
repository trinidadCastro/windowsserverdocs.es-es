---
title: Hosts de sesión de escritorio remoto de optimización del rendimiento
description: Optimización del rendimiento directrices de Hosts de sesión de escritorio remoto
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e95671718616fc7c81977434e83a227c858fca17
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811421"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Hosts de sesión de escritorio remoto de optimización del rendimiento


Este tema explica cómo seleccionar hardware de Host de sesión de escritorio remoto (RD Session Host), ajustar el host y ajuste de aplicaciones.

**En este tema:**

-   [Seleccionar el hardware adecuado para el rendimiento](#selecting-the-proper-hardware-for-performance)

-   [Optimización de aplicaciones de Host de sesión de escritorio remoto](#tuning-applications-for-remote-desktop-session-host)

-   [Host de sesión de escritorio remoto para parámetros de ajuste](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Selección del hardware adecuado para el rendimiento


Para una implementación de servidor Host de sesión de escritorio remoto, la elección del hardware se rige por el conjunto de aplicaciones y cómo usar los usuarios. Los factores clave que afectan al número de usuarios y su experiencia son la CPU, memoria, disco y gráficos. Esta sección contiene directrices adicionales que son específicas de los servidores Host de sesión de escritorio remoto y está relacionada principalmente con el entorno de varios usuarios de los servidores Host de sesión de escritorio remoto.

### <a name="cpu-configuration"></a>Configuración de la CPU

Configuración de la CPU conceptualmente se determina multiplicando la CPU necesaria para admitir una sesión por el número de sesiones que se espera que el sistema para admitir manteniendo una zona de búfer para manejar los picos temporales. Varios procesadores lógicos pueden ayudar a reducir anómalas situaciones de congestión de la CPU, que suelen deberse a unos pocos subprocesos molestias contenidas en un número similar de procesadores lógicos.

Por lo tanto, los procesadores lógicos más en un sistema, cuanto menor sea el margen de protección que debe compilarse a la estimación de uso de CPU, lo que resulta en un porcentaje mayor de carga activa por CPU. Un factor importante a recordar es que lo que duplica el número de CPU no no duplica la capacidad de CPU.

### <a name="memory-configuration"></a>Configuración de memoria

Configuración de memoria depende de las aplicaciones que emplean los usuarios; Sin embargo, se puede calcular la cantidad necesaria de memoria mediante la fórmula siguiente: TotalMem = OSMem + SessionMem \* NS

OSMem es la cantidad de memoria que requiere el sistema operativo para ejecutar (por ejemplo, imágenes binarias del sistema, las estructuras de datos y así sucesivamente), SessionMem es cuánto requieren procesos de memoria que se ejecutan en una sesión y NS es el número de sesiones activas de destino. La cantidad de memoria necesaria para una sesión en su mayoría viene determinada por la referencia de memoria privada establecida para las aplicaciones y procesos del sistema que se ejecutan dentro de la sesión. Páginas de código o datos compartidas tienen poco efecto porque hay solo una copia en el sistema.

Una observación interesante (suponiendo que el sistema de disco que se copia el archivo de paginación no cambia) es que cuanto mayor sea el número de sesiones activas simultáneas del sistema tiene previsto admitir, cuanto mayor sea la asignación de memoria por sesión debe ser. Si no se aumenta la cantidad de memoria asignada por sesión, el número de errores de página que las sesiones activas genera aumenta con el número de sesiones. Finalmente, estos errores saturar el subsistema de E/S. Si aumenta la cantidad de memoria asignada por sesión, disminuye la probabilidad de incurrir en errores de página, que le ayuda a reducir la velocidad total de errores de página.

### <a name="disk-configuration"></a>Configuración del disco

Almacenamiento de información es uno de los aspectos más alto al configurar los servidores Host de sesión de escritorio remoto y puede ser la limitación más comunes en los sistemas que se implementan en el campo.

La actividad del disco que se genera en un servidor Host de sesión de escritorio remoto típico afecta a las áreas siguientes:

-   Archivos del sistema y archivos binarios de aplicación

-   Archivos de paginación

-   Perfiles de usuario y datos de usuario

Idealmente, estas áreas realizar copias de seguridad por los dispositivos de almacenamiento distinta. Uso de configuraciones RAID distribuidas u otros tipos de almacenamiento de alto rendimiento aún más mejora el rendimiento. Recomendamos encarecidamente que use adaptadores de almacenamiento con almacenamiento en caché de escritura de la batería. Los controladores de almacenamiento en caché de escritura de disco ofrecen compatibilidad mejorada para las operaciones de escritura sincrónica. Dado que todos los usuarios tienen un subárbol independiente, las operaciones de escritura sincrónica son significativamente más comunes en un servidor Host de sesión de escritorio remoto. Los subárboles del registro se guardan periódicamente en el disco mediante el uso de las operaciones de escritura sincrónica. Para habilitar estas optimizaciones, desde la consola de administración de discos, abra el **propiedades** cuadro de diálogo para el disco de destino y, en el **directivas** ficha, seleccione el **Habilitar caché de escritura en el disco** y **desactivar el vaciado de búfer de memoria caché de escritura de Windows** en las casillas de verificación del dispositivo.

### <a name="network-configuration"></a>Configuración de red

Uso de la red para un servidor Host de sesión de escritorio remoto incluye dos categorías principales:

-   Uso del tráfico de conexión de Host de sesión de escritorio remoto depende casi exclusivamente de los patrones de dibujos que se muestran las aplicaciones que se ejecutan dentro de las sesiones y el tráfico de E/S redirigida dispositivos.

    Por ejemplo, aplicaciones de procesamiento de texto y datos de entrada consumen ancho de banda de aproximadamente 10 a 100 kilobits por segundo, mientras que los gráficos enriquecidos y reproducción de vídeo provocar aumentos significativos en el uso de ancho de banda.

-   Conexiones de back-end, como perfiles móviles, la aplicación acceso a recursos compartidos de archivos, servidores de base de datos, servidores de correo electrónico y los servidores HTTP.

    El volumen y el perfil de tráfico de red es específico para cada implementación.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Optimización de aplicaciones de Host de sesión de escritorio remoto


La mayoría del uso de CPU en un servidor Host de sesión de escritorio remoto está controlado por las aplicaciones. Las aplicaciones de escritorio se optimizan normalmente hacia la capacidad de respuesta con el objetivo de minimizar el tiempo que tarda una aplicación para responder a una solicitud de usuario. Sin embargo, en un entorno de servidor, es igualmente importante minimizar la cantidad total de uso de CPU que se necesita para completar una acción para evitar afectar negativamente a otras sesiones.

Al configurar las aplicaciones que se utilizan en un servidor Host de sesión de escritorio remoto, tenga en cuenta las siguientes sugerencias:

-   Minimizar el procesamiento de bucle inactivo en segundo plano

    Ejemplos típicos son deshabilitar la comprobación de ortografía y gramática de segundo plano, datos de indexación de búsqueda, y lo guarda en segundo plano.

-   Minimizar la frecuencia con una aplicación realiza una comprobación de estado o la actualización.

    Deshabilitar este tipo de comportamientos o incrementando el intervalo entre iteraciones de sondeo y el temporizador de activación significativamente beneficia el uso de CPU porque el efecto de estas actividades se intensifica rápidamente para muchas de las sesiones activas. Ejemplos típicos son los iconos de estado de conexión y las actualizaciones de información de barra de estado.

-   Minimizar la contención de recursos entre aplicaciones reduciendo su frecuencia de sincronización.

    Ejemplos de estos recursos incluyen las claves del registro y archivos de configuración. Ejemplos de características y componentes de aplicación son el indicador de estado (por ejemplo, las notificaciones del shell), la indización en segundo plano o supervisión de los cambios y sincronización sin conexión.

-   Deshabilite los procesos innecesarios que están registrados para comenzar con un inicio de sesión o inicio de sesión de usuario.

    Estos procesos pueden contribuir significativamente el costo de uso de CPU al crear una nueva sesión de usuario, que generalmente es un proceso intensivo de CPU, y puede ser muy costosa en escenarios de mañana. Usar MsConfig.exe o MsInfo32.exe para obtener una lista de procesos que se inician en el inicio de sesión de usuario. Para obtener información más detallada, puede usar [Autoruns para Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Para el consumo de memoria, tenga en cuenta lo siguiente:

-   Compruebe que no se reubica los archivos DLL cargados por una aplicación.

    -   Cambia la ubicación de los archivos DLL se puede comprobar mediante la selección de vista de la DLL del proceso, tal como se muestra en la ilustración siguiente, mediante el uso de [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Aquí podemos ver que y.dll. se reubicarán debido a x.dll ya está ocupado su dirección base predeterminada y no se habilitó ASLR

        ![cambia la ubicación de los archivos DLL](../../media/perftune-guide-relocated-dlls.png)

        Si se reubica los archivos DLL, es imposible compartir su código entre las sesiones, lo que aumenta considerablemente la superficie de una sesión. Esto es uno de los problemas más comunes de rendimiento relacionados con la memoria en un servidor Host de sesión de escritorio remoto.

-   Para aplicaciones de common language runtime (CLR), use el generador de imágenes nativas (Ngen.exe) para aumentar el uso compartido de la página y reducir la sobrecarga de CPU.

    Cuando sea posible, aplicar técnicas similares para otros motores de ejecución similares.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Host de sesión de escritorio remoto para parámetros de ajuste


### <a name="page-file"></a>Archivo de paginación

El tamaño del archivo de página insuficiente puede provocar errores de asignación de memoria en las aplicaciones o componentes del sistema. Puede usar el contador de rendimiento de bytes de memoria asignados para controlar cuánta memoria virtual confirmada se encuentra en el sistema.

### <a name="antivirus"></a>Antivirus

Instalación de software antivirus en un servidor Host de sesión de escritorio remoto en gran medida afecta al rendimiento general del sistema, especialmente el uso de CPU. Se recomienda encarecidamente que excluir de la lista de supervisión activa todas las carpetas que contienen los archivos temporales, generan especialmente las que los servicios y otros componentes del sistema.

### <a name="task-scheduler"></a>Programador de tareas

Programador de tareas le permite examinar la lista de tareas que están programados para los distintos eventos. Para un servidor Host de sesión de escritorio remoto, es útil centrarse específicamente en las tareas que están configurados para ejecutarse en inactividad, en el inicio de sesión de usuario o en la sesión de conectar y desconectar. Debido a las características específicas de la implementación, muchas de estas tareas pueden ser innecesarios.

### <a name="desktop-notification-icons"></a>Iconos de notificación del escritorio

Iconos de notificación en el escritorio pueden tener mecanismos de actualización bastante costosos. Debe deshabilitar las notificaciones mediante la eliminación del componente que éstos se registra en la lista de inicio o cambiando la configuración en las aplicaciones y componentes del sistema para deshabilitarlos. Puede usar **personalizar las notificaciones de iconos** para examinar la lista de notificaciones que están disponibles en el servidor.

### <a name="remotefx-data-compression"></a>Compresión de datos de RemoteFX

Se puede configurar la compresión de Microsoft RemoteFX mediante el uso de directiva de grupo en **configuración del equipo &gt; plantillas administrativas &gt; componentes de Windows &gt; servicios de escritorio remoto &gt; remoto Host de sesión de escritorio &gt; entorno de sesión remoto &gt; configurar la compresión de datos de RemoteFX**. Tres valores posibles son:

-   **Optimizado para usar menos memoria** consume la menor cantidad de memoria por cada sesión pero tiene la razón de compresión más baja y, por tanto, el consumo de ancho de banda más alto.

-   **Equilibra la memoria y ancho de banda** reduce el consumo de ancho de banda al tiempo que aumenta ligeramente el consumo de memoria (aproximadamente 200 KB por sesión).

-   **Optimizado para utilizar menos ancho de banda de red** reduce aún más el uso de ancho de banda de red con un costo de aproximadamente 2 MB por sesión. Si desea usar esta configuración, debe evaluar el número máximo de sesiones y probar a ese nivel con esta configuración antes de colocar el servidor de producción.

También puede elegir no usar un algoritmo de compresión de RemoteFX. Si decide no usar un algoritmo de compresión RemoteFX usará más ancho de banda de red, y solo se recomienda si usa un dispositivo de hardware que está diseñado para optimizar el tráfico de red. Incluso si decide no usar un algoritmo de compresión de RemoteFX, algunos datos de gráficos se comprimirán.

### <a name="device-redirection"></a>Redirección de dispositivos

Se puede configurar la redirección de dispositivos mediante la directiva de grupo en **configuración del equipo &gt; plantillas administrativas &gt; componentes de Windows &gt; servicios de escritorio remoto &gt; escritorio remoto Host de sesión &gt; redireccionar los recursos y dispositivos** o mediante el **colección de sesiones** en Administrador del servidor de cuadro de propiedades.

Por lo general, redirección de dispositivos aumenta cuánto se usan conexiones de servidor de Host de sesión de escritorio remoto de ancho de banda de red porque los datos se intercambian entre los dispositivos en los equipos cliente y los procesos que se ejecutan en la sesión del servidor. La extensión del aumento es una función de la frecuencia de las operaciones realizadas por las aplicaciones que se ejecutan en el servidor frente a los dispositivos redirigidos.

Redirección de impresoras y Plug and Play redirección de dispositivos también aumenta el uso de CPU en el inicio de sesión. Puede redirigir impresoras de dos maneras:

-   Búsqueda de coincidencias basado en controlador redirección de impresoras cuando un controlador de la impresora debe instalarse en el servidor. Las versiones anteriores de Windows Server usan este método.

-   Se introdujo en Windows Server 2008, redirección de controlador de impresora Easy Print usa un controlador de impresora común para todas las impresoras.

Se recomienda el método Easy Print porque hace menos uso de CPU para la instalación de impresora en tiempo de conexión. El método de controlador coincidente provoca un mayor uso de CPU porque requiere el servicio de cola para cargar controladores diferentes. Para el uso de ancho de banda, Easy Print hace uso de ancho de banda de red ligeramente mayor, pero no lo suficientemente importante como para compensar las otras ventajas de rendimiento, facilidad de uso y la confiabilidad.

Redirección de audio hace que un flujo constante de tráfico de red. Redirección de audio también permite a los usuarios ejecutar aplicaciones multimedios que normalmente tienen elevado consumo de CPU.

### <a name="client-experience-settings"></a>Configuración de la experiencia de cliente

De forma predeterminada, conexión a Escritorio remoto (RDC) elige automáticamente en función de la idoneidad de la conexión de red entre los equipos cliente y servidor de configuración de experiencia de la derecha. Se recomienda que la configuración de RDC permanecen en **detectar automáticamente la calidad de conexión**.

Para los usuarios avanzados, RDC proporciona control sobre una variedad de opciones que influyen en el rendimiento del ancho de banda de red para la conexión de servicios de escritorio remoto. Puede acceder a la siguiente configuración mediante el **experiencia** pestaña conexión a Escritorio remoto o como una configuración en el archivo RDP.

Al conectarse a cualquier equipo, se aplican las siguientes opciones:

-   **Deshabilitar el papel tapiz** (Disable wallpaper: i: 0) no muestra el papel tapiz del escritorio en conexiones redirigidas. Esta configuración puede reducir significativamente el uso de ancho de banda si el papel tapiz del escritorio consta de una imagen u otro contenido con costos importantes para el dibujo.

-   **Caché de mapa de bits** (Bitmapcachepersistenable:i:1) cuando se habilita esta configuración, crea una caché de cliente de mapas de bits que se representan en la sesión. Proporciona una mejora considerable sobre el uso de ancho de banda y siempre debe habilitarse (a menos que haya otras consideraciones de seguridad).

-   **Mostrar el contenido de windows mientras se arrastra** (deshabilitar arrastrar ventana completa: i: 1) cuando esta opción está deshabilitada, reduce el ancho de banda al mostrar el marco de ventana en lugar de todo el contenido cuando se arrastra la ventana.

-   **Animación de menús y ventanas** (Deshabilitar menú anims:i:1 y configuración i cursor de deshabilitación:: 1): Cuando estas opciones están deshabilitadas, reduce el ancho de banda deshabilitando la animación de menús (por ejemplo, la atenuación) y los cursores.

-   **Suavizado de fuentes** (permitir suavizado de fuentes: i: 0) soporte técnico de procesamiento de fuentes ClearType de controles. Al conectarse a equipos que ejecutan Windows 8 o Windows Server 2012 y versiones posteriores, habilitar o deshabilitar a esta configuración no tiene un impacto significativo en el uso de ancho de banda. Sin embargo, para los equipos que ejecutan versiones anteriores a Windows 7 y Windows 2008 R2, si habilita a esta configuración afecta al consumo de ancho de banda de red significativamente.

Las siguientes opciones solo se aplican al conectarse a equipos que ejecutan Windows 7 y versiones anteriores del sistema operativo:

-   **Composición del escritorio** esta configuración solo se admite para una sesión remota en un equipo que ejecuta Windows 7 o Windows Server 2008 R2.

-   **Los estilos visuales** (deshabilitar temas: i: 1) cuando esta opción está deshabilitada, reduce el ancho de banda, ya que simplifica los dibujos de tema que usan el tema clásico.

Mediante el uso de la **experiencia** pestaña dentro de la conexión a Escritorio remoto, puede elegir la velocidad de conexión para influir en el rendimiento del ancho de banda de red. A continuación enumeran las opciones que están disponibles para configurar la velocidad de conexión:

-   **Detectar automáticamente la calidad de conexión** (tipo de conexión: i:7) cuando se habilita esta configuración, conexión a Escritorio remoto elige automáticamente la configuración resultante en la experiencia de usuario óptima según la calidad de la conexión. (Se recomienda esta configuración cuando se conecta a los equipos que ejecutan Windows 8 o Windows Server 2012 y versiones posteriores).

-   **Módem (56 Kbps)** (conexión tipo: i: 1) esta configuración habilita el almacenamiento en caché de mapa de bits persistente.

-   **Baja velocidad de banda ancha (256 Kbps - 2 Mbps)** (tipo de conexión: i:2) esta configuración habilita los estilos de almacenamiento en caché y visual de mapa de bits persistente.

-   **(2 Mbps - 16 Mbps con una latencia alta) de red de telefonía móvil o satélite** (tipo de conexión: i:3) esta configuración permite la composición del escritorio, el almacenamiento en caché de mapa de bits persistente, los estilos visuales y fondo de escritorio.

-   **Banda ancha de alta velocidad (2 Mbps – 10 Mbps)** (tipo de conexión: i:4) esta configuración permite la composición del escritorio, mostrar el contenido de windows mientras arrastra, animación de menús y ventanas, el almacenamiento en caché de mapa de bits persistente, los estilos visuales y fondo de escritorio.

-   **WAN (10 Mbps o superior con una latencia elevada)** (tipo de conexión: i:5) esta configuración permite la composición del escritorio, mostrar el contenido de windows mientras arrastra, animación de menús y ventanas, el almacenamiento en caché de mapa de bits persistente, los estilos visuales y fondo de escritorio.

-   **LAN (10 Mbps o superior)** (tipo de conexión: i:6) esta configuración permite la composición del escritorio, mostrar el contenido de windows mientras arrastra, animación de menús y ventanas, el almacenamiento en caché de mapa de bits persistente, temas y fondo de escritorio.

### <a name="desktop-size"></a>Tamaño del escritorio

Tamaño del escritorio para sesiones remotas puede controlarse mediante el uso de la ficha para mostrar en conexión a Escritorio remoto o mediante el archivo de configuración de RDP (desktopwidth:i:1152 y desktopheight:i:864). Cuanto mayor sea el tamaño del escritorio, mayor será el consumo de memoria y ancho de banda que está asociado a esa sesión. El tamaño máximo del escritorio actual es 4096 x 2048.
