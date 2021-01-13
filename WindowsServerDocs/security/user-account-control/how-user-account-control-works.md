---
title: Funcionamiento del Control de cuentas de usuario
description: Obtenga información sobre el control de cuentas de usuario (UAC) y cómo ayuda a evitar que programas malintencionados dañen un equipo y ayuda a las organizaciones a implementar un escritorio mejor administrado.
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 948ef5cc3145f3ecdb35415f77f316ed265b44bf
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177624"
---
# <a name="how-user-account-control-works"></a>Funcionamiento del Control de cuentas de usuario

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El Control de cuentas de usuario (UAC) ayuda a evitar que un programa malintencionado (también conocido como malware) dañe un equipo, al tiempo que contribuye a que las organizaciones implementen un escritorio mejor administrador Con UAC, las aplicaciones y las tareas siempre se ejecutan en el contexto de seguridad de una cuenta que no es de administrador, a menos que un administrador autorice expresamente el nivel de acceso de administrador al sistema. UAC impide la instalación automática de aplicaciones no autorizadas, así como los cambios involuntarios en la configuración del sistema.

## <a name="uac-process-and-interactions"></a>Interacciones y proceso de UAC
Todas las aplicaciones que necesiten el token de acceso de administrador deben pedir su consentimiento al administrador La única excepción se produce en la relación existente entre los procesos primarios y secundarios. Los procesos secundarios heredan el token de acceso de usuario del proceso primario. Sin embargo, ambos procesos deben tener el mismo nivel de integridad. Windows Server 2012 protege los procesos marcando sus niveles de integridad. Los niveles de integridad son medidas de confianza. Así, una aplicación con una integridad "alta" es aquella que realiza tareas que alteran los datos del sistema (como una aplicación para crear particiones de disco), mientras que una con una integridad "baja" lleva a cabo tareas que podrían suponer un riesgo para el sistema operativo (como un explorador web). Las aplicaciones con niveles de integridad más bajos no pueden modificar los datos de las aplicaciones con niveles más elevados. Cuando un usuario estándar trata de ejecutar una aplicación que requiere un token de acceso de administrador, UAC le obliga a proporcionar credenciales de administrador válidas.

Con el fin de comprender mejor cómo se produce este proceso, es importante revisar los detalles del proceso de inicio de sesión de Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Proceso de inicio de sesión de Windows Server 2012
En la siguiente ilustración se indica la diferencia entre el proceso de inicio de sesión de un administrador y el de un usuario estándar.

![Ilustración en la que se muestra cómo el proceso de inicio de sesión de un administrador difiere del proceso de inicio de sesión de un usuario estándar](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

De forma predeterminada, los usuarios estándar y los administradores obtienen acceso a los recursos y ejecutan aplicaciones en el contexto de seguridad de los usuarios estándar. Cuando un usuario inicia sesión en un equipo, el sistema crea un token de acceso para dicho usuario. Este token de acceso contiene información sobre el nivel de acceso concedido al usuario, además de los identificadores de seguridad (SID) y los privilegios de Windows específicos.

Cuando un administrador inicia sesión, se crean dos tokens de acceso distintos: uno de usuario estándar y otro de administrador. El token de acceso de usuario estándar contiene la misma información específica de usuario que el token de acceso de administrador, pero los SID y los privilegios administrativos de Windows se eliminan. Este token sirve para iniciar aplicaciones que no realizan tareas de carácter administrativo (aplicaciones de usuario estándar). Así, por ejemplo, sirve para mostrar el escritorio (Explorer.exe). Explorer.exe es el proceso primario del que todos los demás procesos iniciados por el usuario heredan el token de acceso. En consecuencia, todas las aplicaciones se ejecutan como un usuario estándar, a menos que un usuario dé su consentimiento o suministre las credenciales correspondientes para permitir que una aplicación haga uso del token de acceso administrativo completo.

Un usuario que sea miembro del grupo Administradores puede iniciar sesión, navegar por la Web y consultar el correo electrónico con un token de acceso de usuario estándar. Cuando el administrador necesita realizar una tarea que requiere el token de acceso de administrador, Windows Server 2012 solicita automáticamente al usuario la aprobación. Esta petición se denomina petición de elevación, y su comportamiento se puede configurar mediante el complemento de directiva de seguridad local (Secpol.msc) o la directiva de grupo.

> [!NOTE]
> El término "Elevate" se usa para hacer referencia al proceso en Windows Server 2012 que solicita al usuario el consentimiento o las credenciales para usar un token de acceso de administrador completo.

### <a name="the-uac-user-experience"></a>La experiencia del usuario del UAC
Cuando UAC está habilitado, la experiencia de los usuarios estándar es distinta de la de los administradores en el Modo de aprobación de administrador. El método recomendado y más seguro de ejecutar Windows Server 2012 es convertir la cuenta de usuario principal en una cuenta de usuario estándar. ya que esto ayuda a potenciar la seguridad de un entorno administrado. Con el componente de elevación de UAC integrado, los usuarios estándar pueden realizar tareas administrativas con total facilidad si especifican unas credenciales válidas de una cuenta de administrador local. El componente de elevación de UAC integrado predeterminado de los usuarios estándar es la petición de credenciales.

La alternativa a ejecutarse como un usuario estándar es hacerlo como un administrador en el Modo de aprobación de administrador. Con el componente de elevación de UAC integrado, los miembros del grupo de administradores local pueden realizar fácilmente una tarea administrativa proporcionando aprobación. El componente de elevación de UAC integrado de una cuenta de administrador en el Modo de aprobación de administrador se denomina petición de consentimiento. El comportamiento de la petición de elevación de UAC se puede configurar mediante el complemento de directiva de seguridad local (Secpol.msc) o la directiva de grupo.

**Las peticiones de credenciales y consentimiento**

Con UAC habilitado, Windows Server 2012 solicita consentimiento o solicita las credenciales de una cuenta de administrador local válida antes de iniciar un programa o una tarea que requiere un token de acceso de administrador completo. Con esta petición, se garantiza que no se va a instalar ningún software malintencionado de forma inadvertida.

**La petición de consentimiento**

La petición de consentimiento aparece cuando un usuario trata de realizar una tarea que requiere un token de acceso administrativo de usuario. A continuación se muestra una captura de pantalla de la petición de consentimiento de UAC.

![Captura de pantalla de la petición de consentimiento de UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**La petición de credenciales**

La petición de credenciales aparece cuando un usuario estándar trata de realizar una tarea que requiere un token de acceso administrativo de usuario. El comportamiento de petición predeterminado de usuario estándar se puede configurar mediante el complemento de directiva de seguridad local (Secpol.msc) o la directiva de grupo. Si el valor de la configuración de directiva Control de cuentas de usuario: comportamiento de la petición de elevación para los administradores en Modo de aprobación de administrador se establece en Pedir credenciales, los administradores también tendrán que suministrar sus credenciales.

La captura de pantalla siguiente es un ejemplo de la solicitud de credenciales de UAC.

![Captura de pantalla que muestra un ejemplo de la solicitud de credenciales de UAC](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Peticiones de elevación de UAC**

Las peticiones de elevación de UAC se codifican mediante colores para que sean específicas de cada aplicación, lo que permite saber al instante el posible riesgo que una aplicación entraña. Cuando una aplicación intenta ejecutarse con un token de acceso completo de administrador, Windows Server 2012 primero analiza el archivo ejecutable para determinar su publicador. En primer lugar, las aplicaciones se dividen en tres categorías basadas en el publicador del archivo ejecutable: Windows Server 2012, publicador comprobado (firmado) y editor no comprobado (sin firmar). En el diagrama siguiente se muestra cómo Windows Server 2012 determina qué petición de elevación de color se presenta al usuario.

La codificación de color de las peticiones de elevación es la siguiente:

-   Fondo rojo con un icono de escudo rojo: la directiva de grupo ha bloqueado la aplicación o esta procede de un publicador que está bloqueado.

-   Fondo azul con un icono de escudo azul y oro: la aplicación es una aplicación administrativa de Windows Server 2012, como un elemento del panel de control.

-   Fondo azul con un icono de escudo azul: la aplicación se ha firmado mediante Authenticode y es de confianza en el equipo local.

-   Fondo amarillo con un icono de escudo amarillo: la aplicación no está firmada, o bien sí lo está pero no es de confianza en el equipo local.

**Icono de escudo**

Algunos elementos del Panel de control (como **Propiedades de fecha y hora**) contienen una combinación de operaciones de usuario estándar y administrador. Así, los usuarios estándar pueden ver el reloj y cambiar la zona horaria, pero necesitan un token de acceso de administrador completo para cambiar la hora del sistema local. La siguiente es una captura de pantalla del elemento del panel de control **propiedades de fecha y hora** .

![Captura de pantalla que muestra el elemento de panel de control * * propiedades de fecha y hora * *](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

El icono de escudo en el botón **Cambiar fecha y hora** pone de manifiesto que el proceso requiere un token de acceso de administrador completo y se mostrará una petición de elevación de UAC.

**Asegurar la petición de elevación**

El proceso de elevación se asegura más aún cuando la petición se dirige al escritorio seguro. De forma predeterminada, los mensajes de consentimiento y de credenciales se muestran en el escritorio seguro en Windows Server 2012. Solo los procesos de Windows pueden tener acceso al escritorio seguro. Para disfrutar de un mayor nivel de seguridad, se recomienda dejar habilitada la configuración de directiva **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación**.

Cuando un archivo ejecutable solicita la elevación, el escritorio interactivo (también conocido como escritorio de usuario) cambia a escritorio seguro. Cuando esto sucede, el escritorio de usuario se atenúa y se muestra una petición de elevación que se debe responder para poder continuar. Cuando el usuario hace clic en Sí o en No, se regresa al escritorio de usuario.

El malware puede aparecer como una imitación del escritorio seguro, pero si la configuración de directiva Control de cuentas de usuario: comportamiento de la petición de elevación para los administradores en Modo de aprobación de administrador está establecida en Pedir consentimiento, no podrá obtener la elevación en caso de que el usuario haga clic en Sí en la imitación. Si esta configuración de directiva está establecida en Pedir credenciales, sí existirá la posibilidad de que el malware que imita a la petición de credenciales se haga con las credenciales del usuario. Sin embargo, no conseguirá el privilegio elevado y el sistema dispone de otras protecciones que evitan que tome el control de la interfaz de usuario con una contraseña recopilada.

Aunque el malware es capaz de aparecer como una imitación del escritorio seguro, esto no sucede a menos que un usuario haya instalado previamente el malware en el equipo. Como los procesos que requieren un token de acceso de administrador no pueden realizar instalaciones silenciosas cuando UAC está habilitado, el usuario deberá dar su consentimiento expreso haciendo clic en **Sí** o bien proporcionar credenciales de administrador. El comportamiento concreto de la petición de elevación de UAC depende de la directiva de grupo.

## <a name="uac-architecture"></a>Arquitectura de UAC
En el siguiente diagrama se ilustra la arquitectura de UAC.

![Diagrama que detalla la arquitectura de UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Para comprender mejor cada componente, revise la siguiente tabla:

|Componente|Descripción|
|-------|--------|
|**User**||
|El usuario realiza una operación que precisa de privilegios|Si la operación supone un cambio en el sistema de archivos o en el Registro, se llama a virtualización. El resto de operaciones llama a ShellExecute.|
|ShellExecute|ShellExecute llama a CreateProcess. ShellExecute busca el error ERROR_ELEVATION_REQUIRED en CreateProcess. Si recibe el error, ShellExecute llama al Servicio de información de aplicaciones para tratar de realizar la tarea solicitada con la petición elevada.|
|CreateProcess|Si la aplicación precisa de elevación, CreateProcess rechaza la llamada con ERROR_ELEVATION_REQUIRED.|
|**Sistema**||
|Servicio de información de aplicaciones|Servicio del sistema que ayuda a iniciar aquellas aplicaciones que requieran uno o más privilegios elevados o derechos de usuario para ejecutarse (como las tareas administrativas locales) o las aplicaciones que necesiten un mayor nivel de integridad. Para que dichas aplicaciones se inicien, el Servicio de información de aplicaciones crea un nuevo proceso para la aplicación con un token de acceso completo de usuario administrativo cuando la elevación sea necesaria y (en función de la directiva de grupo) exista el consentimiento por parte del usuario para ello.|
|Elevación de una instalación de ActiveX|Si ActiveX no está instalado, el sistema comprueba el nivel de control deslizante de UAC. Si ActiveX está instalado, se comprueba la configuración de directiva de grupo **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación**.|
|Comprobación del nivel de control deslizante de UAC|Ahora UAC ofrece la posibilidad de elegir entre cuatro niveles de notificación distintos usando un control deslizante:<p><ul><li>Alto<p>    Cuando el control deslizante se establece en **Notificarme siempre**, el sistema comprueba si el escritorio seguro está habilitado</li><li>Media<p>    Cuando el control deslizante se establece en **Predeterminado: notificarme sólo cuando un programa intente realizar cambios en el equipo**, se comprueba la configuración de directiva de grupo **Control de cuentas de usuario: elevar sólo los archivos ejecutables firmados y validados**:<p><ul><li>Si está habilitada, se aplica la validación de la ruta de certificación de la infraestructura de clave pública (PKI) a un archivo ejecutable concreto para que pueda ejecutarse.</li><li>Si no está habilitada (que es el valor predeterminado), no se aplica la validación de la ruta de certificación de la infraestructura de clave pública (PKI) a un archivo ejecutable concreto para que pueda ejecutarse. Se comprueba la configuración de directiva de grupo **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación**.</li></ul></li><li>Bajo<p>    Cuando el control deslizante se establece en **Notificarme sólo cuando un programa intente realizar cambios en el equipo (no atenuar el escritorio)**, se llama a CreateProcess.</li><li>No notificarme nunca<p>    Si el control deslizante se establece en no **Notificarme nunca cuando**, el símbolo del sistema de UAC nunca notificará cuando un programa esté intentando instalar o intentando realizar algún cambio en el equipo. **Importante:**     No se recomienda esta configuración. Esta configuración es la misma que la configuración de directiva **Control de cuentas de usuario: comportamiento de la petición de elevación para los administradores en el modo de aprobación de administrador** en **Elevar sin preguntar**.</li></ul>|
|Escritorio seguro habilitado|Se comprueba la configuración de directiva de grupo **Control de cuentas de usuario: cambiar al escritorio seguro cuando se pida confirmación de elevación**:<p>-Si el escritorio seguro está habilitado, todas las solicitudes de elevación van al escritorio seguro, independientemente de la configuración de la Directiva de comportamiento de petición para administradores y usuarios estándar.<br />-Si el escritorio seguro no está habilitado, todas las solicitudes de elevación van al escritorio del usuario interactivo y se utiliza la configuración por usuario para los administradores y los usuarios estándar.|
|CreateProcess|CreateProcess llama a AppCompat, Fusión y Detección del instalador para sopesar si la aplicación precisa de elevación. Tras ello, el archivo ejecutable se inspecciona con objeto de averiguar el nivel de ejecución solicitado correspondiente, información que se almacena en el manifiesto de aplicación del archivo en cuestión. Si el nivel de ejecución solicitado que refleja el manifiesto de aplicación no es el mismo que el token de acceso, CreateProcess devuelve un error (ERROR_ELEVATION_REQUIRED) a ShellExecute. |
|AppCompat|La base de datos AppCompat almacena información en las entradas de corrección de compatibilidad de una aplicación.|
|Fusión|La base de datos Fusión almacena la información de los manifiestos de aplicación que describe a las aplicaciones. El esquema del manifiesto se actualiza para agregar un nuevo campo de nivel de ejecución solicitado.|
|Detección del instalador|La detección del instalador detecta los archivos ejecutables de instalación, lo que contribuye a impedir que se efectúen instalaciones sin el conocimiento o autorización del usuario.|
|**Kernel**||
|Virtualización|La tecnología de virtualización garantiza que las aplicaciones no conformes no van a dejar de funcionar o generen errores sin aviso de un modo que impida conocer las causas. UAC permite además el registro y virtualización de archivos y del Registro para las aplicaciones que escriben en áreas protegidas.|
|Sistema de archivos y Registro|La virtualización de Registro y archivo por usuario redirige las solicitudes de escritura en archivo y Registro por equipo a las ubicaciones por usuario equivalentes. En cuanto a las solicitudes de lectura, se redirigen primero a la ubicación por usuario virtualizada y, después, a la ubicación por equipo.|

Hay un cambio en el UAC de Windows Server 2012 a partir de versiones anteriores de Windows. El control deslizante nuevo nunca desactivará UAC. La nueva configuración hará lo siguiente:

-   Mantendrá el servicio UAC en ejecución.

-   Hará que todas las solicitudes de elevación iniciadas por los administradores se aprueben automáticamente sin mostrar un símbolo del sistema del UAC.

-   Denegará automáticamente todas las solicitudes de elevación para los usuarios estándar.

> [!IMPORTANT]
> Para deshabilitar completamente el UAC, debes deshabilitar la directiva **Control de cuentas de usuario: ejecutar todos los administradores en Modo de aprobación de administrador**.

> [!WARNING]
> Las aplicaciones personalizadas no funcionarán en Windows Server 2012 cuando UAC esté deshabilitado.

### <a name="virtualization"></a>Virtualización
Dado que los administradores de sistema de los entornos empresariales intentan mantener los sistemas protegidos, muchas de las aplicaciones de línea de negocio (LOB) se diseñan para que usen únicamente un token de acceso de usuario estándar. Como resultado, no es necesario que los administradores de ti reemplacen la mayoría de las aplicaciones al ejecutar Windows Server 2012 con UAC habilitado.

 Windows Server 2012 incluye la tecnología de virtualización de archivos y del registro para las aplicaciones que no son conformes a UAC y que requieren que el token de acceso de un administrador se ejecute correctamente. La virtualización garantiza que incluso las aplicaciones que no son conformes a UAC son compatibles con Windows Server 2012. Cuando una aplicación administrativa que no es conforme a UAC intenta escribir en un directorio protegido (como la carpeta Archivos de programa), UAC ofrece a la aplicación en cuestión su propia vista virtualizada del recurso que está tratando de cambiar. La copia virtualizada se conserva en el perfil del usuario. Con esta estrategia se crea una copia independiente del archivo virtualizado para cada uno de los usuarios que ejecutan la aplicación no conforme.

La mayor parte de las tareas de aplicación se ejecuta adecuadamente mediante características de virtualización. Con todo, aunque la virtualización permite que la mayoría de las aplicaciones se ejecute, se trata de un remedio a corto plazo, más que una solución a largo plazo. Los desarrolladores de aplicaciones deben modificar sus aplicaciones para que sean compatibles con el programa del logotipo de Windows Server 2012 tan pronto como sea posible, en lugar de depender del archivo, la carpeta y la virtualización del registro.

La virtualización no es la mejor elección en los siguientes escenarios:

1.  La virtualización no procede en aplicaciones que se elevan y ejecutan con un token de acceso administrativo completo.

2.  La virtualización admite solo aplicaciones de 32 bits. Las aplicaciones de 64 bits no elevadas simplemente reciben un mensaje de acceso denegado cuando tratan de obtener un identificador (único) de un objeto de Windows. Es necesario que las aplicaciones Windows nativas de 64 bits sean conformes a UAC y escriban datos en las ubicaciones correctas.

3.  La virtualización se deshabilitará para una aplicación si esta incluye un manifiesto de aplicación con un atributo de nivel de ejecución solicitado.

### <a name="request-execution-levels"></a>Niveles de ejecución de solicitudes
Un manifiesto de aplicación es un archivo XML que describe e identifica los ensamblados en paralelo, compartidos y privados, a los que debe enlazarse una aplicación en tiempo de ejecución. En Windows Server 2012, el manifiesto de aplicación incluye entradas para la compatibilidad de aplicaciones de UAC. Las aplicaciones administrativas que tengan una entrada en el manifiesto de aplicación piden permiso al usuario para obtener acceso al token de acceso del usuario. A pesar de que carecen de una entrada en el manifiesto de aplicación, casi todas las aplicaciones administrativas se pueden ejecutar sin modificación mediante correcciones de compatibilidad de aplicación. Las correcciones de compatibilidad de aplicaciones son entradas de base de datos que permiten que las aplicaciones que no son compatibles con UAC funcionen correctamente con Windows Server 2012.

Todas las aplicaciones conformes a UAC deben tener un nivel de ejecución solicitado agregado al manifiesto de aplicación Si la aplicación necesita acceso administrativo al sistema, al marcarla con un nivel de ejecución solicitado de "se requiere administrador" se garantiza que el sistema va a considerar este programa como una aplicación administrativa y se llevan a cabo los pasos de elevación pertinentes. Los niveles de ejecución solicitados establecen los privilegios necesarios para una aplicación.

### <a name="installer-detection-technology"></a>Tecnología de detección del instalador
Los programas de instalación son aplicaciones cuyo objetivo es implementar software. Prácticamente todos los programas de instalación escriben en los directorios del sistema y las claves del Registro. Con la tecnología de detección del instalador, normalmente solo el administrador puede escribir en estas ubicaciones de sistema protegidas, lo que significa que los usuarios estándar carecen de privilegios suficientes para instalar programar. Windows Server 2012 detecta de forma heurística los programas de instalación y solicita las credenciales de administrador o la aprobación del usuario administrador para poder ejecutarlos con privilegios de acceso. Windows Server 2012 también detecta heurísticamente las actualizaciones y programas que desinstalan aplicaciones. Uno de los objetivos de diseño de UAC reside en evitar que las instalaciones se ejecuten sin el conocimiento o autorización del usuario, ya que los programas de instalación escriben en áreas protegidas del sistema de archivos o el Registro.

La detección del instalador se aplica únicamente a los siguientes elementos:

-   Archivos ejecutables de 32 bits.

-   Aplicaciones sin un atributo de nivel de ejecución solicitado.

-   Procesos interactivos que se ejecutan como un usuario estándar con UAC habilitado.

Antes de que se cree un proceso de 32 bits, se comprueban los siguientes atributos para saber si se trata de un instalador:

-   El nombre de archivo comprende palabras clave como "install", "setup" o "update" (que hacen referencia a instalaciones o actualizaciones).

-   Los campos de recursos de control de versiones contienen palabras clave del tipo proveedor, nombre de compañía, nombre de producto, descripción de archivo, nombre de archivo original, nombre interno y nombre de exportación.

-   Las palabras clave en el manifiesto en paralelo están integradas en el archivo ejecutable.

-   Las palabras clave en entradas específicas de StringTable están integradas en el archivo ejecutable.

-   Los atributos clave de los datos de script de recurso están vinculados en el archivo ejecutable.

-   Existen secuencias de bytes de destino en el archivo ejecutable.

> [!NOTE]
> Las palabras clave y las secuencias de bytes se derivaron de las características comunes observadas de diferentes tecnologías de instalador.

> [!NOTE]
> El control de cuentas de usuario: la detección de instalaciones de aplicaciones y la petición de configuración de directivas de elevación deben estar habilitadas para que la detección del instalador detecte los programas de instalación. Esta configuración está habilitada de manera predeterminada y se puede definir localmente mediante el complemento de directiva de seguridad local (Secpol.msc), o bien establecer para el dominio, la unidad organizativa o grupos concretos mediante la directiva de grupo (Gpedit.msc).


