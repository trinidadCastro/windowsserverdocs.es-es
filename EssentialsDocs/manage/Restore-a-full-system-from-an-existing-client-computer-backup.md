---
title: Restaurar un sistema completo de una copia de seguridad del equipo cliente existente
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cfc4d1ce461e1e1cbb9b99970355c4dc7241911b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restaurar un sistema completo de una copia de seguridad del equipo cliente existente

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Errores de hardware y del sistema operativo son poco frecuentes, pero pueden producirse. Un ventilador no funciona correctamente podría sobrecalentamiento una placa base del equipo y hacer sea inútil. El sistema operativo pudo dañado y no se iniciará. Fuego y el agua puede provocar daños en el hardware permanente. Puede producir un error en una unidad de disco duro o decides reemplazarla por una unidad de disco duro más grande.  
  
 Este documento proporciona información sobre los siguientes temas:  
  
-   [¿Qué es la restauración del sistema completa equipo?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [¿Cómo funciona el entorno de restauración del sistema?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Crear una unidad flash USB de arranque para restaurar un equipo cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [Con el Asistente de restauración del sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [¿Dónde puedo encontrar los controladores para el hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="BKMK_WhatIs"></a>¿Qué es la restauración del sistema completa equipo?  
 En caso de que reemplazar una unidad de disco duro o el equipo se hasta el punto donde no se puede usar o no se inicia, puedes restaurar el sistema desde una copia de seguridad del equipo anterior. Una restauración completa del sistema, devuelve el sistema al estado que estaba en el momento de la copia de seguridad.  
  
> [!IMPORTANT]
>  No puedes realizar una restauración completa del sistema en el hardware del equipo, como una placa del sistema, que no es similar al que se va a reemplazar el hardware del equipo. Un sistema operativo instalado está estrechamente depende del hardware subyacente del equipo. Sin embargo, puedes realizar una restauración completa del sistema a un disco duro que sea igual en tamaño o mayor que el que se va a reemplazar.  
  
 Al realizar una restauración completa del sistema, puedes elegir una copia de seguridad específicos del equipo para restaurar el sistema, con todas las aplicaciones, configuraciones y configuración familiar para el usuario antes de los errores, catástrofe o robo. También puede elegir los volúmenes que desea restaurar.  
  
 Al planear o Preparándose para restaurar el sistema completo en un equipo de red, teniendo en cuenta lo siguiente:  
  
### <a name="windows-preinstallation-environment"></a>Entorno de preinstalación de Windows  
 Entorno de preinstalación de Windows (Windows PE) es un sistema operativo mínimo diseñado para preparar un equipo para la instalación de Windows. Para servidores que ejecutan Windows Server Essentials, Windows PE se instala automáticamente cuando se inserta el medio de restauración en un equipo restaurarse. Para servidores que ejecutan Windows Server Essentials, Windows PE se instala automáticamente al iniciar el equipo con el servicio de restauración de cliente o con la unidad flash USB.  
  
 Windows PE no admite conexiones inalámbricas. Por este motivo, el equipo está restaurando debe estar conectado físicamente a la red de pequeñas empresas.  
  
### <a name="bitlocker"></a>BitLocker  
 Cifrado de unidad BitLocker (BitLocker) es una característica de protección de datos que está disponible en algunas versiones de Windows Vista, Windows 7 y Windows 8. BitLocker protege contra robos o exposición en equipos perdidos o robados y ofrece la eliminación de datos es más seguro cuando se retiran los equipos.  
  
 **Para Windows Server Essentials:** si el equipo que necesita restaurar cifrado con BitLocker (si se encontraba la unidad del sistema operativo o la unidad del sistema operativo y único o varias otras unidades fijas), puedes seguir usando el medio de restauración del sistema completo contenido en el CD proporcionado con el servidor y el Asistente para restaurar sistema completo para volver a instalar la imagen de disco duro, incluido el sistema operativo, desde una copia de seguridad y restaurar los datos en el equipo nuevo o reparar.  
  
 **Para Windows Server Essentials:** si el equipo que necesita restaurar cifrado con BitLocker (si se encontraba la unidad del sistema operativo o la unidad del sistema operativo y único o varias otras unidades fijas), seguir usando el Asistente para restaurar sistema completo para volver a instalar la imagen de disco duro, incluido el sistema operativo, desde una copia de seguridad y restaurar los datos en el equipo nuevo o reparar.  
  
 Cuando el servidor realiza copias de seguridad de archivos, carpetas y unidades, se guarda una versión sin cifrar en el servidor. Durante la restauración del sistema completa, esta versión sin cifrar se copia en el equipo.  
  
> [!NOTE]
>  Tras una restauración completa del sistema correcta, debe volver a activar BitLocker en el equipo.  
>   
>  Para obtener instrucciones sobre cómo habilitar BitLocker en equipos que ejecutan Windows 8, consulta [BitLocker: cómo habilitar BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Para obtener instrucciones sobre cómo habilitar BitLocker en equipos que ejecutan Windows 7, consulta [cifrado de unidad BitLocker Step-by-Step guía para Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Para obtener más información sobre los conceptos básicos de cifrado de unidad BitLocker, consulta [BitLocker más preguntas frecuentes (P+F)](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Cifrado de archivos cifrados por el sistema de archivos  
 La característica del sistema de cifrado de archivos (EFS) de Windows puede proporcionar el cifrado de nivel de archivo basada en usuarios adicionales para distintos niveles de seguridad entre varios usuarios del mismo equipo. Es importante tener en cuenta que, a diferencia de unidades cifradas con BitLocker, archivos y carpetas cifrados con EFS seguirán cifrarse en cualquier copia de seguridad del equipo. EFS no está disponible en Windows XP Home Edition, Windows Vista Starter, Windows Vista Home Basic, Windows Vista Home Premium, Windows 7 Starter, Windows 7 Home Basic, Windows 7 Home Premium o Windows 8. Solo está disponible en Windows 8 Pro.  
  
> [!WARNING]
>  A diferencia de BitLocker, solo puede acceder a archivos protegidos con EFS desde dentro del sistema operativo que ellos cifradas.  
  
### <a name="disk-partitions"></a>Particiones de disco  
 Si el tamaño de unidad de disco duro en el equipo nuevo es el mismo o más grande que el original, el disco duro unidad de disco se vuelve a formatear y volver a particionar automáticamente. Consulte el gráfico a continuación para obtener información específica:  
  
|Equipo original|Restaurada o nuevo equipo|  
|-----------------------|------------------------------|  
|Solo disco con varias particiones|Solo disco con varias particiones y todo el espacio extra se asigna a la última partición|  
|Solo disco con una sola partición|Se usa solo disco con una partición única y todo el espacio disponible para la partición única|  
  
> [!NOTE]
>  Si existen diferencias de diseño de tamaño y la partición de disco con el entre el original y el equipo nuevo o restaurado, debes usar Administración de discos para crear las particiones apropiadas en el equipo nuevo o restaurado. Puedes hacerlo en el Asistente para restaurar sistema completo.  
  
### <a name="raid-and-dynamic-disks"></a>RAID y los discos dinámicos  
 Realizar copias de seguridad de la matriz redundante de discos independientes (RAID) y discos dinámicos no se admiten.  
  
##  <a name="BKMK_HowDoes"></a>¿Cómo funciona el entorno de restauración del sistema?  
 El medio de restauración del sistema proporcionado con ServerÂ Windows (r) 2012 Essentials instala el entorno de preinstalación de Windows (Windows PE) en el equipo. Windows PE reemplaza el entorno de MS-DOS y contiene los archivos de programa principal para Windows. En Windows Server Essentials, existen dos formas compatibles para restaurar un sistema: con el servicio de restauración del cliente, que usa una red y no dependen de medios o con USB unidad flash.  
  
> [!NOTE]
>  Windows PE no admite conexiones inalámbricas. Por este motivo, el equipo está restaurando debe estar conectado físicamente a la red de pequeñas empresas.  
  
 El entorno de restauración del sistema incluye (x86) 32 bits y 64 bits (x64) archivos de programa. Después de insertar el sistema medio de restauración, elige la versión apropiada de los archivos. 32 bits (x86) es el valor predeterminado y se selecciona automáticamente si elige no transcurridos 30 segundos. Si hay actualizaciones para el sistema completo restauran archivos de programa en el servidor, los archivos actualizados se descargan automáticamente en el equipo.  
  
 Después de configura el entorno de preinstalación, inicia el Asistente para restaurar sistema completo. El asistente ayuda a restaurar el equipo desde una copia de seguridad anterior. También puedes usar el entorno de restauración del sistema para restaurar una copia de seguridad a un nuevo equipo con un hardware similar.  
  
 En la mayoría de los casos, los archivos de programa y controladores incluidos en el entorno de restauración del sistema son todo lo que es necesario reiniciar el equipo nuevo o restaurado. Según el hardware del equipo nuevo o restaurado, el entorno de restauración del sistema no puede incluir todo el almacenamiento y red controladores de adaptador que son necesarios cuando se reinicie el equipo nuevo o restaurado. El Asistente para restaurar sistema completo te ofrece una oportunidad para instalar a los controladores, si es necesario. ¿Para obtener información acerca de cómo buscar los controladores de hardware, consulta [dónde puedo encontrar los controladores para el hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Para obtener información sobre cómo usar el medio de restauración del sistema, consulta [mediante el Asistente para restaurar sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="BKMK_CreateBootable"></a>Crear una unidad flash USB de arranque para restaurar un equipo cliente  
 Si necesita restaurar un equipo cliente desde una copia de seguridad existente, pero no puede encontrar el CD restaurar suministrada con el servidor (en Windows Server Essentials) o no desea configurar el servicio de restauración de cliente en el servidor (en Windows Server Essentials), puedes crear una unidad flash USB de arranque. A continuación, puedes usar la unidad flash USB para iniciar el equipo cliente y restaurar el sistema. La unidad flash USB que usas debe ser al menos 1 GB o más grande.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Para crear una unidad flash USB de arranque  
  
1.  Abre el **panel**.  
  
2.  Haz clic en el **dispositivos** pestaña.  
  
3.  En **tareas** panel, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, haz clic en **tareas de copia de seguridad de equipos cliente**.  
  
4.  Haz clic en el **herramientas** pestaña y, a continuación, haz clic en **crear clave** en la **recuperación del equipo** sección. Abre el Asistente para crear clave de recuperación de equipo.  
  
5.  Inserta un 1 GB o más grandes USB en el servidor de una unidad flash y, a continuación, sigue las instrucciones del asistente.  
  
    > [!CAUTION]
    >  Se eliminarán todos los datos de la unidad flash USB.  
  
##  <a name="BKMK_Using"></a>Con el Asistente de restauración del sistema completo  
 Después de usar correctamente la restauración multimedia, servicio de restauración del cliente o unidad flash USB para iniciar el equipo y comprueba que aparezca todo el hardware de controladores se cargan en el equipo cliente restaurada o nuevo, el Asistente para restaurar sistema completo. Este asistente te permite acceder a los volúmenes de origen que deseas restaurar el equipo y realiza el proceso de restauración real, la copia de seguridad del equipo y el servidor.  
  
> [!NOTE]
>   Windows Server Essentials no admite los siguientes escenarios de restauración:  
>   
>  -   Equipos basados en restaurar un disco de registro de arranque maestro (MBR) a un Unified Extensible Firmware Interface (UEFI).  
> -   Restaurar una copia de seguridad UEFI/GPT para un sistema BIOS.  
>   
>  Si restauras datos en cualquiera de estos escenarios, no podrá arrancar el sistema. Además, es posible que no podrás usar unidades de disco duro más de dos terabytes de tamaño.  
  
 **Requisitos previos:**  
  
-   Antes de iniciar el sistema completo el proceso de restauración, usa un cable de red (una conexión por cable) para conectar el equipo a la misma red que el servidor. Asegúrate de que tienes acceso a todas las unidades de disco duros en el equipo cliente.  
  
    > [!WARNING]
    >  No intente realizar una restauración completa del sistema a un equipo que usa una conexión a la red inalámbrica.  
  
-   Si sabes que el equipo no tiene red crítica o controladores de dispositivos de almacenamiento, tendrás que buscar y copiar esos controladores en una unidad flash antes de iniciar el proceso de restauración del sistema completo. Para Windows Server Essentials: Si estás usando el medio de restauración del sistema completo que se proporciona en CD, CD debe permanecer en la unidad durante la fase del inicio del proceso de restauración del sistema completo. Por lo tanto, no debe copiar los controladores que faltan en un CD o DVD, a menos que tengas una segunda unidad de CD o DVD. En su lugar, copia los controladores que faltan en una unidad flash USB.  
  
     ¿Para obtener información acerca de cómo encontrar los controladores para el equipo, consulta [dónde puedo encontrar los controladores para el hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   Para Windows Server Essentials: Si no encuentra el CD de restauración de Windows Server Essentials, puedes crear una unidad flash USB de arranque. Para obtener más información, consulta [crear una unidad flash USB de arranque para restaurar un equipo cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Usar el Asistente para restaurar sistema completo  
  
1.  Realiza una de las siguientes acciones:  
  
    -   Windows Server Essentials: Activa el equipo cliente que desea restaurar, inserta el medio de restauración y, a continuación, apague el equipo.  
  
         Enciende el equipo de nuevo, durante la energía Self comprobación encendido (POST), presiona la tecla de función correspondiente (F teclas) para acceder al menú de dispositivo de arranque y, a continuación, selecciona la unidad de CD o DVD. Se inicia el Administrador de arranque de Windows.  
  
    -   Windows Server Essentials: Si estás usando el servicio de restauración de cliente, reinicia el equipo mediante el **arranque de red** opción. De lo contrario, inicia el equipo mediante la clave USB.  
  
         Enciende el equipo de nuevo y durante la energía Self comprobación encendido (POST), presiona la tecla de función correspondiente (F teclas) para acceder al menú de dispositivo de arranque y, a continuación, selecciona **arranque de red** (o puedes elegir arrancar desde la clave USB). Se inicia el Administrador de arranque de Windows.  
  
    > [!NOTE]
    >  Consulta la documentación del fabricante del equipo para determinar qué tecla de función tiene acceso al menú de dispositivo de arranque.  
  
2.  El medio de restauración de equipo contiene (x86) 32 bits y opciones de arranque de 64 bits (x 64). En el Administrador de arranque de Windows, elige **restauración del sistema completa (x86)** o **restauración del sistema completa (x64)**. Si los controladores de hardware del equipo están 32 bits, elija x86; Si están 64 bits, elija x64. Los archivos de Windows se cargan y el Asistente para restaurar sistema completa lleva a cabo una comprobación para asegurarse de que todos los controladores de hardware están disponibles.  
  
3.  En la **completa Asistente para restaurar sistema**, elige el idioma que prefieras y, a continuación, haz clic en la flecha.  
  
4.  Elige el apropiado **formato de hora y moneda**y el **teclado o método de entrada** para este equipo. Haz clic en **continuar**.  
  
5.  Si faltan controladores, se muestra el mensaje en que el proceso de restauración no puede comprobar los controladores. Haz clic en **cerrar**y en el cuadro de diálogo de bienvenida, después, haz clic en **cargar controladores**.  
  
    1.  En la **detectar Hardware** cuadro de diálogo, haz clic en **instalar controladores**.  
  
    2.  Inserta la unidad flash USB que contiene los controladores de hardware, y, a continuación, en la **instalar controladores** cuadro de diálogo, haz clic en **digitalizar**.  
  
    3.  En la **instalar controladores** cuadro de diálogo, haz clic en **Aceptar** cuando se encuentran los controladores.  
  
    4.  En la **detectar Hardware** cuadro de diálogo, haz clic en **continuar**.  
  
6.  Si todos los controladores se encuentran en la comprobación inicial, o cuando se instalan todos los controladores críticos, en la **restauración del sistema completa** ventana, haz clic en **continuar**.  
  
7.  En la **el Asistente para restaurar sistema completo** página, haz clic en **siguiente**.  
  
8.  Busca en el Asistente de tu servidor.  
  
    1.  Si el asistente no puede encontrar el servidor, tiene la opción Buscar de nuevo, o escribe la dirección IP del servidor.  
  
    2.  Si se detectan varios servidores, se te pide que selecciones una aplicación.  
  
    3.  Si el servidor se encuentra, el **< YourServerName\ > Inicio de sesión** se muestra la página.  
  
9. En la **< YourServerName\ > Inicio de sesión**, escriba *< AdministratorAccountName\ >* en la **nombre de usuario** cuadro de texto y la contraseña de cuenta de administrador en el **contraseña** cuadro de texto y, a continuación, haz clic en **siguiente**.  
  
    > [!IMPORTANT]
    >  Debes usar una cuenta de administrador que se crea en inglés. Si no tienes uno, a continuación, debes crear una nueva cuenta de administrador. Para ello, abre la **usuarios** ficha en el panel de servidor, a continuación establece el formato de idioma de teclado en inglés y, a continuación, ejecutar la **agregar una cuenta de usuario** tarea para crear la cuenta de administrador. A continuación, usa la nueva cuenta de administrador para continuar restaurar el equipo cliente.  
  
10. En la **seleccionar un equipo para restaurar** página, selecciona el equipo que desea restaurar y, a continuación, haz clic en **siguiente**. Puedes elegir **< ComputerName\ >: (este equipo)** o elegir un equipo diferente de la red desde el **otro equipo** lista desplegable.  
  
    > [!NOTE]
    >  Si se trata de un equipo que se desconoce en el servidor (por ejemplo, un nuevo o un equipo reasingado), el **este equipo** opción no se muestra.  
  
11. En la **selecciona una copia de seguridad para restaurar** página, revise la lista de las copias de seguridad disponibles y selecciona el que desea restaurar en el equipo.  
  
    > [!NOTE]
    >  Se recomienda que selecciones una copia de seguridad correcta (activado en verde). Esto ayuda a garantizar que todos los archivos de sistema y los datos se restauran correctamente.  
  
12. (*Opcional*) Selecciona una copia de seguridad y, a continuación, haz clic en **detalles** para abrir el **detalles de copia de seguridad** página y ver más información sobre esa copia de seguridad. Utilice la información de la **detalles de copia de seguridad** página para comparar varias copias de seguridad y ayudarte a decidir qué copia de seguridad es la mejor opción. Haz clic en **cerrar** en la **detalles de copia de seguridad** página para volver a la **selecciona una copia de seguridad para restaurar** página.  
  
13. En la **selecciona una copia de seguridad para restaurar** página, selecciona una copia de seguridad y, a continuación, haz clic en el **siguiente** botón.  
  
14. En la **seleccionar la opción de restauración** página, haz clic en uno de estos procedimientos y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  Esta página no se muestra si la partición automática no es compatible.  
  
    1.  **Permitir que el Asistente para realizar una restauración completa del equipo (recomendado)**. Esta opción ayuda a garantizar que el equipo se restaurará al estado que estaba justo antes de la hora y fecha de la copia de seguridad que se eligió. Si elige esta opción, avanza al paso 15.  
  
    2.  **Seleccionar los volúmenes para restaurar (avanzado)**. Esta opción permite elegir los volúmenes que desea restaurar y donde quieres restaurarlos. También puedes crear particiones en el disco duro.  
  
15. En la **selecciona los volúmenes restaurar** página, puedes elegir los volúmenes que desea restaurar.  
  
    > [!NOTE]
    >  Esta página se muestra si existen varias unidades de disco duras del equipo de origen de copia de seguridad, o si la unidad de destino de restauración tiene menos espacio de almacenamiento que la unidad de origen de copia de seguridad.  
  
    1.  El asistente intenta hacer coincidir los volúmenes de origen y de destino. Debes comprobar que la asignación predeterminada es correcta.  
  
        1.  Para anular la selección de un volumen, haga clic en la flecha del menú lista para ese volumen y, a continuación, haz clic en **ninguno**.  
  
        2.  Cuando termines de seleccionar los volúmenes, haz clic en **siguiente**.  
  
    2.  Si el volumen de origen y el volumen de destino son del mismo tamaño, o si el tamaño de fuente es menor que el destino, aparece una flecha verde entre las dos. Si hay un error de coincidencia de tamaño de volumen (donde el volumen de origen es mayor que el volumen de destino), aparece una X roja entre el origen y destino.  
  
        > [!NOTE]
        >  Una X roja también puede aparecer si:  
        >   
        >  -   El tamaño de sector de disco del volumen de origen no coincide con el tamaño de sector de disco del volumen de destino. Esto puede ocurrir si el disco físico, se reemplaza con un disco que tiene un tamaño de sector diferente, o si configuras los espacios de almacenamiento (que pueden tener un tamaño de sector distinto que el del disco físico).  
        > -   Llegar a la limitación del número de clúster. Para restaurar el volumen de origen para el volumen de destino, debes formatear el volumen de destino con el mismo tamaño de clúster como el volumen de origen. Si el volumen de destino es demasiado grande, y el tamaño del clúster es demasiado pequeño, puede ponerse en contacto limitación de número de clúster.  
  
        1.  Haz clic en **ejecutar Disk Manager (avanzado)**y crea un nuevo volumen que tiene el mismo tamaño como el volumen del sistema reservado.  
  
            > [!NOTE]
            >  Si un equipo cliente es Unified Extensible Firmware Interface (UEFI) en función, debes usar el **diskpart** herramienta para inicializar el disco del sistema. Para ello, abra una ventana de comandos (presiona Ctrl + Alt + Mayús durante 5 segundos en el entorno de WinPE), ejecuta **diskpart.exe**, y, a continuación, ejecuta los siguientes comandos de diskpart:  
            >   
            >  1.  **DISKPART > lista de discos**  
            > 2.  **DISKPART > selecciona disco #***< disco de inicio\ >*  
            > 3.  **DISKPART > limpia**  
            > 4.  **DISKPART > convertir gpt**  
            > 5.  **DISKPART > Crear partición efi tamaño =***100* (donde *100* es un tamaño de partición de ejemplo en MB, debe ser el mismo que la partición original)  
            > 6.  **DISKPART > Crear partición msr tamaño =***128* (donde *128* es un tamaño de partición de ejemplo en MB, debe ser el mismo que la partición original)  
            > 7.  **DISKPART > Salir**  
  
        2.  *(Opcional) *Selecciona la opción **no asigne una letra de unidad o una ruta de unidad**.  
  
        3.  Formatea el volumen como **NTFS**.  
  
        4.  Cuando finalice el formato, haz clic en el volumen del sistema nuevo y, a continuación, haz clic en **Marcar partición como activa**.  
  
        5.  Si necesitas volúmenes adicionales para que se corresponda con otros volúmenes en la copia de seguridad, repite los pasos *ii* a través de *iv* para crear y activar los volúmenes y, a continuación, cierra **administración de discos**.  
  
        6.  En la **selecciona los volúmenes restaurar** página, el sistema reservado el volumen del origen de copia de seguridad para el volumen del mismo tamaño que creaste en el paso de mapa *v*.  
  
        7.  Todos los demás volúmenes de origen se asignan a los volúmenes de destino correspondiente.  
  
        8.  Haz clic en **siguiente** para continuar con la restauración.  
  
16. En la **confirmar volúmenes restaurar** página, revisa la asignación y, a continuación, haz clic en **siguiente**. Si es necesario hacer ningún cambio, haz clic en **Atrás**y, a continuación, repite el paso 14.  
  
17. La **restaurando < ComputerName\ > en < fecha y hora de backup\ >** página informa del progreso del proceso de restauración.  
  
18. En la **la restauración finalice correctamente** página, quitar el medio de restauración y, a continuación, haz clic en **finalizar**. Reinicia el equipo.  
  
    > [!IMPORTANT]
    >  Si se habilitó el cifrado de unidad BitLocker en el equipo antes de la restauración, debe habilitar BitLocker manualmente después de reiniciar el equipo.  
  
##  <a name="BKMK_FindDrivers"></a>¿Dónde puedo encontrar los controladores para el hardware?  
 Según el hardware del equipo nuevo o restaurado, el medio de restauración no puede incluir todo el almacenamiento y los controladores de adaptador que se necesitan cuando se reinicie el equipo restaurado de red. Debes determinar qué controladores faltan, busque esos controladores en medio existente o en el sitio Web s fabricante, copiarlos en una unidad flash y, a continuación, copien de la unidad flash a la nueva o restaurarán el equipo cuando ejecutas el Asistente para restaurar sistema completo.  
  
 Cuando un equipo es una copia de seguridad, los controladores para el equipo se guardan en la copia de seguridad. Si los medios de recuperación no incluyen todos los controladores que necesitas, puedes abrir una copia de seguridad para ese equipo y, a continuación, copia los controladores en una unidad flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Para copiar los controladores de una copia de seguridad a una unidad flash USB  
  
1.  En otro equipo, abra el panel.  
  
2.  Haz clic en **dispositivos**y, a continuación, haz clic en el equipo para el que necesitas controladores.  
  
3.  Haz clic en **restaurar archivos o carpetas para el equipo**. Abre el restaurar archivos o carpetas del asistente.  
  
4.  Haz clic en la última copia de seguridad correcta y, a continuación, haz clic en **siguiente**.  
  
5.  Haz clic en un volumen para abrirlo y, a continuación, haz clic en **siguiente**. Se abre una ventana que enumera los archivos y carpetas en la copia de seguridad.  
  
6.  Insertar la unidad flash USB en un conector USB en el equipo y, a continuación, copia los controladores para la carpeta de restauración del sistema completo en la unidad flash USB.  
  
    > [!NOTE]
    >  Es posible que debas haz clic en **Subir un nivel** hasta llegar a la raíz del volumen del sistema.  
  
7.  Quita la unidad flash y, a continuación, se inserta en el equipo que está restaurando.  
  
 Puedes usar la unidad flash USB para instalar a los controladores para el equipo cuando lo restauras. El restaurar archivos o carpetas asistente busca controladores adicionales en esta unidad flash USB al usar al Asistente para restaurar sistema completo. Los controladores que es más probable que necesite son el controlador del adaptador de red y controladores de dispositivo de almacenamiento.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
