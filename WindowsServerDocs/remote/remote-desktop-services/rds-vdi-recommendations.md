---
title: Configuración recomendada para equipos de escritorio VDI
description: Recomienda la configuración y la configuración para minimizar la sobrecarga para equipos de escritorio de Windows 10 1607 (10.0.1393) utilizado como imágenes VDI
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 24704373dedf6a44809b83f3df17bd073cee2bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818386"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Configuración recomendada para equipos de escritorio VDI

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows10

Microsoft Desktop Virtualization detecta automáticamente las configuraciones de dispositivo y las condiciones de red para recuperar los usuarios ponerse en marcha más rápido al habilitar la configuración de instantánea de las aplicaciones corporativas y escritorios, y equipa a TI para proporcionar acceso a heredado aplicaciones durante la migración a Windows 10.

Aunque el sistema operativo Windows 10 se ajusta muy bien desde el principio, hay oportunidades para mejorarla aún más específicamente para el entorno corporativo de Microsoft Virtual Desktop Infrastructure (VDI). En el entorno de VDI, muchos servicios en segundo plano y las tareas están deshabilitadas desde el principio.

En este tema no es un plano, pero en su lugar una guía o punto de partida. Algunas recomendaciones podrían deshabilitar la funcionalidad que prefiere usar, por lo que debe considerar el costo frente a la ventaja de ajuste cualquier configuración específica en su escenario.

Estas instrucciones y la configuración recomendada es relevante para Windows 10 1607 (versión 10.0.1393).

> [!NOTE]  
> Cualquier configuración que no se especifican explícitamente en este tema puede dejarse en sus valores predeterminados (o conjunto por sus requisitos y directivas) sin impacto apreciable en la funcionalidad VDI.

Cuando se crea una imagen que se basará la implementación de VDI, asegúrese de usar el **rama actual**. Para obtener más información acerca de la rama actual, vea [información de versión de Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Creación de la imagen de Windows 10
El primer paso es instalar una imagen de referencia de Windows 10 1607 (versión 10.0.1393) en la máquina virtual o física. Instalar en una máquina virtual es fácil y le permite guardar las versiones del archivo (VHD) de disco duro virtual, en caso de que desea revertir a una versión anterior.

Durante la instalación, puede elegir **configuración rápida** o **personalizar**. La configuración se ofrecieron durante la **personalizar** opción son ajustables mediante la directiva de grupo, por lo que no es importante que el método de instalación del sistema operativo base.


Si eligió **personalizar**, puede ajustar estos valores durante la instalación:

## <a name="in-customize-settings"></a>En "Personalizar la configuración"

También puede ajustar estos después de la instalación con el Editor de directivas de grupo; Consulte la sección "Configuración de directiva de grupo" de este tema.

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|**Personalización**| | |
|Personalice su voz, escribiendo y entrada de tinta mediante el envío de los datos de entrada a Microsoft.|    Activado| Desactivado|
|Enviar escribiendo y datos a Microsoft para mejorar la plataforma de reconocimiento y la sugerencia de tinta.|  Activado| Desactivado|
|Permiten que las aplicaciones utilizar su Id. de publicidad de experiencia en las aplicaciones.|  Activado| Desactivado|
|Deje que ayudan a conectar con amigos en su libreta de direcciones y compruebe su número de teléfono móvil de Skype (si está instalado). Pueden aplicarse cargos por SMS y datos.|    Activado| Desactivado|
|**Ubicación**| | |
|Activar mi dispositivo y permitir que Windows y aplicaciones solicitan su ubicación, incluido el historial de ubicación de búsqueda| Activado| Desactivado|
|Informe de errores de conectividad| | |
|Conectarse automáticamente a zonas Wi-Fi sugeridas con cobertura abierta No todas las redes son seguras.|    Activado| Desactivado|
|Conectar automáticamente para abrir las zonas activas temporalmente para ver si el pago de servicios de red disponibles.| Activado| Desactivado|
|Enviar datos de uso y diagnóstico completo a Microsoft. Si desactiva esta envía solo los datos básicos.| Activado| Desactivado|
|**Explorador, la protección y actualización**| | |
|SmartScreen de uso en línea de servicios para ayudar a proteger contra contenido malintencionado y descarga en sitios cargados por los exploradores de Windows y aplicaciones de Store|    Activado| En (si no hay ningún acceso a Internet, a continuación, establezca en Off.)
|Usar predicción de página para mejorar la lectura, acelerar la exploración y mejorar su experiencia general en los exploradores de Windows. Los datos de exploración se enviará a Microsoft.| Activado| Desactivado|
|Obtener actualizaciones desde y envíe las actualizaciones en otros equipos en Internet para acelerar la aplicación y descargar actualizaciones de Windows|   Activado| Desactivado|

Una vez completada la instalación, puede continuar ajustando la configuración a partir de **Windows configuración**.

## <a name="in-windows-settings"></a>En la configuración de Windows
Para obtener acceso a la configuración de Windows, haga clic en **iniciar** (el icono de Windows en la barra de tareas) y, a continuación, haga clic en el **configuración** icono (forma de un icono de engranaje).

### <a name="in-the-system-area-of-windows-settings"></a>En el área de "Sistema" de configuración de Windows
En el área de configuración de Windows, haga clic en el **sistema** icono proporciona acceso a una serie de valores relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso VDI, estas configuraciones son las más importantes:

#### <a name="apps-and-features"></a>Aplicaciones y características

Para quitar una aplicación, sin incluir, por tanto, desde su imagen VDI, haga clic en la aplicación y, a continuación, haga clic en **desinstalar**. Si **desinstalar** está deshabilitado, no puede quitarla mediante este método; es posible que pueda quitarla con Windows PowerShell o pruebe estos pasos:
1. Haga clic en **administrar características opcionales** (inmediatamente debajo del **aplicaciones y características** encabezado en la misma página).
2. Haga clic en la característica opcional y, a continuación, haga clic en **desinstalar**.

Las características que considere la posibilidad de quitar (si existe) incluyen lo siguiente:
- **Póngase en contacto con soporte técnico**
- **Contenido de demostración de venta directa de inglés (Estados Unidos)**
- **Contenido de demostración Retail neutro**
- **Ayuda rápida**

#### <a name="default-apps"></a>Aplicaciones predeterminadas

Este área define la aplicación que se usará de forma predeterminada para determinadas funciones genéricas como correo electrónico, exploración web y mapas. Si desea que otra aplicación que se usará para una función determinada, haga clic en la entrada actual y, a continuación, haga clic en la aplicación que desea que se utilice en la imagen VDI. Para que una aplicación que no sean de Microsoft ser una opción disponible, debe instalar la aplicación antes de ajustar esta configuración.

#### <a name="notifications-and-actions"></a>Notificaciones y acciones

Estos valores recomendados reducirá las notificaciones y la actividad de red en segundo plano en un entorno VDI:

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Recibir notificaciones de aplicaciones y otros remitentes| Activado| Desactivado|
|Muestra las notificaciones en la pantalla de bloqueo.|    Activado| Desactivado|
|Mostrar llamadas entrantes de VoIP, recordatorios y alarmas en la pantalla de bloqueo.|   Activado| Desactivado|
|Mostrar sugerencias, trucos y sugerencias que usa Windows.|    Activado| Desactivado|


#### <a name="offline-maps"></a>Mapas sin conexión

Esta opción solo es aplicable si está instalada la aplicación de mapas. Su valor predeterminado es **en**; para el valor recomendado es de uso VDI **desactivar**. 

#### <a name="tablet-mode"></a>Modo tableta

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Cuando se inicia sesión en|    Use el modo adecuado para mi hardware|   Usar el modo de escritorio|
|Cuando este dispositivo cambia automáticamente el modo activado o desactivado|    Preguntar siempre antes de cambiar| No preguntar y no cambiar|
|Las opciones Ocultar los iconos de aplicación en la barra de tareas en el modo tableta|  Activado| Desactivado|


### <a name="in-the-devices-area-of-windows-settings"></a>En el área "Dispositivos" de la configuración de Windows
En el área de configuración de Windows, haga clic en el **dispositivos** icono proporciona acceso a una serie de valores relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso VDI, estas configuraciones son las más importantes:

#### <a name="autoplay"></a>Reproducción automática

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Usar la reproducción automática para todos los medios y dispositivos|    Activado| Desactivado|
|Unidad extraíble:|Elija un valor predeterminado|No realiza ninguna acción|
|Tarjeta de memoria|Elija un valor predeterminado|No realiza ninguna acción|

### <a name="in-the-personalization-area-of-windows-settings"></a>En el área "Personalización" de la configuración de Windows
En el área de configuración de Windows, haga clic en el **personalización** icono proporciona acceso a una serie de valores relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso VDI, estas configuraciones son las más importantes:

#### <a name="background"></a>Background
A veces, el fondo negro predeterminado puede hacer que los usuarios a pensar que el equipo no responde. Cambiar el color de fondo puede ayudar a aclararlo. Para ello, realice los pasos siguientes:
1. En el **en segundo plano** área, haga clic en el menú desplegable.
2. Para cambiar el color de fondo, haga clic en **color sólido**y, a continuación, haga clic en cualquiera de los colores que no sea negro. Como alternativa, puede hacer clic en **imagen** y, a continuación, seleccione una imagen para usar como fondo.

#### <a name="start"></a>Comienzo

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|En ocasiones, mostrar sugerencias en el menú Inicio|    Activado| Desactivado|
|Mostrar usado más aplicaciones|Activado|Desactivado|
|Mostrar las aplicaciones agregadas recientemente|Activado|Desactivado|
|Mostrar elementos abiertos recientemente en Jump Lists en el inicio o la barra de tareas|Activado|Desactivado|

#### <a name="taskbar"></a>Barra de tareas
El valor predeterminado es usar los botones de barra de tareas de gran tamaño (es decir, un valor de "Desactivada" para **usar botones pequeños**). Esta configuración hace que el elemento de Cortana usar una gran cantidad de área de barra de tareas. Para evitar esto, establezca **usar botones pequeños** en "Activado". Si prefiere que los elementos de la barra de tareas permanecen mayor, pero prefieren no tener Cortana ocupar mucho espacio, haga clic en la barra de tareas, elija **Cortana**y en el menú que sale por, seleccione **Hidden**.

### <a name="in-the-privacy-area-of-windows-settings"></a>En el área "Privacy" de la configuración de Windows
En el área de configuración de Windows, haga clic en el **privacidad** icono proporciona acceso a una serie de valores relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso VDI, estas configuraciones son las más importantes:

#### <a name="general"></a>General
Algunas de estas opciones también se establecen en la ventana "Personalizar la configuración", que se describe al principio de este tema.

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Permitir que las aplicaciones usar mi Id. de publicidad para experiencias en las aplicaciones (esto al desactivar will restablece su Id.)|  Activado| Desactivado|
|Dejar que los sitios web ofrezcan contenido relevante a nivel local mediante el acceso a mi lista de idiomas|Activado|Desactivado|
|Permitir que las aplicaciones en mis aplicaciones de otros dispositivos abierto y continuar experiencias en este dispositivo|Activado|Desactivado|

#### <a name="camera"></a>Cámara

El valor predeterminado para "Permitir que las aplicaciones usen mi cámara" es **en**; para el valor recomendado es de uso VDI **desactivar**.


#### <a name="microphone"></a>Micrófono

El valor predeterminado para "Permitir que las aplicaciones usen mi micrófono" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="notifications"></a>Notificaciones

El valor predeterminado para "Permitir que las aplicaciones tener acceso a mis notificaciones" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="contacts"></a>Contactos

El valor predeterminado para "Permitir que las aplicaciones tener acceso a Mis contactos" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="calendar"></a>Calendario

El valor predeterminado para "Permitir que las aplicaciones tener acceso a mi calendario" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="call-history"></a>Historial de llamadas

El valor predeterminado para "Permitir que las aplicaciones tener acceso a su historial de llamadas" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="email"></a>Correo electrónico

Es el valor predeterminado de "Permiten el acceso a aplicaciones y enviar correo electrónico" **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="messaging"></a>Mensajería

El valor predeterminado para "Permitir que las aplicaciones leer o enviar mensajes (texto o MMS)" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="radios"></a>Señales de radio

El valor predeterminado para "Permitir que los radios de control de aplicaciones" es **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="other-devices"></a>Otros dispositivos

Es el valor predeterminado de "Permitir que las aplicaciones automáticamente compartirán y sincronización información con dispositivos inalámbricos que no estén emparejados de forma explícita con su PC, tableta o teléfono" **en**; para el valor recomendado es de uso VDI **desactivar**.

#### <a name="feedback-and-diagnostics"></a>Comentarios y diagnósticos

El valor predeterminado es "Windows deben pedir mis comentarios" **automáticamente**; para el uso VDI, el valor recomendado es **nunca**.

#### <a name="background-apps"></a>Aplicaciones en segundo plano
Aplicaciones de la lista tienen un valor predeterminado de **en**, lo que les permite recibir información, enviar notificaciones y se actualizan si se están utilizando o no. Debe deshabilitar (establecido en **desactivar**) las aplicaciones no desea que se ejecuta en segundo plano en la imagen VDI.

### <a name="update-and-security"></a>Actualización y seguridad
#### <a name="windows-update"></a>Windows Update
En el **actualizar la configuración de** área, haga clic en **opciones avanzadas** ajustar estos valores:

|Parámetro|Valor predeterminado|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Ofrecerme actualizaciones para otros productos de Microsoft al actualizar Windows|    Desactivada|    seleccionado|
|Aplazar actualizaciones de características|Desactivada|seleccionado|
|Usar mi información de inicio de sesión para finalizar automáticamente la configuración de mi dispositivo después de una actualización |Desactivada|Depende de la configuración de VDI específica|

En el **opciones avanzadas** página, haga clic en **elija cómo las actualizaciones se entregan** para tener acceso a la configuración de "Actualizaciones de más de un lugar". El valor predeterminado es **en**; para el valor recomendado es de uso VDI **desactivar**.

## <a name="in-control-panel-and-other-system-utilities"></a>En el Panel de Control y otras utilidades del sistema

La configuración de esta sección es ajustable por desplazarse por el Panel de Control o abrir la utilidad directamente.

> [!NOTE]  
> Cualquier configuración que no se especifican explícitamente en este tema puede dejarse en sus valores predeterminados (o conjunto por sus requisitos y directivas) sin impacto apreciable en la funcionalidad VDI.


### <a name="task-scheduler"></a>Programador de tareas
Es la forma más rápida para abrir el programador de tareas Insertar el botón de Windows y escriba *programador de tareas* o *taskschd.msc*. En los que devuelven resultados, haga clic en **programador de tareas** para abrir la utilidad. En el programador de tareas, expanda **biblioteca del programador de tareas**, expanda **Microsoft**y, a continuación, expanda **Windows**. Ahora tiene acceso a la lista de colecciones de tareas. Para cambiar el estado de cada tarea programada, haga clic en él y, a continuación, haga clic en el estado deseado (por lo general, **deshabilitado** para su uso VDI).

|Colección de tareas|Nombre de la tarea|Estado predeterminado|Estado recomendado para su uso VDI|  
|-------------------|-------------|----------|--------------|
|Programa para la mejora de la experiencia del usuario||||
||Consolidator|Enabled|Deshabilitada|
||KernelCeipTask|Enabled|Deshabilitada|
||UsbCeip|Enabled|Deshabilitada|
|desfragmentación||||
||ScheduledDefrag|Enabled|Deshabilitada|
|Location||||
||Notificaciones|Enabled|Deshabilitada|
||WindowsActionDialog|Enabled|Deshabilitada|
|Mantenimiento||||
||WinSAT|Enabled|Deshabilitada|
|Maps||||
||MapsToastTask|Enabled|Deshabilitada|
||MapsUpdateTask|Enabled|Deshabilitada|
|Cuentas de banda ancha móviles||||
||Analizador de metadatos MNO|Enabled|Deshabilitada|
|Diagnóstico de eficiencia energética||||
||Analizar sistema|Enabled|Deshabilitada|
|Entorno de recuperación||||
||VerifyWinRE|Enabled|Deshabilitada|
|Demostración de venta directa||||
||CleanupOfflineContent|Enabled|Deshabilitada|
|Shell||||
||FamilySafetyMonitor|Enabled|Deshabilitada|
||FamilySafetyRefreshTask|Enabled|Deshabilitada|
|Informe de errores de Windows||||
||QueueReporting|Enabled|Deshabilitada|
|Uso compartido de multimedia de Windows||||
||UpdateLibrary|Enabled|Deshabilitada|

Haga clic en **Windows** nuevo para contraerlo, a continuación, haga clic en **XblGameSave**. Esto le otorga acceso a las tareas **XBLGameSaveTask** y **XBLGameSaveTaskLogon**; ambos se pueden establecer en **deshabilitado**.

### <a name="performance-monitor"></a>Performance Monitor
Es la forma más rápida para abrir el Monitor de rendimiento insertar el botón de Windows y escriba *monitor de rendimiento* o *perfmon.msc*. En los que devuelven resultados, haga clic en **Monitor de rendimiento**. En el Monitor de rendimiento, haga clic en **conjuntos de recopiladores de datos** y, a continuación, haga doble clic en **sesiones de seguimiento de eventos**. Haga clic en **WiFiSession**; si se encuentra en el estado predeterminado de **ejecutando**, a continuación, haga clic en **detener**.

Haga clic en **StartupEventTraceSessions**, a continuación, haga clic en **ReadyBoot**; si se está ejecutando, haga clic en **detener**. Haga clic en **sesiones de seguimiento de eventos**, haga clic en **ReadyBoot**y, a continuación, haga clic en **propiedades**. En el cuadro de diálogo que se abre, haga clic en el **sesión de seguimiento** ficha. Desactive el **habilitado** casilla de verificación.

### <a name="services"></a>Servicios
Es la forma más rápida para administrar los servicios insertar el botón de Windows y escriba *servicios*. En los que devuelven resultados, haga clic en **servicios**. Los siguientes servicios son buenos candidatos para deshabilitar para su uso en escenarios VDI; Sin embargo, es posible que deba hacer algunas pruebas para comprobar que no son necesarios para sus fines. Para deshabilitar un servicio, en el **servicios** complemento, haga clic en el nombre del servicio y, a continuación, haga clic en **propiedades**. En el **General** pestaña, haga clic en el **tipo de inicio** menú desplegable y, a continuación, haga clic en **deshabilitado**. Haga clic en **Aceptar**.

- BranchCache
- Optimización de distribución
- Host de servicio de diagnóstico
- Servicio de zona con cobertura inalámbrica móvil de Windows
- Administrador de Xbox Live Auth
- Guardar juego de Xbox Live
- Servicio de red de Xbox Live

### <a name="file-explorer-options"></a>Opciones del explorador de archivos
El botón de Windows y el tipo de inserción *panel de control*. En los que devuelven resultados, haga clic en **Panel de Control**. En el Panel de Control, haga clic en **opciones del explorador de archivos**. En el cuadro de diálogo que se abre, haga clic en el **búsqueda** ficha y, a continuación, en el **al buscar ubicaciones no indizadas** área, desactive la casilla de **incluyen los directorios del sistema**. Haga clic en **Aceptar** para guardar.

### <a name="flash-settings"></a>Configuración del Flash
El botón de Windows y el tipo de inserción *panel de control*. En los que devuelven resultados, haga clic en **Panel de Control**. En el Panel de Control, haga clic en **Flash Player** para abrir el Administrador de configuración del Reproductor de Flash. En el **almacenamiento** pestaña, seleccione el botón de radio para **bloquear todos los sitios de almacenar información en este equipo**. En el cuadro de diálogo que se abre, haga clic en **Aceptar**.

En el **cámara y micrófono** ficha la **cámara y micrófono configuración** área, seleccione el botón de opción para **bloquear todos los sitios del uso de la cámara y micrófono**.

En el **reproducción** ficha la **redes P2P** área, seleccione el botón de opción para **bloquear todos los sitios del uso de las redes P2P**. Cierre el Administrador de configuración de Flash Player.

### <a name="internet-options"></a>Opciones de Internet
El botón de Windows y el tipo de inserción *panel de control*. En los que devuelven resultados, haga clic en **Panel de Control**. En el Panel de Control, haga clic en **opciones de Internet** para abrir las propiedades de Internet. En el **página principal** área, escriba la dirección URL para el sitio web que desea que los usuarios puedan ver como página principal en los exploradores. Podría tratarse de un sitio web de su empresa o puede establecerlo en una página de inicio en blanco escribiendo *acerca de: en blanco*.

En el **historial de exploración** área, seleccione la casilla de verificación **eliminar historial de exploración al salir**.

### <a name="power-options"></a>Opciones de energía
El botón de Windows y el tipo de inserción *panel de control*. En los que devuelven resultados, haga clic en **Panel de Control**. En el Panel de Control, haga clic en **opciones de energía** para abrir el panel de control de opciones de energía. En el **elegir o personalizar un plan de energía** área, haga clic en la flecha hacia abajo para **Mostrar planes adicionales**y, a continuación, seleccione el botón de radio para **de alto rendimiento**. Esta configuración tendrá muy poco impacto en el host VDI.

### <a name="system"></a>Sistema
El botón de Windows y el tipo de inserción *panel de control*. En los que devuelven resultados, haga clic en **Panel de Control**. En el Panel de Control, haga clic en **System** para abrir el panel de control del sistema. En el panel izquierdo, haga clic en **configuración avanzada del sistema**. En el cuadro de diálogo que se abre, haga clic en el **avanzadas** ficha. En el **rendimiento** área, haga clic en el **configuración** botón, a continuación, en **efectos visuales** ficha en el cuadro de diálogo que se abre, selecciona el **ajustar para mejorar el rendimiento**  botón de radio. Haga clic en **Aceptar** para guardar y salir.

## <a name="group-policy-settings"></a>Configuración de directiva de grupo

Para editar la configuración de directiva de grupo, presione el botón de Windows y escriba *directiva de grupo* o *gpedit.msc*. En los que devuelven resultados, haga clic en **Editar directiva de grupo** para abrir el Editor de directivas de grupo Local.

> [!NOTE]  
> Cualquier configuración que no se especifican explícitamente en este tema puede dejarse en sus valores predeterminados (o conjunto por sus requisitos y directivas) sin impacto apreciable en la funcionalidad VDI.

En **configuración del equipo**, expanda **Windows configuración**y, a continuación, expanda **configuración de seguridad**. Haga clic en **directivas del Administrador de red lista**y, a continuación, haga doble clic en **todas las redes**. En el cuadro de diálogo que se abre, en el **ubicación de red** área, seleccione el botón de opción para **usuario no puede cambiar la ubicación**. Haga clic en el **Aceptar** botón para guardar.

Contraer **Windows configuración**y, a continuación, expanda **plantillas administrativas**. Haga clic o expanda **red**y, a continuación, ajustar cada configuración como se indica a continuación haga doble clic en él, a continuación, seleccionando el botón de radio para el valor indicado y haciendo clic en el **Aceptar** botón:

|Configuración de área|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|-------|----------|
|Servicio de transferencia inteligente en segundo plano (BITS)|||
||No se permiten al cliente BITS utiliza BranchCache de Windows|Enabled|
||No permitir que el equipo para que actúe como un cliente de caché del mismo nivel de BITS|Enabled|
||No permitir que el equipo actúe como un servidor de caché del mismo nivel de BITS|Enabled|
||Permitir que la caché del mismo nivel de BITS|Deshabilitada|
|BranchCache||
||Activar BranchCache|Deshabilitada|
|Autenticación de punto de conexión||
||Habilitar la autenticación de punto de conexión|Deshabilitada|
|Servicios de redes punto a punto de Microsoft||
||Desactivar todos los servicios de redes punto a punto de Microsoft|Enabled|
|Archivos sin conexión||
||Permitir o denegar el uso de la característica archivos sin conexión|Deshabilitada|

Contraer **red**y, a continuación, expanda **sistema**. Ajustar cada valor doble clic en él, como se indica a continuación, a continuación, seleccione el botón de radio para el valor indicado y haga clic en el **Aceptar** botón:

|Configuración de área|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|----------|--------------|
|Instalación de dispositivos||
||No se enviará un informe de error de Windows cuando se instala un controlador genérico en un dispositivo|Enabled|
||Impedir la creación de un punto de restauración del sistema durante la actividad de los dispositivos que normalmente se haría solicita la creación de un punto de restauración|Enabled|
||Evitar la recuperación de metadatos de dispositivo de Internet|Enabled|
||Impedir que Windows envíe un informe de errores cuando un software de las solicitudes de controlador de dispositivo adicionales durante la instalación|Enabled|
||Desactivar los globos de "Nuevo Hardware encontrado" durante la instalación del dispositivo|Enabled|

Expanda **Filesystem**, haga doble clic en **NTFS**, haga doble clic en **las opciones de creación de nombre cortas**, seleccione el botón de radio para **habilitado**, y, a continuación, utilice el **opciones** menú desplegable para seleccionar **habilitar en todos los volúmenes**. Haga clic en el **Aceptar** botón para guardar.

Contraer **Filesystem**y, a continuación, expanda **administración de comunicaciones de Internet**. Haga clic en **de comunicaciones de Internet**. Ajustar cada configuración como se indica a continuación, haga doble clic en él y, después, seleccione el botón de radio para **habilitado**y, a continuación, haga clic en el **Aceptar** botón:

- Deshabilitar vínculos "Events.asp" del Visor de eventos
- Desactivar compartir datos de personalización de escritura a mano
- Desactivar informe de errores de reconocimiento de escritura a mano
- Desactivar el centro de ayuda y soporte técnico "¿Sabía...?" content
- Desactivar la búsqueda de ayuda y soporte técnico de centro de Microsoft Knowledge Base
- Desactivar al Asistente para conexión de Internet si la conexión de direcciones URL hace referencia a Microsoft.com
- Desactivar la descarga de Internet para la publicación en Web y los asistentes de pedidos en línea
- Desactivar el servicio de asociaciones de archivo de Internet
- Desactivar el registro si la conexión de direcciones URL hace referencia a Microsoft.com
- Desactivar la tarea de imágenes «Pedir copias fotográficas»
- Desactivar la tarea «Publicar en Web» para archivos y carpetas
- Desactivar el programa para la mejora de la experiencia del cliente de Windows Messenger
- Desactivar Windows Customer Experience Improvement Program
- Desactivar informe de errores de Windows
- Desactivar la búsqueda de controladores de dispositivo de Windows Update

Haga clic en **administración de energía** y, a continuación, haga doble clic en **seleccionar un plan de energía activo**. Seleccione el botón de radio para **habilitado**y, a continuación, utilice el **opciones** menú desplegable para seleccionar **de alto rendimiento**. Haga clic en el **Aceptar** botón para guardar.

Haga clic en **recuperación**y, a continuación, haga doble clic en **permitir la restauración del sistema al estado predeterminado**. Seleccione el botón de radio para **habilitado**y, a continuación, haga clic en el **Aceptar** botón para guardar.

Expanda **solución de problemas y diagnóstico**. Haga clic en **mantenimiento programado**, haga doble clic en **configurar comportamiento mantenimiento programado**y, a continuación, seleccione el botón de radio para **deshabilitado**. Haga clic en el **Aceptar** botón para guardar.

Para cada una de las siguientes áreas de configuración, haga clic en él y, a continuación, haga doble clic en **configurar el nivel de ejecución de escenario**, seleccione el botón de radio para **deshabilitado**y, a continuación, haga clic en el **Aceptar**botón para guardar:

- Diagnóstico de rendimiento de arranque de Windows
- Diagnóstico de pérdida de memoria de Windows
- Resolución y detección de agotamiento de recursos de Windows
- Diagnóstico de rendimiento de apagado de Windows
- Diagnóstico de rendimiento de suspensión/reanudación de Windows
- Diagnóstico de rendimiento de la capacidad de respuesta del sistema de Windows

Contraer **sistema**y, a continuación, expanda **componentes de Windows**. Ajustar cada configuración como se indica a continuación haga doble clic en él, a continuación, seleccionando el botón de radio para el valor indicado y haciendo clic en el **Aceptar** botón:

|Configuración de área|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|-------|----------|
|Agregar características a Windows 10|||
||Impedir que se ejecute el Asistente|Enabled|
|Directivas de reproducción automática|||
||Configura el comportamiento predeterminado para la ejecución automática|Habilitado, a continuación, utilice el **opciones** menú desplegable para seleccionar **no se ejecutan los comandos de ejecución automática**|
|Cloud Content|||
||No mostrar sugerencias de Windows|Enabled|
||Desactivar experiencias del consumidor de Microsoft|Enabled|
|Recopilación de datos y las compilaciones de versión preliminar|||
||Permitir telemetría|Habilitado, a continuación, utilice el **opciones** menú desplegable para seleccionar **1: básico**|
||Deshabilitar las características o configuración de versión preliminar|     Deshabilitada|
||No volver a mostrar notificaciones de comentarios|       Enabled|
||Alternar control de usuario sobre compilaciones de Insider|      Deshabilitada|
|Administrador de ventanas de escritorio|||
||No permitir la invocación de Flip3D|       Enabled|
||No permitir animaciones de ventanas|       Enabled|
||Usar color sólido para el fondo de inicio|     Enabled|
|Borde de la interfaz de usuario|||
||Permitir pasar el dedo edge|     Deshabilitada|
||Deshabilitar las sugerencias de ayuda|        Enabled|
|Explorador de archivos|||
||No mostrar la notificación 'nueva aplicación instalada'|     Enabled|
|Explorador de juegos|||
||Desactivar la descarga de la información de juegos|     Enabled|
||Desactivar las actualizaciones de juegos|        Enabled|
||Desactivar el seguimiento del último tiempo de reproducción de juegos en la carpeta juegos|     Enabled|
|Grupo Hogar|||
||Impedir que el equipo se una a un grupo en el hogar|        Enabled|
|Internet Explorer|||
||Permitir que los servicios Microsoft proporcionen sugerencias mejoradas mientras el usuario escribe en la barra de direcciones|        Deshabilitada|
||Deshabilitar comprobación periódica de actualizaciones de software de Internet Explorer|        Enabled|
||Deshabilitar la visualización de la pantalla de presentación|        Enabled|
||Instalar nuevas versiones de Internet Explorer automáticamente|      Deshabilitada|
||Impedir la participación en el Customer Experience Improvement Program|     Enabled|
||Evitar la ejecución primera ejecución del Asistente para ir directamente a la página principal|   Habilitado, a continuación, utilice el **opciones** menú desplegable para seleccionar **ir directamente a la página principal**|
||Establecer el crecimiento del proceso de pestaña|Habilitado, a continuación, escriba lo siguiente en el **pestaña proceso de crecimiento** cuadro: *Baja*.|
||Especificar el comportamiento predeterminado para una nueva pestaña|Habilitado, a continuación, utilice el **opciones** menú desplegable para seleccionar **nueva página de ficha**|
||Desactivar las notificaciones de rendimiento de complementos|        Enabled|
||Desactivar geoubicación del explorador|     Enabled|
||Desactivar volver a abrir última sesión de exploración|        Enabled|
||Desactivar las sugerencias para todos los proveedores instalados por el usuario|        Enabled|
||Activar sitios sugeridos|       Deshabilitada|

En el mismo nivel que el **Internet Explorer** configuración solo ajusta en la tabla anterior, tenga en cuenta otro nivel de carpetas que abarcan desde **aceleradores** a **las barras de herramientas**. En otras palabras, ahora está en la directiva de equipo Local > configuración del equipo > plantillas administrativas > componentes de Windows > Internet Explorer. 

Abra el **eliminar el historial de exploración** carpeta, haga doble clic en **permite eliminar el historial de exploración al salir**, seleccione **habilitar**y, a continuación, haga clic en **Aceptar**para guardar y salir.

Use la flecha Atrás que aparece en la esquina superior izquierda del Editor de directivas de Local grupo para volver a la **Internet Explorer** nivel. Haga doble clic en **configuración de Internet**, haga doble clic en **configuración avanzada**y, a continuación, ajuste la configuración en las subcarpetas del siguiente modo:

|Carpeta de configuración en **configuración avanzada**|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|-------|----------|
|**Browsing**|||
||Desactivar detección de número de teléfono|Enabled|
|**Multimedia**|||
||Permita que Internet Explorer reproducir archivos multimedia que usan códecs alternativos|Deshabilitada|

Vaya a copia de seguridad en el nivel de **Internet Explorer**, a continuación, haga doble clic en **configuración de Internet**. En esta carpeta, establezca estos dos valores en **Autocompletar** a **habilitado**:

- Desactivar las sugerencias de URL
- Desactivar Windows Search Autocompletar

Vaya a copia de seguridad de cuatro niveles a **componentes de Windows**, haga doble clic en **ubicación y sensores**y, a continuación, establezca estas tres opciones **habilitado** (para cada uno, haga clic en  **Aceptar** para guardar y salir):

- Desactive la opción de ubicación
- Desactive la opción de scripting de ubicación
- Desactivar sensores

Tiempo en el nivel de **ubicación y sensores**, haga doble clic en **proveedor de ubicación de Windows** y establecer **desactivar el proveedor de ubicación de Windows** a **habilitado**. Haga clic en **Aceptar** para guardar y salir.

En el panel izquierdo, haga clic en **mapas**, establecer esta configuración en **habilitado**; para cada uno, a continuación, haga clic en **Aceptar** para guardar y salir:

- Desactivar la descarga y actualización automáticas de los datos de mapa
- Desactivar el tráfico de red no solicitado en la página Configuración de mapas sin conexión

Mediante el panel izquierdo, escriba cada una de las siguientes subcarpetas de la configuración y ajustar los valores individuales como sigue:

|Carpeta de configuración en **componentes de Windows**|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Impedir el uso de OneDrive para almacenar archivos|Enabled|
||Guardar documentos en OneDrive de forma predeterminada|Deshabilitada|
|**Fuentes RSS**|||
||Evitar la detección automática de las fuentes y Web Slices|Enabled|
|**Buscar**|||
||Permitir a Cortana|        Deshabilitada|
||Permitir a Cortana por encima de la pantalla de bloqueo|      Deshabilitada|
||Permitir el uso de la ubicación para las búsquedas y Cortana|     Deshabilitada|
||No permitir búsquedas en la Web|      Enabled|
||No buscar en la web o mostrar los resultados web en la búsqueda|        Enabled|
||Impedir la adición de UNC ubicaciones al índice desde el Panel de Control|     Enabled|
||Evitar la indización de archivos en memoria caché de archivos sin conexión|        Enabled|
|**Store**|||
||Desactivar la oferta para actualizar a la versión más reciente de Windows|Enabled|
|**Informe de errores de Windows**|||
||Enviar automáticamente los volcados de memoria para los informes de errores generados por el sistema operativo|       Deshabilitada|
||Deshabilitar Informe de errores de Windows|      Enabled|
|**Windows Installer**|||
||Tamaño máximo del control de caché de archivos de base de referencia|  Habilitado, a continuación, usar la caja de número en el **opciones** área para establecer **tamaño máximo de caché de archivos de instantánea** a *5*.|
||Desactivar la creación de puntos de restauración del sistema de control|      Enabled|
|**Windows Mail**|||
||Desactivar la característica de Comunidades|Enabled|
|**Reproductor de Windows Media**|||
||No mostrar cuadros de diálogo de primer uso|       Enabled|
||Evitar el uso compartido de multimedia|        Enabled|
|**Centro de movilidad de Windows**|||
||Desactivar el centro de movilidad de Windows|Enabled|
|**Análisis de confiabilidad de Windows**|||
||Configurar proveedores WMI de confiabilidad|Deshabilitada|
|**Windows Update**|||
||Permitir la instalación inmediata de actualizaciones automáticas|       Enabled|
||Quitar el acceso a todas las características de Windows Update|     Enabled|
|En el **Windows Update** carpeta abierta **aplazar la actualización de Windows**|||
||Seleccione cuando se reciben las actualizaciones de características|Habilitado, a continuación, en el **opciones** área, use el **seleccione el nivel de preparación de rama para las actualizaciones de características que desea que reciban** menú desplegable para seleccionar **rama actual para empresas**. Establecer el **después del lanzamiento de una actualización de características, aplazar la recepción, para este número de días** caja de número a *180 días*.
||Seleccione cuando se reciben las actualizaciones de calidad|Habilitado, a continuación, en el **opciones** área, establezca el **después del lanzamiento de una actualización de calidad, aplazar la recepción, para este número de días** caja de número a *30 días* y seleccione la casilla de verificación **Pausar las actualizaciones de calidad**.

En el panel izquierdo del Editor de directiva de grupo Local, haga clic en **configuración de usuario**. Con el panel izquierdo, haga clic en **plantillas administrativas** y, a continuación, escriba cada una de las siguientes subcarpetas de la configuración y ajustar los valores individuales como sigue:

|Carpeta de configuración en **plantillas administrativas**|Parámetro|Valor recomendado para su uso VDI|  
|-------------------|-------|----------|
|**Equipo de escritorio**|||
||No agregue los recursos compartidos de documentos abiertos recientemente a ubicaciones de red|Enabled|
|En el **Desktop** carpeta abierta **Active Directory**|||
||Tamaño máximo de las búsquedas de Active Directory|Habilitado, a continuación, en el **opciones** área, use la caja de número para establecer **número de objetos devueltos** a *5000*.|
|**Manu de inicio y barra de tareas**|||
||Borrar la lista de programas recientes para los nuevos usuarios|     Enabled|
||No mostrar elementos, ni realizar el seguimiento de los mismos, en las listas de accesos directos desde ubicaciones remotas|        Enabled|
||Desactivar los globos de notificaciones de anuncios de características|     Enabled|
||Desactivar el seguimiento del usuario|       Enabled|
|En el **menú Inicio y barra de tareas** carpeta abierta **notificaciones**|||
||Desactivar notificaciones del sistema|Enabled|
|En el **componentes de Windows** carpeta abierta:|||
|**Contenido en la nube**|||
||Desactivar todas las características de Contenido destacado de Windows|Enabled|
|**Explorador de archivos**|||
||Desactivar el almacenamiento en caché de imágenes en miniatura|       Enabled|
||Desactivar la visualización de las entradas de búsqueda recientes en el cuadro de búsqueda del explorador de archivos|        Enabled|
||Desactivar el almacenamiento en caché de miniaturas en el archivo oculto thumbs.db|      Enabled|

## <a name="microsoft-store-apps"></a>Aplicaciones de Microsoft Store
Hay una serie de aplicaciones de Microsoft Store que desea quitar de la imagen VDI. quitarlos se reducir el uso de CPU y conservar espacio en disco. Son buenas candidatas para la eliminación:

- Obtener Office
- Skype (versión preliminar)
- Introducción (especialmente si no hay ninguna conexión a Internet)
- Centro de opiniones
- Colección de Solitario de Microsoft
- Pago Wi-Fi y móvil

Para personalizar el perfil de usuario predeterminado utilizado para crear imágenes VDI, utilice la cuenta predefinida Administrador. Si aún no está habilitado, puede hacerlo mediante el uso de usuarios y grupos locales en administración de equipos. A continuación, inicie sesión en la cuenta de administrador para completar los pasos siguientes.

> [!NOTE]  
> No quite las aplicaciones del sistema, como el app Store. Son difíciles de volver a instalar. Otras aplicaciones están reinstallable fácilmente desde el Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Eliminar aplicaciones no deseadas desde el perfil de usuario de administrador
1. En Windows PowerShell, ejecute `Get-AppxPackage | ft PackageFamilyName` para ver la lista de las aplicaciones instaladas.
2. Para cada Empaquetador de aplicaciones que desea desinstalar ejecute cmdlets de este formato de ejemplo:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Eliminar la carga de aplicaciones no deseadas de Store
Esto impedirá que las aplicaciones que se va a volver a instalar.
1. Aplicaciones de la lista Store y otros elementos que se han aprovisionado los datos en el almacenamiento con este cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Quitar un paquete determinado con `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, utilizando el MyAppPackage adecuado devuelta del paso 1. Por ejemplo, para quitar el paquete de Zune, ejecute `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Eliminación de otros elementos
Puede quitar la aplicación y el icono de OneDrive, desactivar los iconos de sistema y eliminar las actualizaciones descargadas.

### <a name="remove-onedrive-icon-and-app"></a>Quitar la aplicación y el icono de OneDrive
1. Haga clic en **iniciar** y desplácese hasta la **OneDrive** icono.
2. Haga clic en el **OneDrive** icono, seleccione **más**y, a continuación, haga clic en **Abrir ubicación del archivo**.
3. Haga clic en el **OneDrive** en su ubicación del archivo y haga clic en icono **eliminar**.

Para quitar la aplicación OneDrive:
1. Haga clic en **iniciar** y desplácese hasta la **OneDrive** icono.
2. Haga clic en el **OneDrive** icono y, a continuación, haga clic en **desinstalar**. Programas y se abre de características.
3. En programas y características, haga clic en **Microsoft OneDrive** y haga clic en **desinstalar**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programas y características (desde versiones anteriores del Panel de Control)
1. Inserte el **iniciar** , escriba *Control*, y, a continuación, presione ENTRAR.
2. Pulse o haga doble clic en **programas y características**.
3. En el extremo izquierdo, bajo **principal del Panel de Control**, pulse o haga clic en **o desactivar las características de Windows Active**. Se abrirá una nueva interfaz de usuario.
4. Desactive las casillas de todos los elementos que no desee o necesite en la imagen base, por ejemplo: **Compatibilidad con uso compartido de archivos de SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Desactivar los iconos del sistema
1. Insertar o haga clic en **iniciar**y, a continuación, haga clic en **configuración** (el icono de engranaje).
2. En el **buscar una configuración** área de texto, escriba *barra de tareas*y, a continuación, haga clic en **configuración de la barra de tareas**.
3. En el **barra de tareas** sección, desplácese o deslice el dedo hacia abajo hasta la **área de notificación** sección.
4. Haga clic o pulse **activar y desactivar los iconos de sistema**y, a continuación, activar cada icono del sistema o desactivar como desee para la imagen.

### <a name="delete-downloaded-updates"></a>Eliminar actualizaciones descargadas
1. Mediante el Explorador de archivos, vaya a **C:\Windows\Software Distribution\Download**.
2. Eliminar todos los archivos y carpetas en ese directorio.













 













