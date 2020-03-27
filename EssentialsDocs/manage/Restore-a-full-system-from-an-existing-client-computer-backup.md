---
title: Restauración completa del sistema desde una copia de seguridad existente del equipo cliente
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b9629f41c4e8eb707b19914a297d9d8b88c6aead
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310655"
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restauración completa del sistema desde una copia de seguridad existente del equipo cliente

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Los fallos de hardware y del sistema operativo no son frecuentes, pero pueden producirse. Un ventilador en mal estado podría calentar la placa base en exceso y hacerla inservible. El sistema operativo podría quedar dañado y no iniciarse. Los daños causados por el fuego y el agua pueden ser permanentes en los componentes del hardware. Los discos duros podrían fallar o podría decidir sustituirlo por una unidad de disco duro de mayor tamaño.  
  
 En este documento se proporciona información sobre los siguientes temas:  
  
-   [¿Qué es la restauración completa del sistema de equipos?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [¿Cómo funciona el entorno de restauración del sistema?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Creación de una unidad flash USB de arranque para restaurar un equipo cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [Usar el Asistente para restauración completa del sistema](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [¿Dónde puedo encontrar los controladores del hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="what-is-computer-full-system-restore"></a><a name="BKMK_WhatIs"></a>¿Qué es la restauración completa del sistema de equipos?  
 En el caso de sustitución de una unidad de disco duro o si el equipo falla hasta el punto en que no se puede utilizar o no se inicia, puede restaurar el sistema a una copia de seguridad anterior del equipo. Una restauración completa del sistema devuelve el sistema al estado en el que estaba en el momento de la copia de seguridad.  
  
> [!IMPORTANT]
>  No se puede ejecutar una restauración completa del sistema en un equipo con hardware (por ejemplo, una placa del sistema) que no sea similar a del equipo de destino. El sistema operativo instalado depende en gran medida del hardware subyacente del equipo. Sin embargo, puede realizar una restauración completa del sistema a un disco duro que sea del mismo o de mayor tamaño que el del equipo de origen.  
  
 Al realizar una restauración completa del sistema puede seleccionar una copia de seguridad del equipo concreta para restaurar el sistema, con todas las aplicaciones, configuraciones y opciones conocidas por el usuario antes del fallo, catástrofe o robo. También puede seleccionar los volúmenes que desee restaurar.  
  
 Al planificar o preparar la restauración del sistema completo en un equipo de red, tenga en cuenta lo siguiente:  
  
### <a name="windows-preinstallation-environment"></a>Entorno de preinstalación de Windows  
 Entorno de preinstalación de Windows (Windows PE) es un sistema operativo mínimo diseñado para preparar un equipo para la instalación de Windows. En el caso de los servidores que ejecutan Windows Server Essentials, Windows PE se instala automáticamente al insertar el medio de restauración en un equipo que se va a restaurar. En el caso de los servidores que ejecutan Windows Server Essentials, Windows PE se instala automáticamente al iniciar el equipo con el servicio de restauración de cliente o con la unidad flash USB.  
  
 Windows PE no admite conexiones inalámbricas. Debido a ello, el equipo que se va a restaurar debe estar conectado físicamente a la red de la pequeña empresa.  
  
### <a name="bitlocker"></a>BitLocker  
 Cifrado de unidad BitLocker (BitLocker) es una característica de protección de datos que está disponible en algunas versiones de Windows Vista, Windows 7 y Windows 8. BitLocker le protege contra el robo o la exposición de datos en equipos que se hayan extraviado o robado y ofrece una eliminación de datos más segura al retirar los equipos.  
  
 **Para Windows Server Essentials:** Si el equipo que necesita restaurar se cifró mediante BitLocker (si era simplemente la unidad del sistema operativo o la unidad del sistema operativo y una o varias unidades fijas), todavía puede usar los medios de restauración completos del sistema incluidos en el CD proporcionado con el servidor y el Asistente para restauración completa del sistema para volver a instalar la imagen de disco duro, incluido el sistema operativo, desde una copia de seguridad y restaurar los datos en el equipo nuevo o reparado.  
  
 **Para Windows Server Essentials:** Si el equipo que necesita restaurar se cifró con BitLocker (ya sea solo la unidad del sistema operativo o la unidad del sistema operativo y una o varias unidades fijas), puede usar el Asistente para restauración completa del sistema para volver a instalar la imagen del disco duro, incluido el sistema operativo, desde una copia de seguridad y restaurar los datos en el equipo nuevo o reparado.  
  
 Cuando el servidor copia las unidades, las carpetas y los archivos, se guarda una versión sin cifrar en el servidor. Durante una restauración completa del sistema se copia esta versión sin cifrar en el equipo.  
  
> [!NOTE]
>  Después de una restauración completa del sistema correcta, deberá activar BitLocker en el equipo.  
>   
>  Para obtener instrucciones sobre cómo habilitar BitLocker en equipos que ejecutan Windows 8, consulte [BitLocker: Cómo habilitar BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Para obtener instrucciones sobre cómo habilitar BitLocker en equipos que ejecutan Windows 7, consulte [cifrado de unidad BitLocker guía paso a paso de Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Para obtener más información acerca de los conceptos básicos del cifrado de unidad BitLocker, consulte [Preguntas más frecuentes acerca de BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Sistema de cifrado de archivos  
 El sistema de cifrado de archivos (EFS) de Windows ofrece cifrado adicional a nivel de usuario basado en archivo para diferentes niveles de seguridad entre diferentes usuarios del mismo equipo. Es importante tener en cuenta que, a diferencia de las unidades cifradas de BitLocker, las carpetas y los archivos cifrados de EFS siguen estando cifrados en todas las copias de seguridad de un equipo. EFS no está disponible en Windows XP Home Edition, Windows Vista Starter, Windows Vista Home Basic, Windows Vista Home Premium, Windows 7 Starter, Windows 7 Home Basic, Windows 7 Home Premium o Windows 8. Solo está disponible en Windows 8 Pro.  
  
> [!WARNING]
>  A diferencia de BitLocker, solo se puede acceder a archivos protegidos de EFS desde el sistema operativo en que se realizó el cifrado.  
  
### <a name="disk-partitions"></a>Particiones de disco  
 Si el tamaño de la unidad de disco duro del nuevo equipo coincide o es mayor que el de la original, la unidad de disco duro se volverá a formatear y particionar. Consulte los detalles en el gráfico que aparece a continuación:  
  
|Equipo original|Equipo nuevo o restaurado|  
|-----------------------|------------------------------|  
|Un solo disco con varias particiones|Un solo disco con varias particiones y el espacio adicional se asignará a la última partición|  
|Un solo disco con una partición|Un solo disco con una partición y todo el espacio disponible se utiliza para la única partición|  
  
> [!NOTE]
>  Si hay diferencias de tamaño de disco y diseño de partición entre el equipo original y el restaurado, deberá utilizar Administración de discos para crear las particiones correspondientes en el equipo nuevo o restaurado. Para ello puede usar el Asistente para restauración completa del sistema.  
  
### <a name="raid-and-dynamic-disks"></a>Discos RAID y dinámicos  
 La copia de seguridad de matrices redundantes de discos independientes (RAID) y discos dinámicos no es compatible.  
  
##  <a name="how-does-the-system-restore-environment-work"></a><a name="BKMK_HowDoes"></a>¿Cómo funciona el entorno de restauración del sistema?  
 Los medios de restauración del sistema suministrados con Windows Server® 2012 Essentials instala Entorno de preinstalación de Windows (Windows PE) en el equipo. Windows PE sustituye el entorno de MS-DOS y contiene los archivos de programa principales para Windows. En Windows Server Essentials, existen dos formas admitidas para restaurar un sistema: mediante el servicio de restauración de cliente, que usa una red y no se basa en medios, o en el uso de la unidad flash USB.  
  
> [!NOTE]
>  Windows PE no admite conexiones inalámbricas. Debido a ello, el equipo que se va a restaurar debe estar conectado físicamente a la red de la pequeña empresa.  
  
 El entorno de restauración del sistema contiene los archivos de programa para 32 bits (x86) y 64 bits (x64). Después de insertar los medios de restauración del sistema, elija la versión adecuada de los archivos. El valor predeterminado es 32 bits (x86) y se selecciona automáticamente si no elige alguno en 30 segundos. Si hay actualizaciones para los archivos de programa de restauración del sistema completo en el servidor, estos se descargan automáticamente en el equipo.  
  
 Después de configurar el entorno de preinstalación, se inicia el Asistente para restauración completa del sistema. El asistente le ayuda a restaurar el equipo desde una copia de seguridad anterior. También puede utilizar el entorno de restauración del sistema para restaurar una copia de seguridad a un nuevo equipo con hardware similar.  
  
 En la mayoría de los casos, los archivos de programa y controladores contenidos en el entorno de restauración del sistema son todos los que se necesitan para reiniciar el equipo nuevo o restaurado. Dependiendo del hardware del equipo nuevo o restaurado, es posible que el entorno de restauración del sistema no incluya todos los controladores del adaptador de red y de almacenamiento necesarios para reiniciar el equipo restaurado. El Asistente para restauración completa del sistema le dará la oportunidad de instalar los controladores, si es necesario. Para obtener más información sobre cómo encontrar los controladores de hardware, consulte [¿Dónde puedo encontrar los controladores del hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Para obtener información sobre cómo usar los medios de restauración del sistema, vea [Uso del Asistente para restauración completa del sistema](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="create-a-bootable-usb-flash-drive-to-restore-a-client-computer"></a><a name="BKMK_CreateBootable"></a>Creación de una unidad flash USB de arranque para restaurar un equipo cliente  
 Si necesita restaurar un equipo cliente a partir de una copia de seguridad existente pero no encuentra el CD de restauración incluido en el servidor (en Windows Server Essentials) o no quiere configurar el servicio de restauración de cliente en el servidor (en Windows Server Essentials) , puede crear una unidad flash USB de arranque. A continuación, puede utilizar la unidad flash USB para iniciar el equipo cliente y restaurar el sistema. La unidad flash USB que utilice debe tener una capacidad de al menos 1 GB o más.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Para crear una unidad flash USB de arranque  
  
1.  Abra el **Panel**.  
  
2.  Haga clic en la pestaña **Dispositivos**.  
  
3.  En el panel **Tareas** , haga clic en **Personalizar copia de seguridad de equipos y configuración del historial de archivos**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, haga clic en **tareas de copia de seguridad de equipos cliente**.  
  
4.  Haga clic en la pestaña **Herramientas** y, a continuación, haga clic en **Crear clave** en la sección **Recuperación de equipos**. Se abre el asistente Crear la clave de recuperación de equipos.  
  
5.  Inserte una unidad flash USB de 1 GB o más en el servidor y, a continuación, siga las instrucciones del asistente.  
  
    > [!CAUTION]
    >  Se eliminarán todos los datos de la unidad flash USB.  
  
##  <a name="using-the-full-system-restore-wizard"></a><a name="BKMK_Using"></a>Usar el Asistente para restauración completa del sistema  
 Después de usar correctamente el medio de restauración, el servicio de restauración de cliente o la unidad flash USB para iniciar su equipo y comprobar que se han cargado todos los controladores en el equipo cliente nuevo o restaurado, se abrirá el Asistente para restauración completa del sistema. Este asistente le permite acceder al servidor, la copia de seguridad del equipo y los volúmenes de origen que desee restaurar en el equipo y ejecuta el proceso de restauración en sí.  
  
> [!NOTE]
>   Windows Server Essentials no admite los siguientes escenarios de restauración:  
> 
> - Restauración de un disco de registro de arranque maestro (MBR) en un equipo basado en Unified Extensible Firmware Interface (UEFI).  
>   -   Restauración de una copia de seguridad de UEFI/GPT al BIOS.  
> 
>   Si restaura los datos en alguno de estos casos, no podrá arrancar el sistema. Además, es posible que no pueda utilizar unidades de disco duro con un tamaño superior a dos terabytes.  
  
 **Requisitos previos**  
  
-   Antes de iniciar el proceso de restauración de todo el sistema, utilice un cable de red (una conexión por cable) para conectar el equipo a la misma red que el servidor. Compruebe que tiene acceso a todas las unidades de disco duro del equipo cliente.  
  
    > [!WARNING]
    >  No intente realizar una restauración completa del sistema en un equipo que utilice una conexión inalámbrica para conectarse la red.  
  
-   Si sabe que el equipo no dispone de controladores importantes de red o de dispositivos de almacenamiento, deberá localizarlos y copiarlos en una unidad flash antes de iniciar el proceso de restauración completa del sistema. En Windows Server Essentials: Si usa el medio de restauración completa del sistema que se proporciona en el CD, dicho CD debe permanecer en la unidad durante la fase de inicio del proceso de restauración completa del sistema. Por lo tanto, no debe copiar los controladores que faltan en un CD o DVD a menos que tenga una segunda unidad de CD/DVD. En lugar de ello, copie los controladores que faltan en una unidad flash USB.  
  
     Para obtener más información acerca de cómo encontrar los controladores para su equipo, consulte [¿Dónde puedo encontrar los controladores del hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   En Windows Server Essentials: Si no puede encontrar el CD de restauración de Windows Server Essentials, puede crear una unidad flash USB de arranque. Si desea obtener más información, consulte [Crear una unidad flash USB de arranque para restaurar un equipo cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Para usar el Asistente para restauración completa del sistema  
  
1. Realice una de las siguientes acciones:  
  
   -   Windows Server Essentials: Encienda el equipo cliente que desee restaurar, inserte el medio de restauración y, a continuación, apague el equipo.  
  
        Vuelva a encender el equipo y, durante las pruebas automáticas de encendido (POST), pulse la tecla de función correspondiente para acceder al menú del dispositivo de arranque y seleccione la unidad de CD/DVD. Se iniciará el Administrador de arranque de Windows.  
  
   -   Windows Server Essentials: Si usa el servicio de restauración de cliente, reinicie el equipo mediante la opción **arranque desde la red** . De lo contrario, inicie el equipo con la llave USB.  
  
        Vuelva a encender el equipo y, durante la comprobación automática durante el inicio (POST), presione la tecla de función correspondiente para tener acceso al menú del dispositivo de arranque y seleccione **Arranque de red** (o seleccione la opción de arrancar desde la llave USB). Se iniciará el Administrador de arranque de Windows.  
  
   > [!NOTE]
   >  Compruebe la documentación del fabricante de su equipo para determinar qué tecla de función accede al menú del dispositivo de arranque.  
  
2. El medio de restauración de equipos contiene las opciones de arranque para 32 bits (x86) y 64 bits (x64). En el Administrador de arranque de Windows, seleccione **Restauración completa del sistema (x86)** o **Restauración completa del sistema (x64)** . Si los controladores de hardware del equipo son de 32 bits, seleccione x86 y si son de 64 bits seleccione x64. Se cargarán los archivos de Windows y el Asistente para restauración completa del sistema realizará una comprobación para asegurarse de que todos los controladores de hardware estén disponibles.  
  
3. En la ventana **Asistente para restauración completa del sistema** elija su idioma preferido y, a continuación, haga clic en la flecha.  
  
4. Seleccione el **Formato de hora y moneda**, y **Teclado o método de entrada** adecuados para el equipo. Haga clic en **Continuar**.  
  
5. Si faltan controladores, el mensaje del proceso de restauración no puede comprobar que se muestran los controladores. Haga clic en **Cerrar** y a continuación, en el cuadro de diálogo Bienvenida, seleccione **Cargar controladores**.  
  
   1.  En el cuadro de diálogo **Detectar hardware**, haga clic en **Instalar controladores**.  
  
   2.  Inserte la unidad flash USB que contenga los controladores de hardware y, en el cuadro de diálogo **Instalar controladores**, haga clic en **Digitalizar**.  
  
   3.  En el cuadro de diálogo **Instalar controladores**, haga clic en **Aceptar** cuando se encuentren los controladores.  
  
   4.  En el cuadro de diálogo **Detectar hardware**, haga clic en **Continuar**.  
  
6. Si se encontraron todos los controladores durante la comprobación inicial o la instalación de los controladores importantes, haga clic en **Continuar** en la ventana **Restauración completa del sistema**.  
  
7. En la página **Bienvenido al Asistente para restauración completa del sistema**, haga clic en **Siguiente**.  
  
8. El asistente buscará el servidor.  
  
   1.  Si el asistente no puede encontrar el servidor, podrá repetir la búsqueda o introducir la dirección IP del servidor.  
  
   2.  Si se detectan varios servidores, deberá seleccionar uno.  
  
   3.  Si se encuentra el servidor, se muestra la página **iniciar sesión en < nombreservidor\>** .  
  
9. En la página **iniciar sesión en < nombreservidor\>** , escriba *<\>nombredecuentadeadministrador* en el cuadro de texto **nombre de usuario** y la contraseña de la cuenta de administrador en el cuadro de texto **contraseña** y, a continuación, haga clic en **siguiente**.  
  
    > [!IMPORTANT]
    >  Debe usar una cuenta de administrador creada en inglés. Si no dispone de una, debe crear una cuenta nueva de administrador. Para ello, abra la pestaña **Usuarios** en el panel del servidor, establezca el formato de idioma de teclado en inglés y, a continuación, ejecute la tarea **Agregar una cuenta de usuario** para crear la cuenta de administrador. A continuación, use la nueva cuenta de administrador para proseguir con la restauración del equipo cliente.  
  
10. En la página **Seleccione el equipo que desee restaurar**, seleccione el equipo correspondiente y haga clic en **Siguiente**. Puede elegir entre **< ComputerName\>: (este equipo)** o elegir un equipo diferente en la red en la lista desplegable **otro equipo** .  
  
    > [!NOTE]
    >  Si el servidor desconoce el equipo (por ejemplo, un equipo nuevo o reasignado), la opción **Este equipo** no aparecerá.  
  
11. En la página **Seleccionar copia de seguridad para restaurar**, revise la lista de copias de seguridad disponibles y seleccione la que desee restaurar en el equipo.  
  
    > [!NOTE]
    >  Se recomienda seleccionar una copia de seguridad correcta (marcada en verde). Esto le ayudará a garantizar que todos los archivos de datos y del sistema se restauren correctamente.  
  
12. (*Opcional*) Seleccione una copia de seguridad y, a continuación, haga clic en **Detalles** para abrir la página **Detalles de copia de seguridad** y visualizar más información acerca de la copia de seguridad. Use la información de la página **Detalles de copia de seguridad** para comparar varias copias de seguridad y ayudarle a decidir cuál es la mejor opción. Haga clic en **Cerrar** en la página **Detalles de copia de seguridad** para volver a la página **Seleccionar copia de seguridad para restaurar**.  
  
13. En la página **Seleccionar copia de seguridad para restaurar**, seleccione una copia de seguridad y haga clic en el botón **Siguiente**.  
  
14. En la página **Seleccionar opción de restauración**, haga clic en una de las siguientes y pulse **Siguiente**.  
  
    > [!NOTE]
    >  Esta página no se muestra si no se admite la creación automática de particiones.  
  
    1.  **Dejar que el asistente restaure por completo el equipo (recomendado)** . Esta opción le ayuda a garantizar que el equipo se restaure al estado anterior a la fecha y hora de la copia seleccionada. Si selecciona esta opción, pase al paso 15.  
  
    2.  **Dejarme seleccionar los volúmenes a restaurar (Avanzado)** . Esta opción le permite seleccionar los volúmenes que desea restaurar y la ubicación. También puede crear particiones en el disco duro.  
  
15. En la página **Seleccionar los volúmenes a restaurar** puede seleccionar los volúmenes que desee restaurar.  
  
    > [!NOTE]
    >  Esta página se muestra si hay varias unidades de disco duro en el equipo de origen de copia de seguridad, o si la unidad de destino de restauración tiene menos espacio de almacenamiento que la unidad de origen de la copia de seguridad.  
  
    1. El asistente intentará que los volúmenes de origen y destino coincidan. Compruebe que la asignación predeterminada sea correcta.  
  
       1.  Para anular la selección de un volumen, haga clic en la flecha de menú de listado del volumen y seleccione **Ninguno**.  
  
       2.  Cuando termine de seleccionar los volúmenes, haga clic en **Siguiente**.  
  
    2. Si el volumen de origen y de destino son del mismo tamaño o el de origen es de menor tamaño que el de destino, aparecerá una flecha verde entre ambos. Si no coincide el tamaño de los volúmenes (si el volumen de origen es mayor que el destino), aparecerá una X roja entre el origen y el destino.  
  
       > [!NOTE]
       >  Una X roja puede aparecer también si:  
       > 
       > - El tamaño de sector de disco del volumen de origen no coincide con el tamaño de sector de disco del volumen de destino. Esto puede ocurrir si se reemplaza el disco físico por un disco con un tamaño de sector diferente o si configura los espacios de almacenamiento (que pueden tener un tamaño de sector diferente al del disco físico).  
       >   -   Ha alcanzado el límite del número de clústeres. Para restaurar el volumen de origen al volumen de destino, debe formatear el volumen de destino con el mismo tamaño de clúster que el volumen de origen. Si el volumen de destino es demasiado grande, y el tamaño del clúster es demasiado pequeño, puede alcanzar el límite del número de clústeres.  
  
       1. Haga clic en **Ejecutar Administrador de discos (avanzado)** y cree un nuevo volumen del mismo tamaño que el volumen de reserva del sistema.  
  
          > [!NOTE]
          >  Si un equipo cliente se basa en Unified Extensible Firmware Interface (UEFI), debe usar la herramienta **DiskPart** para inicializar el disco del sistema. Para ello, abra una ventana de comandos (presione Ctrl + Alt + Mayús durante 5 segundos en el entorno WinPE), ejecute **diskpart.exe** y, a continuación, los siguientes comandos de Diskpart:  
          > 
          > 1. **Disco de lista de > de Diskpart**  
          >    2. **DiskPart > Seleccionar disco #** *< disco\>*  
          >    3. **DiskPart > Clean**  
          >    4. **DiskPart > convertir GPT**  
          >    5. **DiskPart > Create Partition EFI size =** *100* (donde *100* es un tamaño de partición de ejemplo en MB, debe ser el mismo que el de la partición original)  
          >    6. **DiskPart > Create Partition MSR size =** *128* (donde *128* es un tamaño de partición de ejemplo en MB, debe ser el mismo que el de la partición original)  
          >    7. **Salida de Diskpart >**  
  
       2. *(Opcional)* Seleccione la opción **No asignar una letra o ruta de acceso de unidad**.  
  
       3. Formatee el volumen como **NTFS**.  
  
       4. Una vez formateado el volumen, haga clic con el botón secundario en el volumen del sistema y, a continuación, haga clic en **Marcar partición como activa**.  
  
       5. Si necesita que otros volúmenes se correspondan con volúmenes de la copia de seguridad, repita los pasos de *ii* al *iv* para crear y activar los volúmenes y a continuación cierre **Administración de discos**.  
  
       6. En la página **Seleccionar los volúmenes a restaurar**, asigne el volumen reservado del origen de copia de seguridad al volumen del mismo tamaño que creó en el paso *v*.  
  
       7. Asigne el resto de los recursos con los volúmenes de destino correspondientes.  
  
       8. Haga clic en **Siguiente** para continuar con la restauración.  
  
16. En la página **Confirmar volúmenes a restaurar**, revise la asignación y haga clic en **Siguiente**. Si necesita realizar más cambios, haga clic en **Atrás** y repita el paso 14.  
  
17. En la página **restaurar < nombredeequipo\> de < fecha y hora de la copia de seguridad\>** se informa del progreso del proceso de restauración.  
  
18. En la página **Restauración completada correctamente**, retire el medio de restauración y haga clic en **Finalizar**. Se reiniciará el equipo.  
  
    > [!IMPORTANT]
    >  Si ha habilitado el Cifrado de unidad BitLocker antes de la restauración, deberá habilitar BitLocker manualmente cuando se reinicie el equipo.  
  
##  <a name="where-can-i-find-the-drivers-for-my-hardware"></a><a name="BKMK_FindDrivers"></a>¿Dónde puedo encontrar los controladores del hardware?  
 Dependiendo del hardware del equipo nuevo o restaurado, es posible que el medio de restauración no incluya todos los controladores del adaptador de red y de almacenamiento necesarios para reiniciar el equipo restaurado. Debe determinar qué controladores faltan, ubicarlos en un medio existente o en el sitio web del fabricante, copiarlos en una unidad flash y, a continuación, copiarlos de la unidad flash al equipo nuevo o restaurado al ejecutar el Asistente para restauración completa del sistema.  
  
 Al realizar una copia de seguridad de un equipo, los controladores del equipo se guardan en la copia de seguridad. Si el medio de recuperación no incluye todas las unidades que necesite, puede abrir una copia de seguridad del equipo y copiar los controladores a una unidad flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Para copiar controladores desde una copia de seguridad a una unidad flash USB  
  
1. Abra el Panel desde otro equipo.  
  
2. Haga clic en **Dispositivos** y, a continuación, en el equipo para el que necesite los controladores.  
  
3. Haga clic en **Restaurar archivos o carpetas para el equipo**. Se abre el Asistente de restauración de archivos o carpetas.  
  
4. Haga clic en la copia de seguridad correcta más reciente y a continuación haga clic en **Siguiente**.  
  
5. Haga clic en un volumen para abrirlo y pulse **Siguiente**. Se abrirá una ventana con la lista de archivos y carpetas de la copia de seguridad.  
  
6. Introduzca la unidad flash USB en un conector USB del equipo y copie la carpeta de controladores para la restauración completa del sistema en la unidad flash USB.  
  
   > [!NOTE]
   >  Es posible que necesite hacer clic en **Subir un nivel** hasta que alcance el directorio raíz del volumen del sistema.  
  
7. Retire la unidad flash y a continuación introdúzcala en el equipo que vaya a restaurar.  
  
   Puede utilizar la unidad flash USB para instalar los controladores del equipo para completar la restauración. El Asistente de restauración de archivos o carpetas busca controladores adicionales en la unidad flash USB al usar el Asistente para restauración completa del sistema. Muy probablemente, los controladores que necesite son los del adaptador de red y el dispositivo de almacenamiento.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
