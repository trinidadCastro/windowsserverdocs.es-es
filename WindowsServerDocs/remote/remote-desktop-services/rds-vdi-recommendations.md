---
title: Configuración recomendada para equipos de escritorio VDI
description: Configuración y ajustes recomendados para minimizar la sobrecarga de los dispositivos de escritorio Windows 10, versión 1607 (10.0.1393), que se usan como imágenes VDI
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 2ab78ccbc4e49bd95a74fe1e17d5ea14891eb1b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857278"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Configuración recomendada para dispositivos de escritorio VDI

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016 y Windows 10

Microsoft Desktop Virtualization detecta automáticamente las configuraciones de dispositivo y las condiciones de red para que los usuarios puedan empezar a trabajar más rápido. Esto se consigue habilitando la configuración instantánea de dispositivos de escritorio y aplicaciones corporativas. De este modo, el equipo de TI puede proporcionar acceso a las aplicaciones heredadas durante la migración a Windows 10.

Aunque el sistema operativo Windows 10 se ajusta muy bien desde el principio, se puede mejorar aún más con respecto al entorno corporativo de infraestructura de escritorio virtual (VDI) de Microsoft. En el entorno de VDI, muchos servicios y tareas en segundo plano están deshabilitados desde el comienzo.

Este tema no es un proyecto, sino una guía o un punto de partida. Algunas recomendaciones podrían deshabilitar funcionalidades que quizás prefiera usar, por lo que hay que tener en cuenta el costo en comparación con la ventaja de ajustar cualquier configuración específica en tu escenario.

Estas instrucciones y la configuración recomendada son pertinentes para Windows 10 1607 (versión 10.0.1393).

> [!NOTE]
> Cualquier configuración que no se mencione expresamente en este tema puede dejarse en sus valores predeterminados (o ajustarla en función de los requisitos y directivas) sin que repercuta en la funcionalidad de VDI.

Cuando se crea una imagen que se basará la implementación de VDI, hay que asegurarse de usar la **rama actual**. Para obtener más información sobre la rama actual, consulta [Información de versión de Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Creación de la imagen de Windows 10
El primer paso es instalar una imagen de referencia de Windows 10 1607 (versión 10.0.1393) en la máquina virtual o física. Realizar la instalación en una máquina virtual es fácil y permite guardar las versiones del archivo de disco duro virtual (VHD), en caso de que quieras revertir a una versión anterior.

Durante la instalación, puedes elegir **Configuración rápida** o **Personalizar**. La configuración que se ofrece durante la opción **Personalizar** se puede ajustar mediante una directiva de grupo, por lo que no es importante el método de instalación del sistema operativo base.


Si elegiste **Personalizar**, puedes ajustar estos valores durante la instalación:

## <a name="in-customize-settings"></a>En "Personalizar configuración"

También puedes ajustar estos valores después de la instalación con el Editor de directivas de grupo. Consulta la sección "Configuración de la directiva de grupo" de este tema.

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|**Personalización**| | |
|Personaliza la entrada de voz, de escritura y la entrada manuscrita mediante el envío de los datos de entrada a Microsoft.|    Activado| Desactivado|
|Enviar a Microsoft datos de escritura y entrada de lápiz para mejorar el reconocimiento y la plataforma de sugerencias.|  Activado| Desactivado|
|Deja que las aplicaciones usen tu identificador de publicidad para las experiencias entre aplicaciones.|  Activado| Desactivado|
|Permite que Skype (si está instalado) te ayude a conectarte con amigos de tu libreta de direcciones y compruebe tu número de móvil. Se pueden aplicar cargos de datos y SMS.|    Activado| Desactivado|
|**Ubicación**| | |
|Activa Encontrar mi dispositivo y permite que Windows y las aplicaciones soliciten conocer tu ubicación, con el historial de ubicaciones| Activado| Desactivado|
|Informes sobre conectividad y errores| | |
|Conectarse automáticamente a zonas Wi-Fi sugeridas con cobertura abierta No todas las redes son seguras.|    Activado| Desactivado|
|Se conecta automáticamente para abrir las zonas con cobertura inalámbrica temporalmente con el fin de ver si hay servicios de red de pago disponibles.| Activado| Desactivado|
|Enviar datos de uso y de diagnóstico completos a Microsoft. Al desactivar esto solo se envían datos básicos.| Activado| Desactivado|
|**Explorador, protección y actualización**| | |
|Usar los servicios en línea SmartScreen para ayudar a proteger contra contenido y descargas malintencionados en sitios web que carguen los exploradores de Windows y las aplicaciones de la Tienda.|    Activado| Activado (si no hay ningún acceso a Internet, desactiva esta opción).
|Usa la predicción de páginas para mejorar la lectura, acelerar la exploración y mejorar la experiencia global a la hora de usar los exploradores de Windows. Tus datos de exploración se enviarán a Microsoft.| Activado| Desactivado|
|Obtén actualizaciones de Internet y envía actualizaciones a otros equipos a través de Internet para acelerar las aplicaciones y las descargas de Windows Update.|   Activado| Desactivado|

Una vez completada la instalación, puedes continuar ajustando la configuración empezando por **Configuración de Windows**.

## <a name="in-windows-settings"></a>En Configuración de Windows
Para acceder a la configuración de Windows, haz clic en **Inicio** (el icono de Windows de la barra de tareas) y, a continuación, haz clic en el icono de **configuración** (tiene la forma de un icono de engranaje).

### <a name="in-the-system-area-of-windows-settings"></a>En el área "Sistema" de Configuración de Windows
En el área Configuración de Windows, al hacer clic en el icono de **Sistema**, se accede a una serie de ajustes relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso de VDI; estos ajustes son las más importantes:

#### <a name="apps-and-features"></a>Aplicaciones y características

Para quitar una aplicación (y, por tanto, la excluyes de tu imagen de VDI), haz clic en ella y, a continuación, en **Desinstalar**. Si **Desinstalar** está deshabilitado, significa que no puedes quitarla con este método. Es posible que puedas hacerlo con Windows PowerShell o siguiendo estos pasos:
1. Haz clic en **Administrar características opcionales** (justo debajo del encabezado **Aplicaciones y características** en la misma página).
2. Haz clic en la característica opcional y, a continuación, en **Desinstalar**.

Estas son algunas de las características que podría plantearse quitar (si las tienes instaladas):
- **Obtener ayuda**
- **Contenido de prueba de venta en inglés (Estados Unidos)**
- **Contenido de prueba de venta neutro**
- **Asistencia rápida**

#### <a name="default-apps"></a>Aplicaciones predeterminadas

Esta área define la aplicación que se usará de forma predeterminada para determinadas funciones genéricas, como correo electrónico, exploración web y mapas. Si quieres que otra aplicación se use para una función determinada, haz clic en la entrada actual y, a continuación, en la aplicación que quieres que se utilice en la imagen de VDI. Para poder elegir una aplicación que es de Microsoft, debes instalarla antes de ajustar esta configuración.

#### <a name="notifications-and-actions"></a>Notificaciones y acciones

Estos valores recomendados reducirán las notificaciones y la actividad de red en segundo plano en un entorno de VDI:

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Obtener notificaciones de aplicaciones y otros remitentes| Activado| Desactivado|
|Mostrar notificaciones en la pantalla de bloqueo|    Activado| Desactivado|
|Mostrar alarmas, avisos y llamadas de VoIP entrantes en la pantalla de bloqueo|   Activado| Desactivado|
|Obtener trucos, consejos y sugerencias mientras usas Windows|    Activado| Desactivado|


#### <a name="offline-maps"></a>Mapas sin conexión

Esta opción solo es aplicable si está instalada la aplicación Mapas. Su valor predeterminado es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="tablet-mode"></a>Modo tableta

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Cuando inicio sesión|    Usar el modo adecuado para mi hardware|   Usar el modo de escritorio|
|Cuando el dispositivo activa o desactiva automáticamente el modo tableta|    Preguntar siempre antes de cambiar| No preguntarme y no cambiar|
|Ocultar los iconos de aplicación en la barra de tareas en el modo tableta|  Activado| Desactivado|


### <a name="in-the-devices-area-of-windows-settings"></a>En el área "Dispositivos" de Configuración de Windows
En el área Configuración de Windows, al hacer clic en el icono de **Dispositivos**, se accede a una serie de ajustes relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso de VDI; estos ajustes son las más importantes:

#### <a name="autoplay"></a>Reproducción automática

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Usa la reproducción automática para todos los medios y dispositivos.|    Activado| Desactivado|
|Unidad extraíble:|Elegir un valor predeterminado|No realizar ninguna acción|
|Tarjeta de memoria|Elegir un valor predeterminado|No realizar ninguna acción|

### <a name="in-the-personalization-area-of-windows-settings"></a>En el área "Personalización" de Configuración de Windows
En el área Configuración de Windows, al hacer clic en el icono de **Personalización**, se accede a una serie de ajustes relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso de VDI; estos ajustes son las más importantes:

#### <a name="background"></a>Segundo plano
A veces, el fondo negro predeterminado puede provocar que los usuarios crean que el equipo no responde. Cambia el color de fondo para evitar esto. Para ello, realice los pasos siguientes:
1. En el área **Fondo**, haz clic en el menú desplegable.
2. Para cambiar el color de fondo, haz clic en **Color sólido** y, a continuación, en cualquiera de los colores que no sea negro. Como alternativa, podrías hacer clic en **Imagen** y, a continuación, seleccionar una imagen para usarla como fondo.

#### <a name="start"></a>Inicie

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Mostrar sugerencias ocasionalmente en Inicio|    Activado| Desactivado|
|Mostrar las aplicaciones más usadas|Activado|Desactivado|
|Mostrar aplicaciones agregadas recientemente|Activado|Desactivado|
|Mostrar elementos abiertos recientemente en Listas de accesos directos en Inicio o en la barra de tareas|Activado|Desactivado|

#### <a name="taskbar"></a>Barra de tareas
El valor predeterminado es usar botones de barra de tareas grandes (es decir, el valor "Desactivado" en **Usar botones de barra de tareas pequeños**). Esta configuración hace que el elemento Cortana use ocupe un espacio grande en el área de la barra de tareas. Para evitar esto, establece **Usar botones de barra de tareas pequeños** en "Activado". Si prefieres que los elementos de la barra de tareas sigan siendo grandes, pero prefieres que Cortana no ocupe mucho espacio, haz clic en la barra de tareas, elige **Cortana** y, en el menú que aparece, selecciona **Oculto**.

### <a name="in-the-privacy-area-of-windows-settings"></a>En el área "Privacidad" de Configuración de Windows
En el área Configuración de Windows, al hacer clic en el icono de **Privacidad**, se accede a una serie de ajustes relacionados con el sistema. No todos ellos deben ajustarse para optimizar el uso de VDI; estos ajustes son las más importantes:

#### <a name="general"></a>General
Algunos de estos ajustes también se establecen en la ventana "Personalizar configuración", que se describe al principio de este tema.

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Permitir que las aplicaciones usen mi id. de publicidad para experiencias entre aplicaciones (si esto se desactiva, se restablece el id.)|  Activado| Desactivado|
|Dejar que los sitios web ofrezcan contenido relevante a nivel local mediante el acceso a mi lista de idiomas|Activado|Desactivado|
|Permitir que las aplicaciones de mis otros dispositivos abran aplicaciones y continúen con las experiencias en este dispositivo|Activado|Desactivado|

#### <a name="camera"></a>Cámara

El valor predeterminado para "Permitir que las aplicaciones usen la cámara" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.


#### <a name="microphone"></a>Micrófono

El valor predeterminado para "Permitir que las aplicaciones usen el micrófono" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="notifications"></a>Notificaciones

El valor predeterminado para "Permitir que las aplicaciones tengan acceso a mis notificaciones" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="contacts"></a>Contactos

El valor predeterminado para "Permitir que las aplicaciones tengan acceso a los contactos" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="calendar"></a>Calendario

El valor predeterminado para "Permitir que las aplicaciones tengan acceso al calendario" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="call-history"></a>Historial de llamadas

El valor predeterminado para "Permitir que las aplicaciones tengan acceso a mi historial de llamadas" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="email"></a>Correo electrónico

El valor predeterminado para "Permitir que las aplicaciones tengan acceso al correo electrónico, así como el envío de mensajes" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="messaging"></a>Mensajería

El valor predeterminado para "Permitir que las aplicaciones lean o envíen mensajes (texto o MMS)" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="radios"></a>Señales de radio

El valor predeterminado para "Permitir que las aplicaciones controlen las señales de radio" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="other-devices"></a>Otros dispositivos

El valor predeterminado para "Permite que las aplicaciones compartan y sincronicen información con dispositivos inalámbricos que no se tengan que emparejar explícitamente al PC, tableta o teléfono" es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

#### <a name="feedback-and-diagnostics"></a>Comentarios y diagnósticos

El valor predeterminado para "Windows debe solicitar mis comentarios" es **Automáticamente**; para usar entornos de VDI, el valor recomendado es **Nunca**.

#### <a name="background-apps"></a>Aplicaciones en segundo plano
Las aplicaciones de la lista tienen un valor predeterminado de **Activado**, lo que les permite recibir información, enviar notificaciones y actualizarse automáticamente con independencia de si se están utilizando o no. Debe deshabilitar (establecer el valor en **Desactivado**) las aplicaciones que no quieras que se ejecuten en segundo plano en la imagen de VDI.

### <a name="update-and-security"></a>Actualización y seguridad
#### <a name="windows-update"></a>Windows Update
En el área **Configuración de actualización**, haz clic en **Opciones avanzadas** para ajustar estos valores:

|Valor|Valor predeterminado|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Ofrecer actualizaciones para otros productos de Microsoft cuando actualice Windows|    Desactivado|    Seleccionado|
|Aplazar actualizaciones de características|Desactivado|Seleccionado|
|Usar mi información de inicio de sesión para finalizar automáticamente la configuración de mi dispositivo después de una actualización |Desactivado|Depende de la configuración de VDI específica|

En la página **Opciones avanzadas**, haz clic en **Elige el modo en que quieres que se entreguen las actualizaciones** para acceder a la configuración de "Actualizaciones de más de un lugar". El valor predeterminado es **Activado**; para entornos de VDI, el valor recomendado es **Desactivado**.

## <a name="in-control-panel-and-other-system-utilities"></a>En el Panel de Control y otras utilidades del sistema

La configuración de esta sección puede ajustarse si te desplazas por el Panel de Control o abres la utilidad directamente.

> [!NOTE]
> Cualquier configuración que no se mencione expresamente en este tema puede dejarse en sus valores predeterminados (o ajustarla en función de los requisitos y directivas) sin que repercuta en la funcionalidad de VDI.


### <a name="task-scheduler"></a>Programador de tareas
La forma más rápida de abrir el Programador de tareas es pulsar el botón Windows y escribir *programador de tareas* o *taskschd.msc*. En los resultados que aparecen, haz clic en el **Programador de tareas** para abrir la utilidad. En el Programador de tareas, expande **Biblioteca del Programador de tareas**, **Microsoft** y, a continuación, **Windows**. Ahora tienes acceso a la lista de colecciones de tareas. Para cambiar el estado de cada tarea programada, haz clic con el botón derecho en él y, a continuación, haz clic en el estado que quieras (por lo general, **Deshabilitado** para entornos de VDI).

|Colección de tareas|Nombre de la tarea|Estado predeterminado|Estado recomendado para entornos de VDI|
|-------------------|-------------|----------|--------------|
|Programa para la mejora de la experiencia del usuario||||
||Consolidator|Habilitada|Deshabilitada|
||KernelCeipTask|Habilitada|Deshabilitada|
||UsbCeip|Habilitada|Deshabilitada|
|Defrag||||
||ScheduledDefrag|Habilitada|Deshabilitada|
|Ubicación||||
||Notificaciones|Habilitada|Deshabilitada|
||WindowsActionDialog|Habilitada|Deshabilitada|
|Mantenimiento||||
||WinSAT|Habilitada|Deshabilitada|
|Maps||||
||MapsToastTask|Habilitada|Deshabilitada|
||MapsUpdateTask|Habilitada|Deshabilitada|
|Cuentas de banda ancha móvil||||
||Analizador de metadatos MNO|Habilitada|Deshabilitada|
|Diagnóstico de eficiencia energética||||
||Analyze System|Habilitada|Deshabilitada|
|Entorno de recuperación||||
||VerifyWinRE|Habilitada|Deshabilitada|
|Prueba comercial||||
||CleanupOfflineContent|Habilitada|Deshabilitada|
|Shell||||
||FamilySafetyMonitor|Habilitada|Deshabilitada|
||FamilySafetyRefreshTask|Habilitada|Deshabilitada|
|Informe de errores de Windows||||
||QueueReporting|Habilitada|Deshabilitada|
|Uso compartido de elementos multimedia de Windows||||
||UpdateLibrary|Habilitada|Deshabilitada|

Haz clic en **Windows** otra vez para contraerlo y, a continuación, haz clic en **XblGameSave**. De este modo, podrá acceder a las tareas **XBLGameSaveTask** y **XBLGameSaveTaskLogon**; ambas se pueden establecer en **Deshabilitado**.

### <a name="performance-monitor"></a>Monitor de rendimiento
La forma más rápida de abrir el Monitor de rendimiento es pulsar el botón Windows y escribir *monitor de rendimiento* o *perfmon.msc*. En los resultados que aparecen, haz clic en **Monitor de rendimiento**. En el Monitor de rendimiento, haz clic en **Conjuntos de recopiladores de datos** y, a continuación, haz doble clic en **Sesiones de seguimiento de eventos**. Haz clic con el botón secundario en **WiFiSession**. Si se encuentra en el estado predeterminado de **Ejecutando**, haz clic en **Detener**.

Haz clic en **StartupEventTraceSessions** y, a continuación, en **ReadyBoot**. Si se está ejecutando, haz clic en **Detener**. Haz clic en **Sesiones de seguimiento de eventos**, haz clic con el botón derecho en **ReadyBoot** y, a continuación, haz clic en **Propiedades**. En el cuadro de diálogo que se abre, haz clic en la pestaña **Sesión de seguimiento**. Desactiva la casilla de verificación **Habilitado**.

### <a name="services"></a>Servicios
La forma más rápida de administrar servicios es pulsar el botón Windows y escribir *servicios*. En los resultados que aparecen, haz clic en **Servicios**. Los siguientes servicios son buenos candidatos a fin de deshabilitarlos para entornos de VDI. Sin embargo, es posible que debas hacer algunas pruebas para comprobar que no los necesita. Para deshabilitar un servicio, en el complemento **Servicios**, haz clic en el nombre del servicio y, a continuación, en **Propiedades**. En la pestaña **General**, haz clic en el menú desplegable **Tipo de inicio** y, luego, en **Deshabilitado**. Haga clic en **Aceptar**.

- BranchCache
- Optimización de distribución
- Host de servicio de diagnóstico
- Servicio de zona con cobertura inalámbrica móvil de Windows
- Administración de autenticación de Xbox Live
- Partida guardada en Xbox Live
- Servicio de red de Xbox Live

### <a name="file-explorer-options"></a>Opciones del Explorador de archivos
Pulsa el botón Windows y escribe *panel de control*. En los resultados que aparecen, haz clic en **Panel de Control**. En el Panel de Control, haz clic en **Opciones del Explorador de archivos**. En el cuadro de diálogo que se abre, haz clic en la pestaña **Buscar** y, a continuación, en el área **Al buscar en ubicaciones no indizadas**, desactive la casilla de verificación **Incluir directorios del sistema**. Haz clic en **Aceptar** para guardar.

### <a name="flash-settings"></a>Configuración de Flash
Pulsa el botón Windows y escribe *panel de control*. En los resultados que aparecen, haz clic en **Panel de Control**. En el Panel de Control, haz clic en **Flash Player** para abrir el Administrador de configuración de Flash Player. En la pestaña **Almacenamiento**, selecciona el botón de radio para **impedir que ningún sitio almacene información en este equipo**. En el cuadro de diálogo que se abre, haz clic en **Aceptar**.

En la pestaña **Cámara y micrófono**, área **Configuración de cámara y micrófono**, selecciona el botón de radio para **impedir que ningún sitio use la cámara y el micrófono**.

En la pestaña **Reproducir**, área **Redes asistidas por pares**, selecciona el botón de radio para **impedir que ningún sitio use las redes P2P**. Cierra el Administrador de configuración de Flash Player.

### <a name="internet-options"></a>Opciones de Internet
Pulsa el botón Windows y escribe *panel de control*. En los resultados que aparecen, haz clic en **Panel de Control**. En el Panel de Control, haz clic en **Opciones de Internet** para abrir las propiedades de Internet. En el área **Página principal**, escribe la dirección URL del sitio web que quieres que los usuarios vean como página principal en los exploradores. Podría tratarse de un sitio web de tu empresa o puedes establecerlo en una página de inicio en blanco escribiendo *about:blank*.

En el área **Historial de exploración**, selecciona la casilla de verificación **Eliminar el historial de exploración al salir**.

### <a name="power-options"></a>Opciones de energía
Pulsa el botón Windows y escribe *panel de control*. En los resultados que aparecen, haz clic en **Panel de Control**. En el Panel de Control, haz clic en **Opciones de energía** para abrir el panel de control Opciones de energía. En el área **Elegir o personalizar un plan de energía**, haz clic en la flecha hacia abajo para **mostrar planes adicionales** y, a continuación, selecciona el botón de radio de **Alto rendimiento**. Esta configuración tendrá muy poco impacto en el host de VDI.

### <a name="system"></a>System
Pulsa el botón Windows y escribe *panel de control*. En los resultados que aparecen, haz clic en **Panel de Control**. En el Panel de Control, haz clic en **Sistema** para abrir el panel de control del sistema. En el panel izquierdo, haz clic en **Configuración avanzada del sistema**. En el cuadro de diálogo que se abre, haz clic en la pestaña **Opciones avanzadas**. En el área **Rendimiento**, haz clic en el botón **Configuración** y, a continuación, en **Efectos visuales**, en el cuadro de diálogo que se abre, selecciona el botón de radio de **Ajustar para obtener el mejor rendimiento**. Haz clic en **Aceptar** para guardar y salir.

## <a name="group-policy-settings"></a>Configuración de directiva de grupo

Para editar la configuración de directiva de grupo, pulsa el botón Windows y escribe *directiva de grupo* o *gpedit.msc*. En los resultados que aparecen, haz clic en **Editar directiva de grupo** para abrir el Editor de directivas de grupo local.

> [!NOTE]
> Cualquier configuración que no se mencione expresamente en este tema puede dejarse en sus valores predeterminados (o ajustarla en función de los requisitos y directivas) sin que repercuta en la funcionalidad de VDI.

En **Configuración del equipo**, expande **Configuración de Windows** y, a continuación, **Configuración de seguridad**. Haz clic en **Directivas de Administrador de listas de redes** y, a continuación, haz doble clic en **Todas las redes**. En el cuadro de diálogo que se abre, en el área **Ubicación de red**, selecciona el botón de radio **El usuario no puede cambiar la ubicación**. Haz clic en el botón **Aceptar** para guardar.

Contrae **Configuración de Windows** y, a continuación, expande **Plantillas administrativas**. Haz clic o expande **Red** y, a continuación, ajusta cada configuración como se indica a continuación haciendo doble clic en ella. A continuación, selecciona el botón de radio del valor indicado y haz clic en el botón **Aceptar**:

|Área Configuración|Valor|Valor recomendado para entornos de VDI|
|-------------------|-------|----------|
|Servicio de transferencia inteligente en segundo plano (BITS)|||
||No permitir que el cliente de BITS use Windows BranchCache|Habilitada|
||No permitir que el equipo actúe como un cliente de almacenamiento en caché de sistemas de mismo nivel de BITS|Habilitada|
||No permitir que el equipo actúe como un servidor de almacenamiento en caché de sistemas de mismo nivel de BITS|Habilitada|
||Permitir el almacenamiento en caché de sistemas de mismo nivel de BITS|Deshabilitada|
|BranchCache||
||Activar BranchCache|Deshabilitada|
|Autenticación de zona con cobertura inalámbrica||
||Habilitar la autenticación de zona con cobertura inalámbrica|Deshabilitada|
|Servicios de redes de igual a igual de Microsoft||
||Desactivar los Servicios de redes de igual a igual de Microsoft|Habilitada|
|Archivos sin conexión||
||Permitir o denegar el uso de la característica Archivos sin conexión|Deshabilitada|

Contrae **Red** y, a continuación, expande **Sistema**. Ajusta cada configuración como se indica a continuación haciendo doble clic en ella. A continuación, selecciona el botón de radio del valor indicado y haz clic en el botón **Aceptar**:

|Área Configuración|Valor|Valor recomendado para entornos de VDI|
|-------------------|----------|--------------|
|Instalación de dispositivos||
||No enviar un informe de error de Windows cuando se instale un controlador genérico en un dispositivo|Habilitada|
||Impedir la creación de un punto de restauración del sistema cuando se produzca una actividad de dispositivo que normalmente pediría la creación de un punto de restauración|Habilitada|
||Impedir la recuperación de metadatos de dispositivo desde Internet|Habilitada|
||Impedir que Windows envíe un informe de error cuando un controlador de dispositivo solicite software adicional durante su instalación|Habilitada|
||Desactivar los globos de "Nuevo hardware encontrado" durante la instalación de los dispositivos|Habilitada|

Expande **Sistema de archivos**, haz doble clic en **NTFS**, haz doble clic en **Opciones de creación de nombre corto**, selecciona el botón de radio de **Habilitado** y, luego, usa el menú desplegable**Opciones** para seleccionar **Habilitar en todos los volúmenes**. Haz clic en el botón **Aceptar** para guardar.

Contrae **Sistema de archivos** y, a continuación, expande **Administración de comunicaciones de Internet**. Haz clic en **Configuración de comunicaciones de Internet**. Ajusta cada configuración como se indica a continuación haciendo doble clic en ella. A continuación, selecciona el botón de radio de **Habilitado** y haz clic en el botón **Aceptar**:

- Desactivar los vínculos "Events.asp" del Visor de eventos
- Desactivar uso compartido de datos de personalización de escritura a mano
- Desactivar informe de errores de reconocimiento de escritura a mano
- Desactivar el contenido "¿Sabía que...?" del Centro de ayuda y soporte técnico contenido
- Desactivar la búsqueda en Microsoft Knowledge Base del Centro de ayuda y soporte técnico
- Desactivar al Asistente para la conexión a Internet si la conexión de direcciones URL hace referencia a Microsoft.com
- Desactivar descargas de Internet de los asistentes para la publicación en Web y pedidos en línea
- Desactivar el servicio de asociaciones de archivo de Internet
- Desactivar el registro si la conexión de direcciones URL hace referencia a Microsoft.com
- Desactivar la tarea de imágenes "Pedir copias fotográficas"
- Desactivar la tarea "Publicar en Web" para archivos y carpetas
- Desactivar el Programa para la mejora de la experiencia del usuario de Windows Messenger
- Desactivar el Programa para la mejora de la experiencia del usuario de Windows
- Desactivar el informe de errores de Windows
- Desactivar la búsqueda de controladores de dispositivo en Windows Update

Haz clic en **Administración de energía** y, a continuación, haz doble clic en **Seleccionar un plan de energía activo**. Selecciona el botón de radio de **Habilitado** y, luego, usa el menú desplegable **Opciones** para seleccionar **Alto rendimiento**. Haz clic en el botón **Aceptar** para guardar.

Haz clic en **Recuperación** y, a continuación, haz doble clic en **Permitir la recuperación del sistema a su estado predeterminado**. Selecciona el botón de radio de **Habilitado** y, a continuación, haz clic en el botón **Aceptar** para guardar.

Expande **Solución de problemas y diagnósticos**. Haz doble clic en **Mantenimiento programado**, en **Configurar el comportamiento de mantenimiento programado** y, a continuación, selecciona el botón de radio de **Deshabilitado**. Haz clic en el botón **Aceptar** para guardar.

Haz clic en cada una de las siguientes áreas de configuración y, a continuación, haz doble clic en **Configurar nivel de ejecución de escenario**, selecciona el botón de radio de **Deshabilitado** y, a continuación, haz clic en el botón **Aceptar** para guardar:

- Diagnósticos de rendimiento del arranque de Windows
- Diagnóstico de pérdida de memoria de Windows
- Detección y resolución de agotamiento de recursos de Windows
- Diagnóstico de rendimiento de apagado de Windows
- Diagnóstico de rendimiento de espera/reanudación de Windows
- Diagnóstico de rendimiento de respuesta del sistema de Windows

Contrae **Sistema** y, a continuación, expande **Componentes de Windows**. Ajusta cada configuración como se indica a continuación haciendo doble clic en ella. A continuación, selecciona el botón de radio del valor indicado y haz clic en el botón **Aceptar**:

|Área Configuración|Valor|Valor recomendado para entornos de VDI|
|-------------------|-------|----------|
|Agregar características a Windows 10|||
||Impedir que el asistente se ejecute|Habilitada|
|Directivas de Reproducción automática|||
||Establece el comportamiento predeterminado para ejecución automática.|Habilitado; luego usa el menú desplegable **Opciones** para seleccionar **No ejecutar ningún comando de ejecución automática**.|
|Contenido de la nube|||
||No mostrar sugerencias de Windows|Habilitada|
||Desactivar las experiencias del consumidor de Microsoft|Habilitada|
|Recopilación de datos y versiones preliminares|||
||Permitir telemetría|Habilitado; luego usa el menú desplegable **Opciones** para seleccionar **1- Básico**.|
||Deshabilitar las características o configuración de versión preliminar|     Deshabilitada|
||No mostrar notificaciones de comentarios|       Habilitada|
||Alternar control de usuario para compilaciones de Insider|      Deshabilitada|
|Administrador de ventanas de escritorio|||
||No permitir la invocación de Flip 3D|       Habilitada|
||No permitir animaciones de ventanas|       Habilitada|
||Usar un color sólido para el fondo de Inicio|     Habilitada|
|Interfaz del usuario perimetral|||
||Permitir deslizamiento rápido desde borde|     Deshabilitada|
||Deshabilitar sugerencias de ayuda|        Habilitada|
|Explorador de archivos|||
||No mostrar la notificación de nueva aplicación instalada|     Habilitada|
|Explorador de juegos|||
||Desactivar la descarga de información sobre juegos|     Habilitada|
||Desactivar la actualización de juegos|        Habilitada|
||Desactivar el seguimiento del último tiempo de juego en la carpeta Juegos|     Habilitada|
|Grupo en el hogar|||
||Impedir que el equipo se una a un grupo en el hogar|        Habilitada|
|Internet Explorer|||
||Permitir que los servicios Microsoft proporcionen sugerencias mejoradas mientras el usuario escribe en la barra de direcciones|        Deshabilitada|
||Deshabilitar la comprobación periódica de actualizaciones de software de Internet Explorer|        Habilitada|
||Deshabilitar pantalla de presentación|        Habilitada|
||Instalar nuevas versiones de Internet Explorer automáticamente|      Deshabilitada|
||Impedir la participación en el Programa para la mejora de la experiencia del usuario|     Habilitada|
||Impedir que se ejecute el Asistente para la primera ejecución  Ir directamente a la página principal|   Habilitado; luego usa el menú desplegable **Opciones** para seleccionar **Ir directamente a la página principal**.|
||Establecer el crecimiento del proceso de pestañas|Cuando lo habilites, escribe lo siguiente en el cuadro **Crecimiento del proceso de pestañas**: *Baja*.|
||Especificar el comportamiento predeterminado para una nueva pestaña|Habilitado, luego usa el menú desplegable **Opciones** para seleccionar **Página de la nueva pestaña**.|
||Desactivar las notificaciones de rendimiento de complementos|        Habilitada|
||Desactivar geoubicación del explorador|     Habilitada|
||Desactivar Volver a abrir la última sesión de Exploración|        Habilitada|
||Desactivar sugerencias para todos los proveedores instalados por el usuario|        Habilitada|
||Activar Sitios sugeridos|       Deshabilitada|

En el mismo nivel que la configuración de **Internet Explorer** que acaba de ajustar en la tabla anterior, ten en cuenta otro nivel de carpetas que abarcan desde **Aceleradores** hasta **Barras de herramientas**. Es decir, ahora estás en Directiva de equipo local > Configuración del equipo > Plantillas administrativas > Componentes de Windows > Internet Explorer.

Abre la carpeta **Eliminar el historial de exploración**, haz doble clic en **Permitir que se elimine el historial de exploración al salir**, selecciona **Habilitar** y, a continuación, haz clic en **Aceptar**para guardar y salir.

Usa la flecha Atrás de la esquina superior izquierda del Editor de directivas de grupo local para volver al nivel de **Internet Explorer**. Haz doble clic en **Configuración de Internet**, en **Configuración avanzada** y, a continuación, ajusta la configuración en las subcarpetas del siguiente modo:

|Carpeta de configuración en **Configuración avanzada**|Valor|Valor recomendado para entornos de VDI|
|-------------------|-------|----------|
|**Exploración**|||
||Desactivar detección de número de teléfono|Habilitada|
|**Multimedia**|||
||Permitir que Internet Explorer reproduzca archivos multimedia que usan códecs alternativos|Deshabilitada|

Vuelve al nivel de **Internet Explorer** y, a continuación, haz doble clic en **Configuración de Internet**. En esta carpeta, establece estos dos valores de **Autocompletar** en **Habilitado**:

- Desactivar las sugerencias de URL
- Desactivar Autocompletar búsqueda de Windows

Vuelve cuatro niveles hasta **Componentes de Windows**, haz doble clic en **Ubicación y sensores** y, a continuación, establece estas tres opciones en **Habilitado** (para cada una, haz clic en **Aceptar** para guardar y salir):

- Desactivar la ubicación
- Desactivar scripting de ubicación
- Desactivar sensores

En el nivel de **Ubicación y sensores**, haz doble clic en **Servicio de localización de Windows** y establece **Desactivar el Servicio de localización de Windows** en **Habilitado**. Haz clic en **Aceptar** para guardar y salir.

En el panel izquierdo, haz clic en **Mapas**, establece estos ajustes en **Habilitado**. Para cada uno, haz clic en **Aceptar** para guardar y salir:

- Desactivar la descarga y actualización automáticas de los datos de mapa
- Desactivar el tráfico de red no solicitado en la página Configuración de mapas sin conexión

Mediante el panel izquierdo, accede a cada una de las siguientes subcarpetas de configuración y ajusta los valores individuales como sigue:

|Carpeta de configuración en **Componentes de Windows**|Valor|Valor recomendado para entornos de VDI|
|-------------------|-------|----------|
|**OneDrive**|||
||Impedir el uso de OneDrive para almacenar archivos|Habilitada|
||Guardar documentos en OneDrive de forma predeterminada|Deshabilitada|
|**Fuentes RSS**|||
||Impedir detección automática de fuentes y Web Slices|Habilitada|
|**Búsqueda**|||
||Permitir Cortana|        Deshabilitada|
||Permitir usar Cortana sobre la pantalla de bloqueo|      Deshabilitada|
||Permitir el uso de la ubicación para las búsquedas y Cortana|     Deshabilitada|
||No permitir búsquedas en la Web|      Habilitada|
||No buscar en Internet ni mostrar resultados de Internet en la búsqueda|        Habilitada|
||Impedir la adición de ubicaciones UNC al índice desde el Panel de control|     Habilitada|
||Impedir la indización de archivos en la memoria caché de archivos sin conexión|        Habilitada|
|**Store**|||
||Desactivar la oferta para actualizar a la versión de Windows más reciente|Habilitada|
|**Informe de errores de Windows**|||
||Enviar automáticamente volcados de memoria para informes de errores generados por SO|       Deshabilitada|
||Deshabilitar Informe de errores de Windows|      Habilitada|
|**Windows Installer**|||
||Controlar el tamaño máximo de la memoria caché de los archivos de línea base|  Habilitado; luego usa el cuadro de número del área **Opciones** para establecer **Tamaño máximo de la memoria caché de archivos de línea base** en *5*.|
||Desactivar la creación de puntos de control de Restaurar sistema|      Habilitada|
|**Windows Mail**|||
||Desactivar la característica de comunidades|Habilitada|
|**Reproductor de Windows Media**|||
||No mostrar cuadros de diálogo de primer uso|       Habilitada|
||Impedir compartir medios|        Habilitada|
|**Centro de movilidad de Windows**|||
||Desactivar Centro de movilidad de Windows|Habilitada|
|**Análisis de confiabilidad de Windows**|||
||Configurar proveedores de WMI de confiabilidad|Deshabilitada|
|**Windows Update**|||
||Permitir la instalación inmediata de Actualizaciones automáticas|       Habilitada|
||Quitar el acceso a todas las características de Windows Update|     Habilitada|
|En la carpeta **Windows Update**, abre **Aplazar actualizaciones de Windows**|||
||Selecciona cuándo quieres recibir actualizaciones de características|Habilitado; luego, en el área **Opciones**, usa el menú desplegable **Selecciona el nivel de preparación de rama de las actualizaciones de características que quieres recibir** para seleccionar **Rama actual para empresas**. Establece el cuadro de número **Después del lanzamiento de una actualización de características, aplazar su recepción por la siguiente cantidad de días** en *180 días*.
||Selecciona cuándo quieres recibir actualizaciones de calidad|Habilitado; luego, en el área **Opciones**, establece el cuadro de número **Después del lanzamiento de una actualización de calidad, aplazar su recepción por la siguiente cantidad de días** en *30 días* y selecciona la casilla de verificación **Pausar actualizaciones de calidad**.

En el panel izquierdo del Editor de directivas de grupo local, haz clic en **Configuración de usuario**. Mediante el panel izquierdo, haz clic en **Plantillas administrativas** y, a continuación, accede a cada una de las siguientes subcarpetas de configuración y ajusta los valores individuales como sigue:

|Carpeta de configuración en **Plantillas administrativas**|Valor|Valor recomendado para entornos de VDI|
|-------------------|-------|----------|
|**Dispositivo de escritorio**|||
||No agregar recursos compartidos de documentos abiertos recientemente a Ubicaciones de red|Habilitada|
|En la carpeta **Escritorio**, abre **Active Directory**.|||
||Tamaño máximo de las búsquedas en Active Directory|Habilitado; luego, en el área **Opciones**, usa el cuadro de número para establecer **Número de objetos obtenidos** en *5000*.|
|**Menú Inicio y barra de tareas**|||
||Borrar lista de programas recientes para nuevos usuarios|     Habilitada|
||No mostrar elementos, ni realizar seguimiento de los mismos, en las Jump Lists desde ubicaciones remotas|        Habilitada|
||Desactivar globos de anuncios de características|     Habilitada|
||Desactivar seguimiento del usuario|       Habilitada|
|En la carpeta **Menú Inicio y barra de tareas**, abre **Notificaciones**|||
||Desactivar las notificaciones del sistema|Habilitada|
|En la carpeta **Componentes de Windows**, abre:|||
|**Contenido de la nube**|||
||Desactivar todas las características de Contenido destacado de Windows|Habilitada|
|**Explorador de archivos**|||
||Desactivar el almacenamiento en caché de imágenes en miniatura|       Habilitada|
||Desactivar la visualización de las entradas de búsqueda recientes en el cuadro de búsqueda del Explorador de archivos|        Habilitada|
||Desactivar almacenamiento en caché de vistas en miniatura en archivos thumbs.db ocultos|      Habilitada|

## <a name="microsoft-store-apps"></a>Aplicaciones de Microsoft Store
Hay una serie de aplicaciones de Microsoft Store que puede que quieras quitar de la imagen de VDI. Al eliminarlas, se reduce el uso de CPU y se conserva espacio en disco. Son buenas candidatas para su eliminación las siguientes:

- Obtener Office
- Skype (versión preliminar)
- Introducción (sobre todo si no hay ninguna conexión a Internet)
- Centro de opiniones
- Microsoft Solitaire Collection
- Wi-Fi y datos móviles de pago

Con el fin de personalizar el perfil de usuario predeterminado utilizado para crear imágenes VDI, utiliza la cuenta predefinida Administrador. Si aún no está habilitado, puedes hacerlo mediante el uso de los usuarios y grupos locales en Administración de equipos. A continuación, inicia sesión en la cuenta Administrador para completar los pasos siguientes.

> [!NOTE]
> No quites las aplicaciones del sistema, como la app Store, ya que son difíciles de volver a instalar. Otras aplicaciones pueden volver a instalarse fácilmente desde Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Eliminación de aplicaciones no deseadas desde el perfil de usuario Administrador
1. En Windows PowerShell, ejecuta `Get-AppxPackage | ft PackageFamilyName` para ver la lista de las aplicaciones instaladas.
2. Para cada empaquetador de aplicaciones que quieras desinstalar, ejecuta cmdlets de este formato de ejemplo:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Eliminación de la carga de aplicaciones no deseadas de Store
Esto impedirá que las aplicaciones vuelvan a instalarse.
1. Obtén una lista de las aplicaciones de Store y otros elementos que tienen datos aprovisionados en el almacenamiento con este cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Quita un paquete determinado con `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, utilizando el paquete MyAppPackage adecuado que se devolvió en el paso 1. Por ejemplo, para quitar el paquete relacionado con Zune, ejecutarías `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Eliminación de otros elementos
Puedes quitar la aplicación y el icono de OneDrive, desactivar los iconos del sistema y eliminar las actualizaciones descargadas.

### <a name="remove-onedrive-icon-and-app"></a>Eliminación de la aplicación y el icono de OneDrive
1. Haz clic en **Inicio** y desplázate hasta el icono de **OneDrive**.
2. Haz clic con el botón derecho en el icono de **OneDrive**, selecciona **Más** y, a continuación, haz clic en **Abrir ubicación del archivo**.
3. Haz clic con el botón derecho en el icono de **OneDrive** en su ubicación de archivo y haz clic en el icono de **Eliminar**.

Para quitar la aplicación OneDrive, sigue estos pasos:
1. Haz clic en **Inicio** y desplázate hasta el icono de **OneDrive**.
2. Haz clic con el botón derecho en el icono de **OneDrive** y, a continuación, haz clic en **Desinstalar**. Se abrirá Programas y características.
3. En Programas y características, haz clic con el botón derecho en **Microsoft OneDrive** y haz clic en **Desinstalar**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programas y características (desde versiones anteriores del Panel de Control)
1. Pulsa el botón **iniciar**, escribe *Control* y, a continuación, presiona INTRO.
2. Pulsa o haz doble clic en **Programas y características**.
3. En el extremo izquierdo, en **Ventana principal del Panel de control**, pulsa o haz clic en **Activar o desactivar las características de Windows**. Se abrirá una nueva interfaz de usuario.
4. Desactiva las casillas de verificación de todos los elementos que no quieras o necesites en la imagen base, por ejemplo: **Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS**

### <a name="turn-system-icons-off"></a>Desactivación de los iconos del sistema
1. Pulsa o haz clic en **Inicio** y, a continuación, haz clic en **Configuración** (el icono de engranaje).
2. En el área de texto **Buscar una configuración**, escribe *barra de tareas* y, a continuación, haz clic en **Configuración de la tabla de tareas**.
3. En la sección **Barra de tareas**, desplázate o desliza el dedo hacia abajo hasta la sección **Área de notificación**.
4. Haz clic o pulsa **Activar o desactivar iconos del sistema** y, a continuación, activa o desactiva cada icono del sistema según quieras que esté o no en la imagen.

### <a name="delete-downloaded-updates"></a>Eliminación de las actualizaciones descargadas
1. Mediante el Explorador de archivos, ve a **C:\Windows\Software Distribution\Download**.
2. Elimina todos los archivos y carpetas de ese directorio.
