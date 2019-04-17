---
title: "Cómo funciona el Control de cuentas de usuario"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7f5ad234959ebfe0687b8bc10f9e43f2092252f2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="how-user-account-control-works"></a>Cómo funciona el Control de cuentas de usuario

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Control de cuentas de usuario (UAC) ayuda a evitar que programas malintencionados (también denominados malware) dañe un equipo y ayuda a las organizaciones a implementar un escritorio mejor administrado. Con UAC, aplicaciones y tareas siempre se ejecutan en el contexto de seguridad de una cuenta de administrador, a menos que un administrador autorice específicamente el acceso de nivel de administrador al sistema. UAC puede bloquear la instalación automática de aplicaciones no autorizadas y evitar cambios inadvertidos en la configuración del sistema.

## <a name="uac-process-and-interactions"></a>UAC proceso e interacciones
Cada aplicación que requiere el token de acceso de administrador debe solicitar al administrador de consentimiento. La única excepción es la relación que existe entre primarios y secundarios procesos. Los procesos secundarios heredan el token de acceso de usuario del proceso primario. Sin embargo, los primario y secundario procesos, deben tener el mismo nivel de integridad. Windows Server 2012 protege los procesos marcando sus niveles de integridad. Niveles de integridad son medidas de confianza. Una aplicación de integridad "alta" es aquella que realiza tareas que modifiquen los datos del sistema, como un disco en la partición de aplicación, mientras que una aplicación de integridad "baja" es aquella que realiza tareas que podrían poner en riesgo el sistema operativo, como un explorador Web. Aplicaciones con niveles de integridad bajos no pueden modificar datos en aplicaciones con niveles de integridad más altos. Cuando un usuario estándar intenta ejecutar una aplicación que requiere un token de acceso de administrador, UAC requiere que el usuario proporcione credenciales de administrador válidas.

Para comprender mejor cómo produce este proceso es importante para revisar los detalles del proceso de inicio de sesión de Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Proceso de inicio de sesión de Windows Server 2012
La siguiente ilustración muestra cómo difiere el proceso de inicio de sesión para que un administrador del proceso de inicio de sesión para un usuario estándar.

![Ilustración que muestra cómo difiere el proceso de inicio de sesión para que un administrador del proceso de inicio de sesión para un usuario estándar](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

De manera predeterminada, los administradores y usuarios estándar acceso a recursos y ejecutan aplicaciones en el contexto de seguridad de los usuarios estándar. Cuando un usuario inicia sesión en un equipo, el sistema crea un token de acceso para ese usuario. El token de acceso contiene información sobre el nivel de acceso que se concede al usuario, incluidos los identificadores específicos de seguridad (SID) y privilegios de Windows.

Cuando un administrador inicia sesión, se crean dos tokens de acceso independientes para el usuario: un token de acceso de usuario estándar y un token de acceso de administrador. El token de acceso de usuario estándar contiene la misma información específica del usuario que el token de acceso de administrador, pero se quitan el SID y privilegios administrativos de Windows. El token de acceso de usuario estándar se usa para iniciar las aplicaciones que no realizan tareas administrativas (aplicaciones de usuario estándar). El token de acceso de usuario estándar, a continuación, se usa para mostrar el escritorio (Explorer.exe). Explorer.exe es el proceso principal de la que otros procesos iniciados por el usuario heredan el token de acceso. Como resultado, todas las aplicaciones se ejecutan como un usuario estándar a menos que un usuario da su consentimiento o credenciales para autorizar a una aplicación para usar un token de acceso administrativo total.

Un usuario que sea miembro del grupo Administradores puede iniciar sesión, navegar por la Web y consultar el correo electrónico con un token de acceso de usuario estándar. Cuando el administrador necesita realizar una tarea que requiere el token de acceso de administrador, Windows Server 2012 automáticamente pide al usuario su aprobación. Este mensaje se denomina una petición de elevación, y su comportamiento puede configurarse mediante el complemento Directiva de seguridad Local (Secpol.msc) o la directiva de grupo.

> [!NOTE]
> El término "elevar" se usa para hacer referencia al proceso en Windows Server 2012 que pide al usuario su consentimiento o credenciales para usar un token de acceso de administrador total.

### <a name="the-uac-user-experience"></a>La experiencia de usuario UAC
Cuando el UAC está habilitado, la experiencia del usuario para los usuarios estándar es diferente de los administradores en modo de aprobación de administrador. El método recomendado y más seguro para ejecutar Windows Server 2012 es que el usuario principal cuenta de usuario estándar. Se ejecuta como un usuario estándar ayuda a maximizar la seguridad de un entorno administrado. Con el componente de elevación de UAC integrado, los usuarios estándar pueden realizar fácilmente una tarea administrativa escribiendo las credenciales válidas para una cuenta de administrador local. El valor predeterminado, el componente de elevación de UAC integrado para los usuarios estándar es la petición de credenciales.

La alternativa a ejecutar como usuario estándar es ejecutar como administrador en el modo de aprobación de administrador. Con el componente de elevación de UAC integrado, los miembros del grupo de administradores local pueden realizar fácilmente una tarea administrativa proporcionando aprobación. El valor predeterminado, el componente de elevación de UAC integrado para una cuenta de administrador en el modo de aprobación de administrador se llama la petición de consentimiento. El comportamiento de solicitud de elevación de UAC puede configurarse mediante el complemento Directiva de seguridad Local (Secpol.msc) o la directiva de grupo.

**Las peticiones de consentimiento y credenciales**

Con UAC habilitado, Windows Server 2012 solicita consentimiento o solicita credenciales de una cuenta de administrador local válida antes de iniciar el programa o la tarea que requiere un token de acceso de administrador total. Este mensaje garantiza que ningún software malintencionado puede instalarse de forma silenciosa.

**La petición de consentimiento**

La petición de consentimiento se presenta cuando un usuario intenta realizar una tarea que requiere el token de acceso administrativo de un usuario. El siguiente es una captura de pantalla de la petición de consentimiento UAC.

![Captura de pantalla de la petición de consentimiento UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**La petición de credenciales**

La petición de credenciales se presenta cuando un usuario estándar intenta realizar una tarea que requiere el token de acceso administrativo de un usuario. Este comportamiento de solicitud de usuario estándar predeterminado puede configurarse mediante el complemento Directiva de seguridad Local (Secpol.msc) o la directiva de grupo. Los administradores también pueden tener que proporcionar sus credenciales estableciendo el Control de cuentas de usuario: comportamiento de la petición de elevación para los administradores en modo de aprobación de administrador configuración de directiva valor a petición para las credenciales.

La siguiente captura de pantalla es un ejemplo de la petición de credenciales UAC.

![Captura de pantalla que muestra un ejemplo de la petición de credenciales UAC](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Peticiones de elevación del UAC**

Las peticiones de elevación del UAC están codificadas con colores para ser específica de la aplicación, lo que permite la identificación inmediata del posible riesgo de seguridad de la aplicación. Cuando una aplicación intenta ejecutar con el token de acceso completa del administrador, Windows Server 2012 analiza primero el archivo ejecutable para determinar su editor. Las aplicaciones en primer lugar se dividen en tres categorías en función de ejecutable del Editor del archivo: Windows Server 2012, comprobado el Editor (firmada) y editor sin comprobar (sin firmar). El siguiente diagrama ilustra cómo Windows Server 2012 determina qué petición de elevación de color presentar al usuario.

La elevación pedir códigos de colores son la siguiente:

-   Fondo rojo con un icono de escudo rojo: la aplicación está bloqueada por la directiva de grupo o por un editor que está bloqueado.

-   Fondo azul con un icono de escudo azul y dorado: la aplicación es una aplicación de administración de Windows Server 2012, como un elemento del Panel de Control.

-   Fondo azul con un icono de escudo azul: la aplicación está firmada con Authenticode y es de confianza para el equipo local.

-   Fondo amarillo con un icono de escudo amarillo: la aplicación está sin firmar o firmada, pero aún no confía en el equipo local.

**Icono de escudo**

Panel de Control de algunos elementos, como **propiedades de fecha y hora**, contienen una combinación de administrador y las operaciones de usuario estándar. Los usuarios estándar pueden ver el reloj y cambiar la zona horaria, pero es necesario un token de acceso de administrador total para cambiar la hora del sistema local. El siguiente es una captura de pantalla de la **propiedades de fecha y hora** elemento del Panel de Control.

![Pantalla que muestra la ** elemento fecha y hora propiedades ** Panel de Control](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

El icono de escudo del **Cambiar fecha y hora** botón indica que el proceso requiere un token de acceso de administrador total y mostrará una petición de elevación de UAC.

**Protección de la petición de elevación**

El proceso de elevación se protege aún más dirigiendo el símbolo del sistema al escritorio seguro. Las peticiones de consentimiento y credenciales se muestran en el escritorio seguro de forma predeterminada en Windows Server 2012. Solo los procesos de Windows pueden acceder al escritorio seguro. Para un mayor nivel de seguridad, se recomienda mantener la **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación** configuración de directiva habilitada.

Cuando un archivo ejecutable solicita la elevación, el escritorio interactivo, también llamado el escritorio del usuario, se cambia al escritorio seguro. El escritorio seguro atenúa el escritorio del usuario y muestra una petición de elevación que se debe responder antes de continuar. Cuando el usuario hace clic en Sí o No, el escritorio vuelve a cambiar a escritorio del usuario.

El malware puede presentar una imitación del escritorio seguro, pero cuando el Control de la cuenta de usuario: comportamiento de la petición de elevación para los administradores en configuración de directiva de modo de aprobación de administrador se establece como la petición de consentimiento, el malware no obtendrá la elevación si el usuario hace clic en Sí en la imitación. Si la configuración de directiva se establece a petición de credenciales, el malware que imita la petición de credenciales puede recopilar las credenciales del usuario. Sin embargo, el malware no obtiene privilegios elevados y el sistema tiene otras protecciones que mitigar el malware tome el control de la interfaz de usuario incluso con una contraseña obtenida.

Mientras el malware podría presentar una imitación del escritorio seguro, este problema no se puede producir a menos que un usuario instalara previamente el malware en el equipo. Porque los procesos que requieren un token de acceso de administrador no se pueden instalar de forma silenciosa cuando el UAC está habilitado, el usuario debe dar su consentimiento explícito haciendo clic en **Sí** o proporcionando credenciales de administrador. El comportamiento específico de la petición de elevación del UAC depende de la directiva de grupo.

## <a name="uac-architecture"></a>Arquitectura del UAC
El siguiente diagrama detalla la arquitectura del UAC.

![Diagrama que detalla la arquitectura del UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Para comprender mejor cada componente, revisa la tabla siguiente:

|Componente|Descripción|
|-------|--------|
|**Usuario**||
|Usuario realiza la operación que requieren privilegios|Si la operación cambia el registro o el sistema de archivos, llama a la virtualización. Todas las demás operaciones llaman a ShellExecute.|
|ShellExecute|ShellExecute llama a CreateProcess. ShellExecute busca el error ERROR_ELEVATION_REQUIRED de CreateProcess. Si recibe el error, ShellExecute llama al servicio de información de la aplicación para tratar de realizar la tarea solicitada con el símbolo del sistema con privilegios elevados.|
|CreateProcess|Si la aplicación requiere elevación, CreateProcess rechaza la llamada con ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Servicio de información de la aplicación|Un servicio del sistema que ayuda a las aplicaciones de inicio que requieren privilegios elevados o derechos de usuario para ejecutarse, como las tareas administrativas locales y aplicaciones que requieren niveles de integridad más altos. El usuario da consentimiento de estas aplicaciones creando un nuevo proceso para la aplicación con el token de acceso total administrativo de un usuario cuando se requiere elevación y (según la directiva de grupo) el inicio de ayuda a del servicio de información de la aplicación para hacerlo.|
|Elevar una instalación de ActiveX|Si ActiveX no está instalado, el sistema comprueba el nivel de control deslizante UAC. Si ActiveX está instalado, el **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación** se comprueba la configuración de directiva de grupo.|
|Comprobar el nivel de control deslizante del UAC|Ahora, UAC tiene cuatro niveles de notificación para elegir y un control deslizante para seleccionar el nivel de notificación:<br /><br /><ul><li>Alto<br /><br />    Si el control deslizante está establecido en **Notificarme siempre**, el sistema comprueba si está habilitado el escritorio seguro.</li><li>Mediano<br /><br />    Si el control deslizante está establecido en **notificar predeterminado me solo cuando un programa intente realizar cambios en el equipo**, la **Control de cuentas de usuario: elevar solo los archivos ejecutables que están firmados y validados** se comprueba la configuración de directiva:<br /><br /><ul><li>Si se habilita la configuración de directiva, se aplica la validación de rutas de certificación de la infraestructura de clave pública (PKI) para un determinado archivo ejecutable antes de permitir su ejecución.</li><li>Si la configuración de directiva no está habilitado (predeterminado), no se aplica la validación de rutas de certificación de PKI antes de que un determinado archivo ejecutable tiene permiso para ejecutarse. La **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación** se comprueba la configuración de directiva de grupo.</li></ul></li><li>Baja<br /><br />    Si el control deslizante está establecido en **Notificarme solo cuando el programa intente realizar cambios en el equipo (no atenuar el escritorio)**, se llama a CreateProcess.</li><li>No notificarme nunca<br /><br />    Si el control deslizante está establecido en **no notificarme nunca cuando**, will símbolo del sistema del UAC no notificarme nunca cuando un programa está tratando de instalar o tratando de hacer algún cambio en el equipo. **Importante:** esta configuración no se recomienda. Esta configuración es la misma que la **Control de cuentas de usuario: comportamiento de la petición de elevación para los administradores en modo de aprobación de administrador** configuración de directiva **elevar sin preguntar**.</li></ul>|
|Escritorio seguro habilitado|La **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación** se comprueba la configuración de directiva:<br /><br />-Si el escritorio seguro está habilitado, todas las solicitudes de elevación van al escritorio seguro independientemente de la configuración de directiva de comportamiento de solicitud para administradores y usuarios estándar.<br />-Si el escritorio seguro no está habilitado, todas las solicitudes de elevación van al escritorio del usuario interactivo y se usa la configuración de cada usuario para los administradores y usuarios estándar.|
|CreateProcess|CreateProcess llama a la detección de AppCompat, Fusion y del instalador para evaluar si la aplicación requiere elevación. A continuación, se inspeccionará el archivo ejecutable para determinar su nivel de ejecución solicitado, que se almacena en el manifiesto de la aplicación para el archivo ejecutable. CreateProcess genera un error si el nivel de ejecución solicitado especificado en el manifiesto no coincide con el token de acceso y devuelve un error (ERROR_ELEVATION_REQUIRED) a ShellExecute. |
|Compatibilidad de aplicaciones|La base de datos de AppCompat almacena información en las entradas de corrección de compatibilidad de aplicaciones para una aplicación.|
|Fusion|La base de datos de Fusion almacena información de los manifiestos de aplicación que describen las aplicaciones. El esquema del manifiesto se actualiza para agregar un nuevo campo de nivel de ejecución solicitado.|
|Detección del instalador|La detección del instalador detecta los archivos ejecutables de la instalación, lo que ayuda a impedir que las instalaciones se ejecuten sin conocimiento y el consentimiento del usuario.|
|**Kernel**||
|Virtualización|La tecnología de virtualización garantiza que las aplicaciones no conformes no den error al ejecutarse ni den error de manera que no se puede determinar la causa. UAC también proporciona virtualización de archivos y del registro y registro de las aplicaciones que escriben en áreas protegidas.|
|Sistema de archivos y del registro|La virtualización de archivos y del registro por usuario redirige el registro por equipo y las solicitudes de escritura de archivo a ubicaciones equivalentes de cada usuario. Las solicitudes de lectura se redirigen a la ubicación por usuario virtualizada segunda primero y a la ubicación de cada equipo.|

Hay un cambio de Windows Server 2012 UAC respecto a versiones anteriores de Windows. El control deslizante nuevo nunca desactivará el UAC completamente. La nueva configuración lo siguiente:

-   Mantener el servicio UAC en ejecución.

-   Causa las solicitudes de elevación todas las iniciadas por los administradores se aprueben automáticamente sin mostrar un UAC símbolo del sistema.

-   Denegar automáticamente todas las solicitudes de elevación para los usuarios estándar.

> [!IMPORTANT]
> Para deshabilitar completamente el UAC debes deshabilitar la directiva **Control de cuentas de usuario: ejecutar todos los administradores en modo de aprobación de administrador**.

> [!WARNING]
> Aplicaciones adaptadas no funcionará en Windows Server 2012 cuando el UAC está deshabilitado.

### <a name="virtualization"></a>Virtualización
Debido a los administradores del sistema en entornos empresariales intentan proteger los sistemas, muchas aplicaciones de línea de negocio (LOB) están diseñadas para usar solo un token de acceso de usuario estándar. Como resultado, los administradores de TI no es necesario reemplazar la mayoría de las aplicaciones cuando se ejecuta Windows Server 2012 con UAC habilitado.

 Windows Server 2012 incluye la tecnología de virtualización de archivos y del registro para las aplicaciones que no son compatibles con UAC y que requieren el token de acceso de administrador para ejecutarse correctamente. Virtualización garantiza que incluso las aplicaciones que no son compatibles con UAC son compatibles con Windows Server 2012. Cuando una aplicación administrativa que no es compatible con UAC intenta escribir en un directorio protegido, como archivos de programa, UAC proporciona su propia vista virtualizada del recurso que está intentando cambiar a la aplicación. La copia virtualizada se mantiene en el perfil del usuario. Esta estrategia crea una copia independiente del archivo virtualizado para cada usuario que se ejecuta la aplicación no compatible.

La mayoría de las tareas de aplicación funcionan correctamente con las características de virtualización. Aunque la virtualización permite que la mayoría de aplicaciones se ejecuten, es una solución a corto plazo y no es una solución a largo plazo. Los desarrolladores de aplicaciones deben modificar sus aplicaciones para ser compatible con el programa del logotipo de Windows Server 2012 tan pronto como sea posible, en lugar de confiar en la virtualización de archivos, carpetas y del registro.

La virtualización no es la opción en los siguientes escenarios:

1.  La virtualización no se aplica a las aplicaciones que son elevadas y que se ejecutan con un token de acceso administrativo total.

2.  Virtualización es compatible con aplicaciones de 32 bits únicamente. Aplicaciones de 64 bits sin privilegios elevados simplemente reciben un mensaje de acceso denegado al intentar adquirir un identificador (un identificador único) a un objeto de Windows. Las aplicaciones nativas de Windows de 64 bits son necesarias para que sean compatibles con UAC y para escribir datos en las ubicaciones correctas.

3.  Virtualización está deshabilitada para una aplicación si la aplicación incluye un manifiesto de la aplicación con un atributo de nivel de ejecución solicitado.

### <a name="request-execution-levels"></a>Solicitar niveles de ejecución
Un manifiesto de aplicación es un archivo XML que describe e identifica los ensamblados en paralelo compartidos y privados que una aplicación se debe enlazar en tiempo de ejecución. En Windows Server 2012, el manifiesto de la aplicación incluye entradas para fines de compatibilidad de aplicación de UAC. Las aplicaciones administrativas que incluyen una entrada en el manifiesto de aplicación pedir al usuario permiso acceder a token de acceso del usuario. Aunque carecen de una entrada en el manifiesto de aplicación, la mayoría de las aplicaciones administrativa puede ejecutar sin modificaciones mediante el uso de correcciones de compatibilidad de la aplicación. Correcciones de compatibilidad de aplicaciones son entradas de base de datos que permiten que las aplicaciones que no son compatibles con UAC funcionen correctamente con Windows Server 2012.

Todas las aplicaciones compatibles con UAC deben tener un nivel de ejecución solicitado agregado al manifiesto de aplicación. Si la aplicación requiere acceso administrativo al sistema, a continuación, marcar la aplicación con una ejecución solicitado de "requiere administrador" garantiza que el sistema identifique este programa como una aplicación administrativa y realiza los pasos necesarios de elevación. Solicitar niveles de especifican los privilegios necesarios para una aplicación de ejecución.

### <a name="installer-detection-technology"></a>Tecnología de detección del instalador
Programas de instalación son aplicaciones diseñadas para implementar software. La mayoría de los programas de instalación se escriben en directorios del sistema y claves del registro. Estas ubicaciones de sistema protegidos son normalmente solo puede escribir un administrador de tecnología de detección del instalador, lo que significa que los usuarios estándar no tienen acceso suficiente para instalar programas. Windows Server 2012 detecta de forma heurística programas de instalación y pide credenciales de administrador o aprobación del usuario administrador para ejecutarse con privilegios de acceso. Windows Server 2012 detecta también forma heurística actualizaciones y los programas que desinstalan aplicaciones. Uno de los objetivos de diseño del UAC es impedir que las instalaciones se ejecuten sin el conocimiento del usuario y da su consentimiento porque los programas de instalación se escriben en áreas protegidas del sistema de archivos y del registro.

La detección del instalador solo se aplica a:

-   archivos ejecutables de 32 bits.

-   Aplicaciones sin un atributo de nivel de ejecución solicitado.

-   Procesos interactivos que se ejecutan como un usuario estándar con UAC habilitado.

Antes de crea un proceso de 32 bits, se comprueban los siguientes atributos para determinar si es un instalador:

-   El nombre de archivo incluye palabras clave como "instalar", "Configurar" o "actualización".

-   Campos de recursos de control de versiones contienen las siguientes palabras clave: proveedor, nombre de la compañía, nombre del producto, descripción del archivo, nombre de archivo Original, nombre interno y nombre de exportación.

-   Palabras clave en el manifiesto en paralelo se incrustan en el archivo ejecutable.

-   Las palabras clave en determinadas entradas de StringTable están vinculadas en el archivo ejecutable.

-   Los atributos clave en los datos de script de recursos están vinculados en el archivo ejecutable.

-   Existen secuencias dirigidas de bytes en el archivo ejecutable.

> [!NOTE]
> Las palabras clave y las secuencias de bytes se derivan de características comunes observadas de distintas tecnologías de instalador.

> [!NOTE]
> El Control de cuentas de usuario: Detectar instalaciones de aplicaciones y pedir para la configuración de directiva de elevación debe estar habilitado para que la detección del instalador detectar los programas de instalación. Esta configuración está habilitada de manera predeterminada y puede configurarse localmente mediante el complemento Directiva de seguridad Local (Secpol.msc) o configurada para el dominio, unidad organizativa o grupos específicos mediante la directiva de grupo (Gpedit.msc).


