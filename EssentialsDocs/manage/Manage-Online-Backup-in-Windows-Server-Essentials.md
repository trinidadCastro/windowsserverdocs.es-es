---
title: Administrar copias de seguridad en línea en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890726"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Administrar copias de seguridad en línea en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Después de integrar con copia de seguridad de Microsoft Azure, el **Online Backup** aparecerá la página de administración en el panel de Windows Server Essentials. La página **Copia de seguridad en línea** permite realizar tareas administrativas habituales. Las características en la página de copia de seguridad en línea incluyen:  
  
-   Las siguientes páginas de subsección:  
  
    -   **Copia de seguridad en línea** Después de registrar el servidor para realizar copias de seguridad en línea, en esta sección se muestra el estado actual de la copia de seguridad, el estado de almacenamiento y la información de la cuenta.  
  
    -   **Carpetas protegidas** esta sección enumeran todas las carpetas compartidas y todas las carpetas del historial de archivos en el servidor y todas las carpetas que ha elegido realizar copias de seguridad en Azure. La lista incluye el nombre, la ruta de acceso y el estado de cada carpeta compartida.  
  
    -   **Historial de copias de seguridad** En esta sección se muestra una lista de copias de seguridad en línea recientes. La lista incluye el tipo de operación, la hora y el estado de cada copia de seguridad.  
  
-   Un panel de detalles con información adicional sobre una determinada tarea de copia de seguridad o restauración.  
  
-   Un panel de tareas que incluye un conjunto de tareas administrativas que puede realizar.  
  
 Le recomendamos realizar una copia de los datos más importantes de la empresa y de usuario. Por ejemplo, debería copiar las carpetas del servidor que contengan archivos de datos importantes. También debe copiar el historial de archivos para los equipos de la red que contengan información importante.  
  
 Al decidir de qué datos quiere hacer una copia de seguridad en línea, piense con qué frecuencia quiere realizarla y que directiva de retención desea aplicar. A continuación, revise su presupuesto y decida la cantidad de espacio de almacenamiento que se puede permitir. Analice el coste y el volumen de almacenamiento teniendo en cuenta sus necesidades y, a continuación, configure copias de seguridad en línea para proteger la mayor cantidad posible de datos importantes. Para obtener más información sobre precios, consulte [Detalles de precios de Copia de seguridad de Azure](https://azure.microsoft.com/pricing/details/backup/).  
  
 En las siguientes secciones se describen diversas tareas de copia de seguridad en línea que pueden aparecer en el panel de Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Tareas de copia de seguridad en línea en el panel  
  
-   [Cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Iniciar una copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Restaurar archivos y carpetas desde una copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Registrar este servidor para copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Anular el registro de servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a> Cargar un certificado en el almacén de copia de seguridad de Azure  
 Para poder usar copia de seguridad de Azure para copias de seguridad en línea en Windows Server Essentials, debe cargar un certificado público para registrarlo con el almacén de copia de seguridad. El certificado se usa para autenticar la implementación de copia de seguridad de Azure (agente), que actúa en nombre del propietario de la suscripción de Microsoft Online Services para administrar los recursos asociados a la suscripción.  
  
> [!NOTE]
>  Para cargar el certificado, debe completar los procedimientos indicados en [Sign up for Azure Backup Service](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Para cargar un certificado y usarlo con el servicio de Azure Backup  
  
1.  Inicie sesión en el panel de Windows Server Essentials con una cuenta de administrador.  
  
2.  En el panel **Página principal** , haga clic en **COPIA DE SEGURIDAD EN LÍNEA**.  
  
3.  En la sección **Copia de seguridad en línea**, haga clic en **Cargar certificado en el almacén de Azure Backup**.  
  
     Se abrirá **Recovery Services** en el Portal de administración de Azure. Si t de se ha iniciado sesión en Azure, deberá iniciar sesión con su cuenta de Microsoft.  
  
4.  Haga clic en el nombre del almacén de copias de seguridad en línea que usará para abrir la página **Inicio rápido** del almacén de copias de seguridad.  
  
5.  Haga clic en **Administrar certificado**.  
  
6.  En el cuadro de diálogo **Administrar certificado** , pegue la ruta de acceso del certificado público generado con Windows Server Essentials. Para obtener la ruta de acceso del certificado público, haga lo siguiente:  
  
    1.  En el panel de Windows Server Essentials, haga clic en la pestaña **Copia de seguridad en línea** .  
  
    2.  En la página **Copia de seguridad en línea**, copie la ruta de acceso del certificado generado.  
  
    3.  Conmutador en el Portal de administración de Azure y, a continuación, en el **administrar certificado** diálogo cuadro, pegue la ruta de acceso para cargar el certificado público generado.  
  
    > [!NOTE]
    >  También puede usar su propio certificado público. Para saber qué certificado es necesario, en la página **Inicio rápido**, haga clic en el vínculo **Adquirir certificado**.  
  
    > [!NOTE]
    >   Azure requiere un certificado x.509 v2 con una clave pública. Para obtener más información, consulte [Administrar certificados de almacén](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7.  Después de seleccionar el certificado, haga clic en **Aceptar** (marca de comprobación).  
  
8.  Vuelva a la página **Inicio rápido** . Haga clic en **Panel** y compruebe que el certificado se cargue correctamente. Una vez que el certificado se carga correctamente, el panel muestra la huella digital y la fecha de expiración del certificado.  
  
 Después de completar este procedimiento, haga lo siguiente:  
  
1.  Registre el servidor con el servicio de copia de seguridad de Azure. Para obtener más información, vea cómo [registrar este servidor para realizar copias de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
2.  Configure las copias de seguridad en línea del servidor. Para obtener más información, consulte [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_2"></a> Configurar copia de seguridad en línea  
 Después de registrar el servidor con copia de seguridad de Azure, puede configurar la configuración de copia de seguridad en línea en Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Para configurar la copia de seguridad en línea  
  
1.  En el Asistente para registrar el servidor o en la página **Copia de seguridad en línea** del panel de Windows Server Essentials, haga clic en **Configurar copia de seguridad en línea**. Se abrirá el Asistente para configurar copias de seguridad en línea.  
  
2.  En el **Configurar copia de seguridad en línea** página del asistente, seleccione la casilla de cada carpeta del servidor que desea realizar copias de seguridad en Azure Backup. Desactive la casilla de verificación de cada carpeta del servidor que no desee incluir en la copia de seguridad. Para agregar carpetas que no se muestran en la lista, haga clic en **Agregar carpetas**. Cuando haya terminado de realizar la selección, haga clic en **Siguiente**.  
  
    > [!NOTE]
    >  No puede seleccionar una carpeta que no se encuentre en el servidor local o una carpeta que se encuentre en una unidad con formato ReFS.  
  
3.  En la página **Agregar copias de seguridad del Historial de archivos** del asistente, puede incluir las copias de seguridad del historial de archivos de los equipos de red que admitan la característica Historial de archivos. Haga clic en **Siguiente** para continuar.  
  
4.  En la página **Especificar la programación de copias de seguridad** del asistente, configure lo siguiente y, a continuación, haga clic en **Siguiente** para continuar:  
  
    -   Seleccione o acepte la hora del día en que desea ejecutar la copia de seguridad en línea. La hora predeterminada es 10:00 p. m. También puede especificar una hora para ejecutar una segunda copia de seguridad.  
  
    -   Seleccione o acepte los días en los que desee ejecutar la copia de seguridad en línea. De forma predeterminada, se ejecuta de lunes a viernes.  
  
5.  En la página **Especificar la directiva de retención de copia de seguridad** del asistente, seleccione el número de días que desee conservar las copias de seguridad en línea y, a continuación, haga clic en **Siguiente**. El valor predeterminado es de 7 días. También puede conservar las copias de seguridad en línea durante 15 o 30 días.  
  
    > [!NOTE]
    >   Azure Backup siempre conserva la copia de seguridad más reciente. Si el destino de la copia de seguridad no tiene suficiente espacio disponible para almacenarla, el proceso de copia de seguridad no se realizará correctamente. Para evitar esta situación, adquiera espacio de almacenamiento adicional o reduzca el período de retención de datos. Para obtener más información, consulte [detalles de precios](https://azure.microsoft.com/pricing/details/backup/) para Microsoft Azure Backup.  
  
6.  En la página **Elija su uso de ancho de banda** del asistente, si desea restringir la cantidad de ancho de banda de Internet asignada a la copia de seguridad en línea, seleccione **Activar el uso de ancho de banda de Internet**. Use las opciones de la página para especificar cuánto ancho de banda de Internet desea habilitar para la copia de seguridad en línea durante el horario laborable y no laborable. A continuación, defina las horas y los días laborables.  
  
    > [!NOTE]
    >   Azure Backup le permite personalizar cómo el software de integración utiliza el ancho de banda de red al realizar copias de seguridad o restaurar información. Usa una técnica que se conoce comúnmente como la limitación, puede controlar la cantidad de ancho de banda de red que está disponible para su uso por la copia de seguridad y restaurar los procesos durante el día concreto e intervalos de tiempo. Después de seleccionar la casilla **Activar el uso de ancho de banda de Internet** en el Asistente para configurar copias de seguridad en línea, puede especificar los días y las horas laborables durante los que se aplicará el límite de ancho de banda de horario laborable. El límite de horario no laborable se aplicará el resto del tiempo. En ambos límites, los intervalos de ancho de banda pueden estar comprendidos entre 256 KB/s y una cantidad ilimitada.  
  
7.  Cuando finalice la configuración de copia de seguridad en línea, haga clic en **Cerrar**.  
  
    > [!TIP]
    >  Después de configurar la copia de seguridad, la página **Copia de seguridad en línea** muestra el estado de la última copia de seguridad en línea y la cantidad de espacio de almacenamiento utilizado en el almacén de copias de seguridad. Para ver el estado de las copias de seguridad anteriores, haga clic en **Historial de copias de seguridad**.  
  
###  <a name="BKMK_3"></a> Iniciar una copia de seguridad en línea  
  
> [!NOTE]
>  Antes de iniciar una copia de seguridad en línea, debe [registrar este servidor para realizar copias de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5) y, a continuación, [configurar la copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Para iniciar una copia de seguridad en línea inmediatamente  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En el panel **Tareas de copia de seguridad en línea**, haga clic en **Iniciar copia de seguridad ahora**.  
  
###  <a name="BKMK_4"></a> Restaurar archivos y carpetas desde una copia de seguridad en línea  
 El Asistente de restauración de archivos y carpetas le ayuda a localizar, seleccionar y restaurar los archivos y las carpetas que se copian en línea. En los pasos del asistente, realizará las tareas siguientes:  
  
1.  **Elija una copia de seguridad en línea desde el que se va a restaurar archivos y carpetas**  
  
     Al ejecutar el Asistente de restauración de archivos y carpetas, lo primero que debe hacer es especificar si desea restaurar archivos de una copia de seguridad almacenada en un servidor en el que haya iniciado sesión o en un servidor diferente.  
  
2.  **Elegir un servidor desde el que se va a restaurar archivos y carpetas**  
  
     Si decide restaurar archivos y carpetas desde un servidor diferente, seleccione el servidor desde el que desea restaurar los archivos o las carpetas en la lista de servidores disponibles. A continuación, haga clic en **Siguiente**.  
  
3.  **Elija un volumen que contiene los archivos y carpetas que se va a restaurar**  
  
     Las copias de seguridad en línea contienen volúmenes que corresponden a los volúmenes del servidor de copia de seguridad de origen. Después de seleccionar el volumen, seleccione la fecha y la hora de la copia de seguridad desde la que desea restaurar y, a continuación, haga clic en **Siguiente**.  
  
4.  **Seleccionar archivos y carpetas para restaurar**  
  
     En la página **Seleccionar elementos para la restauración** del asistente, puede abrir las distintas ubicaciones de carpeta y seleccione la casilla de cada archivo o carpeta que quiera restaurar. Cuando haya terminado, haga clic en **Siguiente**.  
  
5.  **Especificar la ubicación de destino de archivos y carpetas restaurados**  
  
     La página **Especificar opciones de restauración** del asistente le permite especificar dónde desea restaurar los archivos y carpetas. Hay dos opciones:  
  
    -   **Restaurar en la ubicación original**. Seleccione esta opción para restaurar archivos y carpetas en la ubicación exacta en la que se originó la copia de seguridad.  
  
    -   **Restaurar en una ubicación diferente**.  Elija esta opción para especificar una ubicación diferente del servidor en el que se van a restaurar los archivos.  
  
         En la instalación predeterminada, si ya existe un archivo con el mismo nombre que un archivo de restauración en la ubicación de destino, el asistente crea una copia del archivo original. Además, la lista de control de acceso (ACL) se actualiza para reflejar los permisos de archivo actuales. Puede hacer clic en **Opciones avanzadas** para personalizar la configuración predeterminada.  
  
6.  **Confirme la información de restauración**  
  
     La página **Confirmar la información de restauración** proporciona un resumen de las instrucciones de restauración que ha especificado. Para continuar con la restauración de archivos, debe escribir la frase de contraseña correcta de la cuenta de copia de seguridad en línea.  
  
###  <a name="BKMK_5"></a> Registrar este servidor para copia de seguridad  
 Para realizar copias de seguridad o restaurar archivos, carpetas y el historial de archivos en el servidor de Windows Server Essentials en Azure Backup, primero debe registrar el servidor con el servicio de copia de seguridad de Microsoft Azure.  
  
> [!NOTE]
>  Antes de registrar el servidor, debe cargar un certificado para usarlo en el almacén de copias de seguridad en el que se almacenarán las copias de seguridad en línea. Para obtener más información, vea cómo [cargar un certificado en el almacén de Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Para registrar el servidor en Copia de seguridad de Azure  
  
1.  Inicie sesión en el servidor como administrador y, después, abra el panel.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En la subsección **Copia de seguridad en línea**, haga clic en **Registrar**.  
  
4.  Seleccione el certificado que quiera usar con el almacén de copias de seguridad en línea. El certificado predeterminado está seleccionado de forma predeterminada. Si desea elegir un certificado diferente, use **Examinar**. Haga clic en **Siguiente**.  
  
5.  Siga las instrucciones del asistente para crear una frase de contraseña y, a continuación, completar el registro.  
  
###  <a name="BKMK_6"></a> Anular el registro de servidor  
  
> [!CAUTION]
>  Si anula el registro de su servidor de Windows Server Essentials desde el servicio de copia de seguridad de Microsoft Azure, Azure puede ya no realizar una copia de seguridad del servidor. Además, se borrarán los datos del servidor que se hayan cargado antes. Para reanudar las copias de seguridad en línea, debe registrar el servidor de nuevo.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Para anular el registro de su servidor en Copia de seguridad de Azure  
  
1.  Inicie sesión en el [Portal de administración de Azure](https://manage.windowsazure.com).  
  
2.  Haga clic en **Servicios de recuperación**.  
  
3.  En **Servicios de recuperación**, haga clic en el nombre del almacén de copias de seguridad.  
  
4.  En la página **Inicio rápido** del almacén, haga clic en **Servidores**.  
  
5.  Seleccione el servidor, haga clic en **Eliminar** y, a continuación, haga clic en **Sí** en el mensaje de confirmación.  
  
## <a name="related-tasks"></a>Tareas relacionadas  
  
-   [Cambiar la directiva de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ver el uso de almacenamiento de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Incluir una carpeta nueva en la copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Quitar o excluir las copias de seguridad del historial de archivos de la directiva de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Deshabilitar o volver a habilitar la copia de seguridad de servidor en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Detener una copia de seguridad de servidor en línea en curso](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Ver el estado de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Ver y administrar alertas de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Restablecer configuración predeterminada de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Registrarse para el servicio de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Integrar Azure Backup con Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Proteger las carpetas de copia de seguridad en línea en Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Historial de copia de seguridad en línea en Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a> Cambiar la directiva de copia de seguridad en línea  
 Es fácil realizar cambios en la directiva de copias de seguridad en línea con el panel de Windows Server Essentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Para cambiar la directiva de copias de seguridad en línea  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En el panel **Tareas de copia de seguridad en línea** , haga clic en **Configurar copia de seguridad en línea**.  
  
4.  Siga las instrucciones del asistente para personalizar la directiva de copias de seguridad en línea.  
  
 Para obtener más información sobre las opciones de configuración que pueden personalizarse, vea el tema sobre la [configuración de copias de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_8"></a> Ver el uso de almacenamiento de copia de seguridad en línea  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Para ver la cantidad de espacio de almacenamiento que usa la copia de seguridad en línea  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En la pestaña **Copia de seguridad en línea** , aparece el **Estado del almacenamiento** en el panel de información.  
  
###  <a name="BKMK_9"></a> Incluir una carpeta nueva en la copia de seguridad en línea  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Para incluir una carpeta nueva en la directiva de copias de seguridad en línea  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En el panel **Tareas de copia de seguridad en línea** , haga clic en **Configurar copia de seguridad en línea**.  
  
4.  En la página **Modificar o quitar la directiva de copia de seguridad** , haga clic en **Personalizar la directiva de Copia de seguridad en línea**.  
  
5.  En la página **Configurar copia de seguridad en línea**, si la carpeta que desea incluir no aparece en la lista, haga clic en **Agregar carpetas**.  
  
6.  En la ventana **Agregar carpetas** , busque y seleccione la carpeta que desea incluir en la copia de seguridad en línea y, después, haga clic en **Aceptar**.  
  
7.  En la página **Configurar copia de seguridad en línea** , seleccione otras carpetas que desee incluir y, a continuación, haga clic en **Siguiente**.  
  
8.  Siga las instrucciones del asistente para finalizar la personalización de la directiva de copias de seguridad en línea.  
  
 Para obtener más información acerca de otras opciones de configuración que se pueden personalizar, consulte [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_10"></a> Quitar o excluir las copias de seguridad del historial de archivos de la directiva de copia de seguridad en línea  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Para quitar o excluir una carpeta de la directiva de copias de seguridad en línea  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  Haga clic en la pestaña **Carpetas protegidas**. En el panel de detalles se muestra una lista de carpetas con su estado de copia de seguridad en línea.  
  
4.  Seleccione la carpeta que desea excluir de la directiva de copias de seguridad en línea y, después, en el panel de tareas, haga clic en **Quitar la carpeta de la copia de seguridad en línea**.  
  
###  <a name="BKMK_11"></a> Deshabilitar o volver a habilitar la copia de seguridad de servidor en línea  
 Para obtener instrucciones sobre cómo usar Azure Backup para realizar copias de seguridad o restaurar los datos de servidor, consulte [registrar este servidor para copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Para obtener instrucciones sobre cómo dejar de usar Azure Backup para realizar copias de seguridad o restaurar los datos de servidor, consulte [Unregister server](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="BKMK_12"></a> Detener una copia de seguridad de servidor en línea en curso  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Para detener una copia de seguridad en curso del servidor en línea  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  En el panel **Tareas de copia de seguridad en línea** , haga clic en **Detener la copia de seguridad**.  
  
     Después de detener la copia de seguridad, aparece el estado **Cancelada** en la lista **Historial de copias de seguridad** .  
  
###  <a name="BKMK_13"></a> Ver el estado de copia de seguridad en línea  
  
##### <a name="to-view-the-backup-status"></a>Para ver el estado de la copia de seguridad  
  
1.  Inicie sesión en el panel como administrador.  
  
2.  En la barra de navegación, haga clic en **Copia de seguridad en línea**.  
  
3.  Haga clic en la pestaña **Historial de copias de seguridad** . La vista de lista muestra el estado de cada tarea de copia de seguridad. Seleccione una tarea de copia de seguridad para ver información adicional sobre esa tarea concreta.  
  
###  <a name="BKMK_14"></a> Ver y administrar alertas de copia de seguridad en línea  
 Al igual que muchas otras alertas, alertas de copia de seguridad de Azure se muestran en el Visor de alertas.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Para ver las alertas de copia de seguridad en línea en el Visor de alertas  
  
1.  Abra el Panel.  
  
2.  Realiza una de las siguientes acciones:  
  
      Windows Server Essentials: En el panel de navegación, haga clic en el icono alertas \(puede ser crítico, advertencia o informativa\). Se abrirá el Visor de alertas.  
  
      Windows Server Essentials: En la **Página principal**, haga clic en la pestaña **Seguimiento de estado**.  
  
3.  Revise la lista de alertas para problemas relacionados con la copia de seguridad de Azure.  
  
 Para obtener más información sobre cómo usar el Visor de alertas o la supervisión del estado de la pestaña para administrar las alertas, consulte [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_15"></a> Restablecer configuración predeterminada de copia de seguridad en línea  
 Windows Server Essentials proporciona un asistente que ayuda a configurar las opciones de copia de seguridad en línea. Si desea restaurar la configuración predeterminada, ejecute la tarea **Configurar copia de seguridad en línea** y luego seleccione la opción **Quitar la directiva de Copia de seguridad en línea**. A continuación, ejecute la tarea **Configurar copia de seguridad en línea** de nuevo. Los datos cargados anteriormente no cambiarán.  
  
###  <a name="BKMK_16"></a> Registrarse para el servicio de copia de seguridad de Azure  
 Para preparar la integración de Microsoft Azure Backup con Windows Server Essentials, deberá iniciar sesión en el Portal de administración de Azure con su cuenta de Microsoft Online Services y, a continuación, cree un almacén de copia de seguridad para almacenar las copias de seguridad en línea en Azure. A continuación, podrá descargar el módulo de integración de copia de seguridad de Azure y usar el archivo descargado para instalar el complemento de copia de seguridad de Azure en el servidor de Windows Server Essentials. Si no tiene una cuenta de Microsoft, puede registrarse para usar una versión de prueba gratuita.  
  
 Para realizar esta configuración, complete las siguientes tareas:  
  
1.  Regístrese para obtener una cuenta de Microsoft Online Services y usar la vista previa de copias de seguridad.  
  
2.  Cree un almacén para las copias de seguridad en línea.  
  
3.  Descargue al agente de copia de seguridad de Azure.  
  
4.  Instalar el complemento de copia de seguridad de Azure en el servidor.  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a> Registrarse para obtener una cuenta de Microsoft Online Services y la versión preliminar de la copia de seguridad  
  
1.  Inicie sesión en el panel de Windows Server Essentials.  
  
2.  En la **Página principal** del panel, haga clic en la categoría **Complementos**, en **Integrar con Copia de seguridad de Azure** y, a continuación, en **Haga clic para suscribirse a Windows Azure Backup**.  
  
3.  En Azure **Recovery Services** página, en el **(versión preliminar) de la copia de seguridad** sección, revise los detalles.  
  
4.  Si no tiene una suscripción de Azure, haga clic en **gratuita**y, a continuación, siga las instrucciones para obtener una suscripción de Azure.  
  
     Si ya tiene una suscripción de Azure, haga clic en **Portal** en la esquina superior derecha de la página web para ir al Portal de administración de Azure.  
  
5.  En la página de Portal de administración de Azure, verá **Recovery Services** en el panel izquierdo. S donde ll administrar la copia de seguridad de los almacenes ese almacén las copias de seguridad en línea de Windows Server Essentials.  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a> Crear un almacén de copia de seguridad para almacenar las copias de seguridad en línea  
  
1.  Inicie sesión en el [portal de administración de Azure](https://manage.windowsazure.com)desde el explorador web en el servidor de Windows Server Essentials.  
  
2.  En el Portal de administración de Azure, haga clic en **New**, haga clic en **Data Services**, haga clic en **Recovery Services**, haga clic en **almacén de Backup**y, a continuación, Haga clic en **creación rápida**.  
  
3.  Escriba un nombre para el almacén de copias de seguridad, elija la región donde quiera almacenar las copias de seguridad y, a continuación, haga clic en **Creación rápida**.  
  
     Se abrirá la sección **Servicios de recuperación** mostrando el nuevo almacén.  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a> Descargue al agente de copia de seguridad de Azure  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la **Página principal**, haga clic en la pestaña **Comenzar** y luego en la categoría **Complementos**, en **Integrar con Copia de seguridad de Azure** y en **Haga clic para descargar el módulo de integración de Windows Azure Backup**.  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a> Instalar el complemento de copia de seguridad de Azure en el servidor  
  
1.  Inicie sesión en el servidor con una cuenta de administrador y, a continuación, ejecute el archivo **OnlineBackupAddin.wssx** descargado en el paso anterior.  
  
2.  Se mostrarán los **Términos de licencia de software**. Si está de acuerdo con los términos y condiciones, haga clic en **Aceptar** para continuar con la instalación.  
  
3.  En la página **Instalar el complemento** , haga clic en **Instalar el complemento**.  
  
    > [!NOTE]
    >  Es posible que deba reiniciar el servidor para instalar el software requerido.  
  
     Se abrirá la página **Instalación**. Aparecerá un indicador de progreso de instalación cuando esta se inicie. Una vez completada la instalación, recibirá un mensaje que el complemento Azure Backup se instaló correctamente.  
  
4.  Haga clic en **Finalizar**.  
  
5.  Cierre el panel y vuelva a abrirlo.  
  
     Verá en el panel la nueva pestaña **Copia de seguridad en línea**. En esta pestaña, puede registrar el servidor, configurar opciones de copia de seguridad y abrir **Recovery Services** en el Portal de administración de Azure para administrar los almacenes de copia de seguridad de los servidores.  
  
 Después de completar estos pasos, haga lo siguiente:  
  
1.  En Windows Server Essentials, cargar un certificado que se utilizará para las copias de seguridad en línea. Para obtener más información, vea cómo [cargar un certificado en el almacén de Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
2.  Registrar el servidor con el almacén de copia de seguridad de Azure. Para obtener más información, vea cómo [registrar este servidor para realizar copias de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
3.  Configure las copias de seguridad en línea del servidor. Para obtener más información, consulte [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_17"></a> Integrar Azure Backup con Windows Server Essentials  
 El módulo de integración de Microsoft Azure Backup es una característica nueva de Windows Server Essentials que le permite cifrar y realizar copias de seguridad de archivos y carpetas desde el servidor a un sistema de almacenamiento hospedado de Azure proporcionado por Microsoft. Con Azure Backup para cifrar y hacer una copia de seguridad de los datos en el servidor, puede ayudar a evitar la pérdida grave de datos críticos de empresa a causa de incendios, inundaciones, robos u otros desastres. Al usar Azure Backup para realizar una copia de seguridad de los datos del servidor, la información se cifra con la frase de contraseña antes de cargarlos en un centro de datos segura en Internet. Para acceder a los datos de una copia de seguridad en línea, debe tener un servidor que se autentique mediante un certificado. Además, debe proporcionar la frase de contraseña.  
  
 Después de integrar y registrar el servidor con copia de seguridad de Azure, puede configurar la configuración de copia de seguridad en línea para realizar copias de seguridad programadas con regularidad. También puede iniciar una copia de seguridad en línea en cualquier momento haciendo clic en la tarea **Iniciar copia de seguridad ahora** en el panel de copia de seguridad en línea.  
  
 La configuración de copia de seguridad en línea incluye los siguientes pasos:  
  
1.  [Registrarse para el servicio de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) : después de iniciar suscribirse al servicio de copia de seguridad, crear un almacén de copia de seguridad, descargará e instalará el módulo de integración de Azure Backup.  
  
2.  [Cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Registrar este servidor para copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure Backup usa la frase de contraseña para cifrar archivos y carpetas de la copia de seguridad en línea. Cambiar la frase de contraseña de cifrado reemplazará la frase de contraseña que especificó al registrar el servidor. La frase de contraseña acepta únicamente caracteres codificados en ASCII.  
  
###  <a name="BKMK_18"></a> Proteger las carpetas de copia de seguridad en línea en Windows Server Essentials  
 En la subsección **Carpetas protegidas** de la sección de copias de seguridad en línea del panel aparece una lista de todas las carpetas compartidas en el servidor. En la tabla siguiente se describe la información incluida en la lista.  
  
|Column|Descripción|  
|------------|-----------------|  
|**Nombre de carpeta:**|El nombre de la carpeta que se incluye en la copia de seguridad en línea.<br /><br /> Para agregar o excluir una carpeta, ejecute la tarea **Configurar copia de seguridad en línea**.|  
|**Ruta de acceso de carpeta:**|La ubicación de la carpeta.|  
|**Estado:**|Hay tres tipos de estado **Protected**, **no protegido**, y **desconocido**.|  
  
###  <a name="BKMK_19"></a> Historial de copia de seguridad en línea en Windows Server Essentials  
 En la subsección **Historial de copias de seguridad** de la sección de copias de seguridad en línea del panel aparece una lista de las copias de seguridad en línea más recientes. Puede usar copias de seguridad correctas para restaurar archivos y carpetas. En la tabla siguiente se describe la información incluida en la lista.  
  
|Column|Descripción|  
|------------|-----------------|  
|**Operación:**|Hay dos tipos de operación: **Copia de seguridad** y **Restaurar**.|  
|**hora:**|La hora que se registra para el estado más reciente.|  
|**Estado:**|Hay cinco tipos de estado **éxito**, **en curso**, **Canceled**, **advertencia**, y **incorrecto**.|  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
