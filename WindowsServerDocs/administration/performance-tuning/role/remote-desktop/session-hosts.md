---
title: Optimización del rendimiento Escritorio remoto hosts de sesión
description: Directrices para la optimización del rendimiento para hosts de sesión de Escritorio remoto
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 9a82f0db1f586e1f762292c61eabdd7f6556c8bc
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992108"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Optimización del rendimiento Escritorio remoto hosts de sesión


En este tema se describe cómo seleccionar Escritorio remoto host de sesión (host de sesión de escritorio remoto), ajustar el host y optimizar las aplicaciones.

**En este tema:**

-   [Selección del hardware adecuado para el rendimiento](#selecting-the-proper-hardware-for-performance)

-   [Optimización de aplicaciones para Escritorio remoto host de sesión](#tuning-applications-for-remote-desktop-session-host)

-   [Parámetros de ajuste del host de sesión Escritorio remoto](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Selección del hardware adecuado para el rendimiento


En una implementación de servidor host de sesión de escritorio remoto, la elección del hardware se rige por el conjunto de aplicaciones y el modo en que los usuarios los usan. Los factores clave que afectan al número de usuarios y su experiencia son la CPU, la memoria, el disco y los gráficos. Esta sección contiene instrucciones adicionales que son específicas de los servidores host de sesión de escritorio remoto y están relacionadas principalmente con el entorno multiusuario de los servidores host de sesión de escritorio remoto.

### <a name="cpu-configuration"></a>Configuración de la CPU

La configuración de la CPU se determina conceptualmente multiplicando la CPU necesaria para admitir una sesión por el número de sesiones que se espera que admita el sistema, al tiempo que se mantiene una zona del búfer para controlar los picos temporales. Varios procesadores lógicos pueden ayudar a reducir situaciones anómalas de congestión de la CPU, que suelen deberse a algunos subprocesos sobreactivos contenidos en un número similar de procesadores lógicos.

Por lo tanto, cuanto mayor sea el número de procesadores lógicos de un sistema, menor será el margen de cojín que se debe integrar en el cálculo de uso de CPU, lo que da como resultado un mayor porcentaje de carga activa por CPU. Un factor importante que recordar es que la duplicación del número de CPU no redoble la capacidad de la CPU.

### <a name="memory-configuration"></a>Configuración de la memoria

La configuración de la memoria depende de las aplicaciones que emplean los usuarios; sin embargo, se puede calcular la cantidad de memoria necesaria mediante la siguiente fórmula: TotalMem = OSMem + SessionMem \* NS

OSMem es la cantidad de memoria que requiere la ejecución del sistema operativo (como imágenes binarias del sistema, estructuras de datos, etc.), SessionMem es la cantidad de procesos de memoria que se ejecutan en una sesión y NS es el número objetivo de sesiones activas. La cantidad de memoria necesaria para una sesión está determinada principalmente por el conjunto de referencia de memoria privada para las aplicaciones y los procesos del sistema que se ejecutan dentro de la sesión. El código compartido o las páginas de datos tienen poco efecto porque solo hay una copia en el sistema.

Una observación interesante (suponiendo que el sistema de disco que realiza la copia de seguridad del archivo de paginación no cambie) es que el mayor número de sesiones activas simultáneas que el sistema tiene previsto admitir, cuanto mayor sea la asignación de memoria por sesión. Si no aumenta la cantidad de memoria asignada por sesión, el número de errores de página que generan las sesiones activas aumenta con el número de sesiones. Finalmente, estos errores sobrecargan el subsistema de e/s. Al aumentar la cantidad de memoria asignada por sesión, disminuye la probabilidad de que se produzcan errores de página incurridos, lo que ayuda a reducir la tasa general de errores de página.

### <a name="disk-configuration"></a>Configuración de discos

El almacenamiento es uno de los aspectos más desbuscados al configurar los servidores host de sesión de escritorio remoto y puede ser la limitación más común en los sistemas que se implementan en el campo.

La actividad de disco que se genera en un servidor host de sesión de escritorio remoto típico afecta a las siguientes áreas:

-   Archivos del sistema y binarios de la aplicación

-   Archivos de paginación

-   Perfiles de usuario y datos de usuario

Idealmente, se debe realizar una copia de seguridad de estas áreas mediante dispositivos de almacenamiento distintos. El uso de configuraciones RAID seccionadas u otros tipos de almacenamiento de alto rendimiento mejora el rendimiento. Le recomendamos encarecidamente que use adaptadores de almacenamiento con almacenamiento en caché de escritura con respaldo de batería. Los controladores con el almacenamiento en caché de escritura en disco ofrecen mayor compatibilidad con las operaciones de escritura sincrónicas. Dado que todos los usuarios tienen un subárbol independiente, las operaciones de escritura sincrónicas son mucho más comunes en un servidor host de sesión de escritorio remoto. Los subárboles del registro se guardan periódicamente en el disco mediante operaciones de escritura sincrónicas. Para habilitar estas optimizaciones, en la consola de administración de discos, abra el cuadro de diálogo **propiedades** del disco de destino y, en la ficha **directivas** , active las casillas **Habilitar almacenamiento en caché de escritura en el disco** y **desactivar el vaciado del búfer de caché de escritura de Windows** en el dispositivo.

### <a name="network-configuration"></a>Network configuration (Configuración de red)

El uso de red para un servidor host de sesión de escritorio remoto incluye dos categorías principales:

-   El uso del tráfico de conexión del host de sesión de escritorio remoto lo determinan casi exclusivamente los patrones de dibujo que se muestran en las aplicaciones que se ejecutan dentro de las sesiones y el tráfico de e/s de los dispositivos redirigidos.

    Por ejemplo, las aplicaciones que controlan el procesamiento de texto y la entrada de datos consumen ancho de banda de aproximadamente 10 a 100 kilobits por segundo, mientras que los gráficos y la reproducción de vídeo enriquecidos producen aumentos significativos en el uso de ancho

-   Conexiones de back-end, como perfiles móviles, acceso de la aplicación a recursos compartidos de archivos, servidores de bases de datos, servidores de correo electrónico y servidores HTTP.

    El volumen y el perfil del tráfico de red son específicos de cada implementación.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Optimización de aplicaciones para Escritorio remoto host de sesión


La mayor parte del uso de CPU en un servidor host de sesión de escritorio remoto está controlada por aplicaciones. Las aplicaciones de escritorio normalmente están optimizadas para la capacidad de respuesta con el objetivo de minimizar el tiempo que tarda una aplicación en responder a una solicitud de usuario. Sin embargo, en un entorno de servidor, es igualmente importante minimizar la cantidad total de uso de CPU que se necesita para completar una acción con el fin de evitar que afecte negativamente a otras sesiones.

Tenga en cuenta las siguientes sugerencias al configurar las aplicaciones que se van a usar en un servidor host de sesión de escritorio remoto:

-   Minimizar el procesamiento de bucles inactivos en segundo plano

    Los ejemplos típicos son deshabilitar la gramática en segundo plano y la revisión ortográfica, la indexación de datos para la búsqueda y el guardado en segundo plano.

-   Minimizar la frecuencia con la que una aplicación realiza una comprobación de estado o una actualización.

    La deshabilitación de estos comportamientos o el aumento del intervalo entre las iteraciones de sondeo y la activación del temporizador aumenta significativamente el uso de CPU, ya que el efecto de estas actividades se amplifica rápidamente para muchas sesiones activas. Algunos ejemplos típicos son los iconos de estado de conexión y las actualizaciones de información de la barra de estado.

-   Minimice la contención de recursos entre aplicaciones reduciendo su frecuencia de sincronización.

    Algunos ejemplos de estos recursos son las claves del registro y los archivos de configuración. Algunos ejemplos de componentes y características de la aplicación son el indicador de estado (por ejemplo, las notificaciones de Shell), la indexación en segundo plano o la supervisión de cambios y la sincronización sin conexión.

-   Deshabilite los procesos innecesarios que están registrados para iniciarse con el inicio de sesión de usuario o un inicio de sesión.

    Estos procesos pueden contribuir significativamente al costo del uso de CPU al crear una nueva sesión de usuario, que generalmente es un proceso intensivo de la CPU, y puede resultar muy caro en escenarios de la mañana. Use MsConfig.exe o MsInfo32.exe para obtener una lista de los procesos que se inician en el inicio de sesión de usuario. Para obtener información más detallada, puede usar [Autoruns para Windows](/sysinternals/downloads/autoruns).

En el consumo de memoria, debe tener en cuenta lo siguiente:

-   Compruebe que los archivos dll cargados por una aplicación no se reubican.

    -   Los archivos dll reubicados se pueden comprobar seleccionando procesar DLL vista, como se muestra en la siguiente ilustración, mediante el [Explorador de procesos](/sysinternals/downloads/process-explorer).

    -   Aquí podemos ver que y.dll se reubicaba porque ya x.dll ocupado su dirección base predeterminada y ASLR no estaba habilitado

        ![DLL reubicadas](../../media/perftune-guide-relocated-dlls.png)

        Si se reubican los archivos dll, es imposible compartir el código entre las sesiones, lo que aumenta significativamente la superficie de una sesión. Se trata de uno de los problemas de rendimiento más comunes relacionados con la memoria en un servidor host de sesión de escritorio remoto.

-   En el caso de las aplicaciones Common Language Runtime (CLR), use el generador de imágenes nativas (Ngen.exe) para aumentar el uso compartido de páginas y reducir la sobrecarga de la CPU.

    Cuando sea posible, aplique técnicas similares a otros motores de ejecución similares.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Parámetros de ajuste del host de sesión Escritorio remoto


### <a name="page-file"></a>Archivo de paginación

Un tamaño de archivo de página insuficiente puede producir errores de asignación de memoria en aplicaciones o componentes del sistema. Puede usar el contador de rendimiento de bytes de memoria confirmada para supervisar la cantidad de memoria virtual confirmada que se encuentra en el sistema.

### <a name="antivirus"></a>Antivirus

La instalación del software antivirus en un servidor host de sesión de escritorio remoto afecta en gran medida al rendimiento general del sistema, especialmente al uso de la CPU. Se recomienda excluir de la lista de supervisión activa todas las carpetas que contienen archivos temporales, especialmente las que generan los servicios y otros componentes del sistema.

### <a name="task-scheduler"></a>Programador de tareas

Programador de tareas permite examinar la lista de tareas programadas para distintos eventos. En el caso de un servidor host de sesión de escritorio remoto, resulta útil centrarse específicamente en las tareas configuradas para ejecutarse en estado de inactividad, en el inicio de sesión de usuario o en la conexión y desconexión de la sesión. Debido a los detalles de la implementación, muchas de estas tareas podrían no ser necesarias.

### <a name="desktop-notification-icons"></a>Iconos de notificación de escritorio

Los iconos de notificación en el escritorio pueden tener mecanismos de actualización bastante caros. Debe deshabilitar las notificaciones quitando el componente que las registra de la lista de inicio o cambiando la configuración en aplicaciones y componentes del sistema para deshabilitarlos. Puede usar los **iconos de personalización de notificaciones** para examinar la lista de notificaciones que están disponibles en el servidor.

### <a name="remote-desktop-protocol-data-compression"></a>Protocolo de escritorio remoto compresión de datos

Protocolo de escritorio remoto compresión se puede configurar mediante Directiva de grupo en **configuración del equipo** &gt; **plantillas administrativas** &gt; **componentes de Windows** &gt; **servicios de escritorio remoto** &gt; **escritorio remoto** &gt; **entorno de sesión remota** &gt; **del host de sesión configurar la compresión para los datos de RemoteFX**. Hay tres valores posibles:

-   **Optimizado para usar menos memoria** Consume la menor cantidad de memoria por sesión, pero tiene la menor relación de compresión y, por lo tanto, el mayor consumo de ancho de banda.

-   **Equilibra la memoria y el ancho de banda de red** Reducción del consumo de ancho de banda al tiempo que se aumenta el consumo de memoria (aproximadamente 200 KB por sesión).

-   **Optimizado para usar menos ancho de banda de red** Reduce aún más el uso de ancho de banda de red a un costo de aproximadamente 2 MB por sesión. Si desea usar esta configuración, debe evaluar el número máximo de sesiones y realizar pruebas en ese nivel con esta configuración antes de colocar el servidor en producción.

También puede optar por no usar un algoritmo de compresión de Protocolo de escritorio remoto, por lo que solo se recomienda usarlo con un dispositivo de hardware diseñado para optimizar el tráfico de red. Aunque decida no usar un algoritmo de compresión, se comprimirán algunos datos de gráficos.

### <a name="device-redirection"></a>Redireccionamiento de dispositivos

La redirección de dispositivos se puede configurar mediante Directiva de grupo en **configuración del equipo** &gt; **plantillas administrativas** &gt; **componentes de Windows** &gt; **servicios de escritorio remoto** &gt; **escritorio remoto** &gt; **dispositivo host de sesión y redirección de recursos** , o mediante el cuadro de propiedades colección de **sesiones** en Administrador del servidor.

Por lo general, la redirección de dispositivos aumenta la cantidad de ancho de banda de red que usan las conexiones del servidor host de sesión de escritorio remoto porque los datos se intercambian entre los dispositivos de los equipos cliente y los procesos que se ejecutan en la sesión de servidor. La extensión del aumento es una función de la frecuencia de las operaciones realizadas por las aplicaciones que se ejecutan en el servidor en los dispositivos redirigidos.

La redirección de impresoras y el redireccionamiento de dispositivos Plug and Play también aumenta el uso de CPU en el inicio de sesión. Puede redirigir impresoras de dos maneras:

-   Coincidencia de la redirección basada en el controlador de impresora cuando se debe instalar un controlador para la impresora en el servidor. En las versiones anteriores de Windows Server se usaba este método.

-   En Windows Server 2008, la redirección de un controlador de impresora fácil de impresión utiliza un controlador de impresora común para todas las impresoras.

Se recomienda el método Easy Print, ya que provoca menos uso de la CPU para la instalación de la impresora en el momento de la conexión. El método de controlador coincidente provoca un aumento del uso de la CPU porque requiere que el servicio Administrador de trabajos de impresión cargue controladores diferentes. Para el uso de ancho de banda, la impresión sencilla provoca un aumento del ancho de banda de red, pero no es lo suficientemente significativa como para desplazar las otras ventajas de rendimiento, capacidad de administración y confiabilidad.

La redirección de audio provoca una transmisión estable de tráfico de red. La redirección de audio también permite a los usuarios ejecutar aplicaciones multimedia que normalmente tienen un alto consumo de CPU.

### <a name="client-experience-settings"></a>Configuración de la experiencia del cliente

De forma predeterminada, Conexión a Escritorio remoto (RDC) elige automáticamente la configuración de la experiencia adecuada en función de la idoneidad de la conexión de red entre el servidor y los equipos cliente. Se recomienda que la configuración de RDC permanezca en **detectar automáticamente la calidad**de la conexión.

Para los usuarios avanzados, RDC proporciona control sobre una variedad de opciones de configuración que influyen en el rendimiento del ancho de banda de red para la conexión de Servicios de Escritorio remoto. Puede tener acceso a las siguientes opciones de configuración mediante la pestaña **experiencia** en conexión a escritorio remoto o como configuración en el archivo RDP.

La siguiente configuración se aplica al conectarse a cualquier equipo:

-   **Deshabilitar el papel tapiz** (deshabilitar el papel tapiz: i: 0) no muestra el papel tapiz del escritorio en las conexiones redirigidas. Esta configuración puede reducir significativamente el uso de ancho de banda si el papel tapiz del escritorio se compone de una imagen u otro contenido con costos significativos de dibujo.

-   **Caché de mapas de bits** (Bitmapcachepersistenable: i: 1) cuando se habilita esta configuración, crea una memoria caché del lado cliente de los mapas de bits que se representan en la sesión. Proporciona una mejora significativa en el uso de ancho de banda y siempre debe estar habilitada (a menos que haya otras consideraciones de seguridad).

-   **Mostrar contenido de Windows mientras se arrastra** (deshabilitar arrastre de ventana completa: i: 1) cuando esta opción está deshabilitada, reduce el ancho de banda mostrando solo el marco de la ventana en lugar de todo el contenido cuando se arrastra la ventana.

-   **Animación de menús y ventanas** (deshabilitar el menú anims: i: 1 y deshabilitar la configuración del cursor: i: 1): cuando esta configuración está deshabilitada, reduce el ancho de banda deshabilitando la animación en los menús (como la atenuación) y los cursores.

-   **Suavizado de fuentes** (Permitir suavizado de fuentes: i: 0) controla la compatibilidad con la representación de fuentes ClearType. Al conectarse a equipos que ejecutan Windows 8 o Windows Server 2012 y versiones posteriores, la habilitación o deshabilitación de esta configuración no tiene un impacto significativo en el uso del ancho de banda. Sin embargo, para los equipos que ejecutan versiones anteriores a Windows 7 y Windows 2008 R2, la habilitación de esta configuración afecta significativamente al consumo de ancho de banda de red.

La configuración siguiente solo se aplica cuando se conecta a equipos que ejecutan Windows 7 y versiones anteriores del sistema operativo:

-   **Composición de escritorio** Esta configuración solo se admite para una sesión remota en un equipo que ejecute Windows 7 o Windows Server 2008 R2.

-   **Estilos visuales** (deshabilitar temas: i: 1) cuando esta opción está deshabilitada, reduce el ancho de banda simplificando los dibujos del tema que usan el tema clásico.

Mediante el uso de la pestaña **experiencia** en conexión a escritorio remoto, puede elegir la velocidad de conexión para influir en el rendimiento del ancho de banda de red. A continuación se enumeran las opciones disponibles para configurar la velocidad de conexión:

-   **Detectar la calidad de la conexión automáticamente** (tipo de conexión: i: 7) cuando esta opción está habilitada, conexión a escritorio remoto elige automáticamente la configuración que dará lugar a una experiencia de usuario óptima en función de la calidad de la conexión. (Se recomienda esta configuración al conectarse a equipos que ejecutan Windows 8 o Windows Server 2012 y versiones posteriores).

-   **Módem (56 kbps)** (tipo de conexión: i: 1) esta configuración habilita el almacenamiento en caché de mapas de bits persistente.

-   **Banda ancha de baja velocidad (256 kbps-2 Mbps)** (tipo de conexión: i: 2) esta opción habilita el almacenamiento en caché de mapas de bits persistente y los estilos visuales.

-   **Móvil/satélite (2 Mbps-16 Mbps con latencia alta)** (tipo de conexión: i: 3) esta opción habilita la composición del escritorio, el almacenamiento en caché de mapas de bits persistente, los estilos visuales y el fondo de escritorio.

-   **Banda ancha de alta velocidad (2 Mbps – 10 Mbps)** (tipo de conexión: i: 4) esta opción permite la composición del escritorio, mostrar el contenido de las ventanas mientras se arrastran, la animación de menús y ventanas, el almacenamiento en caché de mapas de bits persistente, los estilos visuales y el fondo de escritorio.

-   **WAN (10 Mbps o superior con latencia alta)** (tipo de conexión: i: 5) esta opción permite la composición del escritorio, mostrar el contenido de las ventanas mientras se arrastran, la animación de menús y ventanas, el almacenamiento en caché de mapas de bits persistente, los estilos visuales y el fondo de escritorio.

-   **LAN (10 Mbps o superior)** (tipo de conexión: i: 6) esta configuración permite la composición del escritorio, mostrar el contenido de las ventanas mientras se arrastran, la animación de menús y ventanas, el almacenamiento en caché de mapas de bits persistente, los temas y el fondo de escritorio.

### <a name="desktop-size"></a>Tamaño del escritorio

El tamaño del escritorio de las sesiones remotas se puede controlar mediante la pestaña Mostrar de Conexión a Escritorio remoto o mediante el archivo de configuración de RDP (desktopwidth: i: 1152 y desktopheight: i: 864). Cuanto mayor sea el tamaño del escritorio, mayor será el consumo de ancho de banda y memoria asociado a esa sesión. El tamaño máximo actual del escritorio es 4096 x 2048.