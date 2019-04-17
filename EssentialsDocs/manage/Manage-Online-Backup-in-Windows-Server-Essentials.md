---
title: "Administrar la copia de seguridad en línea en Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Administrar la copia de seguridad en línea en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Después de integrar con la copia de seguridad de Microsoft Azure, la **copia de seguridad en línea** aparecerá la página de administración en el escritorio de Windows Server Essentials. La **copia de seguridad en línea** página hace posible realizar tareas de administración comunes. Las características en la página de copia de seguridad en línea incluyen:  
  
-   Las páginas de sección secundarias siguientes:  
  
    -   **Copia de seguridad en línea** después de registrar el servidor para la copia de seguridad en línea, en esta sección muestra el estado actual de copia de seguridad, el estado de almacenamiento y la información de cuenta.  
  
    -   **Carpetas protegidas** esta sección enumeran todas las carpetas compartidas y todas las carpetas de historial de archivos en el servidor y todas las carpetas que ha optado por hacer una copia de seguridad en Azure. La lista incluye el nombre de carpeta, la ruta de acceso y el estado de cada carpeta compartida.  
  
    -   **Historial de copia de seguridad** en esta sección se muestra una lista de recientes copias de seguridad en línea. La lista incluye el tipo de operación y la hora y el estado de cada copia de seguridad.  
  
-   Un panel de detalles con información adicional acerca de un trabajo de copia de seguridad seleccionado o el trabajo de restauración.  
  
-   Un panel de tareas que incluye un conjunto de tareas administrativas que se pueden realizar.  
  
 Como procedimiento recomendado, debes hacer copia de seguridad de los datos empresariales y de usuario más importantes. Por ejemplo, debe hacer una copia de carpetas del servidor que contienen los archivos de datos fundamentales. También debe hacer el historial de archivos para los equipos de red que contienen información crítica.  
  
 Cuando vayas a decidir qué datos hay que hacer una copia de seguridad en línea, considera la posibilidad de la directiva de frecuencia y la retención de copia de seguridad que quieres implementar. A continuación, revisar tu presupuesto y determinar la cantidad de espacio de almacenamiento que se puede permitir. Equilibrar el costo y el volumen de almacenamiento frente a tus necesidades y, a continuación, configurar copia de seguridad en línea para proteger la mayor parte de los datos importantes como sea posible. Para información sobre precios, consulta [detalles sobre los precios de copia de seguridad de Azure](https://azure.microsoft.com/pricing/details/backup/).  
  
 Las siguientes secciones describen varias tareas de copia de seguridad en línea que pueden aparecer en el escritorio de Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Tareas de copia de seguridad en línea en el panel  
  
-   [Cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Iniciar una copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Restaurar archivos y carpetas desde una copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Anular el registro de servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>Cargar un certificado en el almacén de copia de seguridad de Azure  
 Antes de realizar copias de seguridad de Azure para las copias de seguridad en línea en Windows Server Essentials, debe cargar un certificado público que estés registrado en el almacén de copia de seguridad. El certificado se usa para autenticar la implementación de copia de seguridad de Azure (el agente), que actúa en nombre del propietario de la suscripción de Microsoft Online Services para administrar los recursos asociados con la suscripción.  
  
> [!NOTE]
>  Antes de cargar el certificado, debes completar los procedimientos descritos en [suscribirse a servicio de copia de seguridad de Azure de](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Para cargar un certificado que se usará con el servicio de copia de seguridad de Azure  
  
1.  Iniciar sesión en el escritorio de Windows Server Essentials con una cuenta de administrador.  
  
2.  En el panel **Home** página, haz clic en **copia de seguridad en línea**.  
  
3.  En la **copia de seguridad en línea** área, haz clic en **carga certificado al almacén de copia de seguridad de Azure**.  
  
     Esta opción abre **recuperación servicios** en el Portal de administración de Azure. Si puedes t de haber iniciado sesión en Azure, debes iniciar sesión con tu cuenta de Microsoft.  
  
4.  Haz clic en el nombre de la copia de seguridad depósito que usarás para copias de seguridad en línea para abrir el **inicio rápido** página para el almacén de copia de seguridad.  
  
5.  Haz clic en **administrar certificados**.  
  
6.  En la **administrar certificados** cuadro de diálogo, pegar la ruta de acceso del público certificado generado por Windows Server Essentials. Para obtener la ruta de acceso del certificado público, haz lo siguiente:  
  
    1.  En el panel de Windows Server Essentials, haz clic en el **copia de seguridad en línea** pestaña.  
  
    2.  En la **copia de seguridad en línea** página, copia la ruta de acceso del certificado generado.  
  
    3.  Cambiar el Portal de administración de Azure y, a continuación, en la **administrar certificados** diálogo cuadro, pegar la ruta de acceso para cargar el certificado público generado.  
  
    > [!NOTE]
    >  También puedes usar tu propio certificado pública. Para saber qué certificado es necesario, en la **inicio rápido** página, haz clic en el **adquirir certificado** vínculo.  
  
    > [!NOTE]
    >   Azure requiere un certificado de v2 x.509 con una clave pública. Para obtener más información, consulta [administrar certificados de almacén](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7.  Después de seleccionar el certificado, haz clic en **Aceptar** (activado).  
  
8.  Vuelve a la **inicio rápido** página. Haz clic en **panel**y comprueba que el certificado se cargó correctamente. Después de carga correctamente el certificado, el panel muestra la fecha de expiración y la huella digital de certificado.  
  
 Después de completar este procedimiento, haz lo siguiente:  
  
1.  Registrar el servidor con el servicio de copia de seguridad de Azure. Para obtener más información, consulta [registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
2.  Configurar copia de seguridad en línea del servidor. Para obtener más información, consulta [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_2"></a>Configurar copia de seguridad en línea  
 Después de registrar el servidor con la copia de seguridad de Azure, puedes configurar la configuración de copia de seguridad en línea en Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Configurar copia de seguridad en línea  
  
1.  Desde el Asistente para registrar su servidor o desde la **Online Backup** página del panel de Essentials de servidor de Windows, haz clic en **Configurar copia de seguridad en línea**. Aparece el Asistente para configurar copia de seguridad en línea.  
  
2.  En la **Configurar copia de seguridad en línea** página del asistente, selecciona la casilla de cada carpeta del servidor que quieres hacer una copia de seguridad de copia de seguridad de Azure. Desactiva la casilla para cada carpeta del servidor que no quieres incluir en la copia de seguridad. Para agregar carpetas a los que no se muestran en la lista, haz clic en **agregar carpetas**. Cuando termine la selección, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  No puedes seleccionar una carpeta que no se encuentra en el servidor local o una carpeta que se encuentra en una unidad con formato ReFS.  
  
3.  En la **agregar copias de seguridad de historial de archivos** página del asistente, puedes seleccionar para incluir las copias de seguridad del historial de archivos para equipos de la red que admiten la función de historial de archivos. A continuación, haz clic en **siguiente** para continuar.  
  
4.  En la **especificar la programación de copia de seguridad** página del asistente, configura lo siguiente y, a continuación, haz clic en **siguiente** para continuar:  
  
    -   Selecciona o acepta el momento del día que desea ejecutar la copia de seguridad en línea. El tiempo predeterminado es 10:00 PM. También puede especificar un momento para ejecutar una segunda copia de seguridad.  
  
    -   Selecciona o acepte los días que desea ejecutar la copia de seguridad en línea. De manera predeterminada, la copia de seguridad en línea se ejecuta el lunes al viernes.  
  
5.  En la **especificar la directiva de retención de copia de seguridad** página del asistente, selecciona el número de días que desea mantener copias de seguridad en línea y, a continuación, haz clic en **siguiente**. El valor predeterminado es 7 días. También puedes mantener las copias de seguridad en línea para 15 o 30 días.  
  
    > [!NOTE]
    >   Copia de seguridad de Azure mantiene siempre la copia de seguridad más reciente. Si el destino no tiene suficiente espacio disponible para almacenar la copia de seguridad, el proceso de copia de seguridad no funcionarán correctamente. Para evitar esta situación, el espacio de almacenamiento adicional de compra o acortar el período de retención de datos. Para información sobre precios, consulta [detalles de precios](https://azure.microsoft.com/pricing/details/backup/) copia de seguridad de Microsoft Azure.  
  
6.  En la **elegir el uso de ancho de banda** página del asistente, si quieres restringir la cantidad de ancho de banda de Internet que se asigna a la copia de seguridad en línea, selecciona **el uso de ancho de banda de Internet habilitar**. Usar las opciones de la página para especificar cuánto ancho de banda de Internet para permitir la copia de seguridad en línea usar durante el trabajo de horas y durante las horas no son de trabajo. A continuación, definir las horas laborables y días laborables.  
  
    > [!NOTE]
    >   Copia de seguridad de Azure te permite personalizar cómo el software de integración usa el ancho de banda de red al realizar copias de seguridad o restaurar la información. Usar una técnica que normalmente se conoce como "limitación", puede controlar la cantidad de ancho de banda de red que está disponible para su uso por la copia de seguridad y restaurar procesos durante el día específico y los intervalos de tiempo. Después de seleccionar la **el uso de ancho de banda de Internet habilitar** casilla en el asistente Configurar copia de seguridad en línea, puedes especificar los días laborables y las horas de trabajo durante la que se va a aplicar el límite de ancho de banda de horas de trabajo. El límite del horario de trabajo no se usa en todo otras veces. Los intervalos de ancho de banda válido son de 256 Kbps ilimitada de ambos límites.  
  
7.  Cuando se completa la configuración de copia de seguridad en línea, haz clic en **cerrar**.  
  
    > [!TIP]
    >  Después de una configuración de copia de seguridad correcta, el **Online Backup** página muestra el estado de la última copia de seguridad en línea y la cantidad de espacio de almacenamiento utilizado por el almacén de copia de seguridad. Para ver el estado de las copias de seguridad anteriores, haz clic en **historial de copia de seguridad**.  
  
###  <a name="BKMK_3"></a>Iniciar una copia de seguridad en línea  
  
> [!NOTE]
>  Antes de empezar a una copia de seguridad en línea, primero debes [registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)y luego [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Iniciar inmediatamente una copia de seguridad en línea  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **tareas de copia de seguridad en línea** panel, haz clic en **iniciar copia de seguridad ahora**.  
  
###  <a name="BKMK_4"></a>Restaurar archivos y carpetas desde una copia de seguridad en línea  
 Para restaurar archivos y carpetas asistente te guía a través del proceso de localización, seleccionar y restaurar archivos y carpetas que se una copia de seguridad en línea. Como avanzar a través del asistente, realizará las siguientes tareas:  
  
1.  **Elige una copia de seguridad en línea desde el que se va a restaurar archivos y carpetas**  
  
     Cuando se ejecución el Asistente de carpetas y restaurar archivos, lo primero que debes hacer es especificar si quieres restaurar archivos desde una copia de seguridad del servidor que se encuentra actualmente a o desde una copia de seguridad de un servidor diferente.  
  
2.  **Elige un servidor desde el que se va a restaurar archivos y carpetas**  
  
     Si has elegido restaurar archivos y carpetas de un servidor diferente, selecciona el servidor que desea restaurar a partir de la lista de servidores disponibles y, a continuación, haz clic en **siguiente**.  
  
3.  **Elige un volumen que contiene los archivos y carpetas restaurarse**  
  
     Las copias de seguridad en línea contienen volúmenes que corresponden a los volúmenes en el servidor de origen de copia de seguridad. Después de seleccionar el volumen, selecciona la fecha y hora de la copia de seguridad desde el que se va a restaurar y, a continuación, haz clic en **siguiente**.  
  
4.  **Seleccionar archivos y carpetas para restaurar**  
  
     En la **seleccionar elementos restaurar** página del asistente, puedes abrir las diversas ubicaciones de carpeta y selecciona la casilla para cada archivo o carpeta que desea restaurar. Cuando hayas terminado de seleccionar los archivos, haz clic en **siguiente**.  
  
5.  **Especificar la ubicación de destino de van a restaurar archivos y carpetas**  
  
     La **especificar opciones de restauración** página del asistente te permite especificar dónde restaurar archivos y carpetas. Existen dos opciones:  
  
    -   **Restaurar a la ubicación original**. Selecciona esta opción para restaurar archivos y carpetas a la ubicación exacta en la que se origina la copia de seguridad.  
  
    -   **Restaurar a otra ubicación**.  Elige esta opción para especificar una ubicación diferente en el servidor en el que se va a restaurar los archivos.  
  
         En la instalación predeterminada, si ya existe un archivo con el mismo nombre que un archivo de restauración en la ubicación de destino, el asistente crea una copia del archivo original. Además, la lista de Control de acceso (ACL) se actualiza para reflejar los permisos de archivo actual. Puedes hacer clic en **avanzadas** para personalizar la configuración predeterminada.  
  
6.  **Confirma la información de restauración**  
  
     La **confirmar tu información de restauración** página proporciona un resumen de las instrucciones de restauración que hayas especificado. Para continuar con la restauración de archivos, debes escribir la frase de contraseña correcto para tu cuenta de copia de seguridad en línea.  
  
###  <a name="BKMK_5"></a>Registrar este servidor copia de seguridad  
 Para hacer copia de seguridad o la restauración de archivos, carpetas y el historial de archivos en el servidor de Windows Server Essentials en copia de seguridad de Azure, primero debes registrar tu servidor con el servicio de copia de seguridad de Microsoft Azure.  
  
> [!NOTE]
>  Antes de registrar el servidor, debes enviar un certificado que se usará con el almacén de copia de seguridad que se almacena las copias de seguridad en línea. Para obtener más información, consulta [cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Para registrar el servidor con la copia de seguridad de Azure  
  
1.  Iniciar sesión en el servidor como administrador y abra el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **copia de seguridad en línea** subsección, haz clic en **registrar**.  
  
4.  Elige el certificado que desea usar con el almacén de copia de seguridad para las copias de seguridad en línea. El certificado predeterminado está activado de forma predeterminada; Si desea elegir un certificado diferente, usa **examinar**. A continuación, haz clic en **siguiente**.  
  
5.  Sigue las instrucciones del Asistente para crear una frase de contraseña y, a continuación, completar el registro.  
  
###  <a name="BKMK_6"></a>Anular el registro de servidor  
  
> [!CAUTION]
>  Si anular el registro de tu servidor de Windows Server Essentials desde el servicio de copia de seguridad de Microsoft Azure, copia de seguridad de Azure ya no hacer una copia del servidor. Además, se borrarán los datos del servidor que se ha cargado previamente. Para reanudar las copias de seguridad en línea, debe registrar el servidor de nuevo.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Para anular el registro de tu servidor con la copia de seguridad de Azure  
  
1.  Inicia sesión en el [Portal de administración de Azure](https://manage.windowsazure.com).  
  
2.  Haz clic en **recuperación servicios**.  
  
3.  En **recuperación servicios**, haz clic en el nombre del almacén de copia de seguridad.  
  
4.  Desde el **inicio rápido** para el almacén de la página, haz clic en **servidores**.  
  
5.  Selecciona el servidor, haz clic en **eliminar**y, a continuación, haz clic en **Sí** en el aviso de confirmación.  
  
## <a name="related-tasks"></a>Tareas relacionadas  
  
-   [Cambiar la directiva de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ver el uso de almacenamiento de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Incluye una nueva carpeta de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Quitar o excluir copias de seguridad de historial de archivos de la directiva de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Deshabilita o vuelve a habilitar la copia de seguridad de servidor en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Detener una copia de seguridad de servidor en línea en curso](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Ver el estado de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Ver y administrar alertas de copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Restablecer la copia de seguridad en línea a la configuración predeterminada](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Registrarse para el servicio de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Integrar la copia de seguridad de Azure con Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Proteger las carpetas de copia de seguridad en línea en Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Historial de copia de seguridad en línea en Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>Cambiar la directiva de copia de seguridad en línea  
 Es fácil realizar cambios en la directiva de copia de seguridad en línea mediante el panel de Windows Server Essentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Para cambiar la directiva de copia de seguridad en línea  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **tareas de copia de seguridad en línea** panel, haz clic en **Configurar copia de seguridad en línea**.  
  
4.  Sigue las instrucciones del Asistente para personalizar la directiva de copia de seguridad en línea.  
  
 Para obtener más información sobre la configuración que se puede personalizar, consulta [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_8"></a>Ver el uso de almacenamiento de copia de seguridad en línea  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Para ver la cantidad de espacio de almacenamiento que usa la copia de seguridad en línea  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **copia de seguridad en línea** ficha, la **estado de almacenamiento** aparece en el panel de información.  
  
###  <a name="BKMK_9"></a>Incluye una nueva carpeta de copia de seguridad en línea  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Para incluir una nueva carpeta en la directiva de copia de seguridad en línea  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **tareas de copia de seguridad en línea** panel, haz clic en **Configurar copia de seguridad en línea**.  
  
4.  En la **cambiar o quitar la directiva de copia de seguridad** página, haz clic en **personalizar la directiva de copia de seguridad en línea**.  
  
5.  En la **Configurar copia de seguridad en línea** no aparece la página, si la carpeta que quieras incluir en la lista, haz clic en **agregar carpetas**.  
  
6.  En la **agregar carpetas** ventana, busca y selecciona la carpeta que quieres incluir en la copia de seguridad en línea y, a continuación, haz clic en **Aceptar**.  
  
7.  En la **Configurar copia de seguridad en línea**, seleccione otras carpetas para incluir que quieras y, a continuación, haz clic en **siguiente**.  
  
8.  Sigue las instrucciones del Asistente para terminar de personalizar la directiva de copia de seguridad en línea.  
  
 Para obtener más información sobre otras opciones de configuración que se pueden personalizar, consulta [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_10"></a>Quitar o excluir copias de seguridad de historial de archivos de la directiva de copia de seguridad en línea  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Para quitar o excluir una carpeta de la directiva de copia de seguridad en línea  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  Haz clic en el **carpetas protegidas** pestaña. El panel de detalles muestra una lista de carpetas de con su estado de copia de seguridad en línea.  
  
4.  Selecciona la carpeta que desea excluir de la directiva de copia de seguridad en línea y, a continuación, en el panel de tareas, haz clic en **quitar la carpeta de copia de seguridad en línea**.  
  
###  <a name="BKMK_11"></a>Deshabilita o vuelve a habilitar la copia de seguridad de servidor en línea  
 Para obtener instrucciones sobre cómo usar la copia de seguridad de Azure para restaurar los datos del servidor, consulta [registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Para obtener instrucciones sobre cómo dejar de usar la copia de seguridad de Azure para restaurar los datos del servidor, consulta [Unregister servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="BKMK_12"></a>Detener una copia de seguridad de servidor en línea en curso  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Para detener una copia de seguridad de servidor en línea en curso  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  En la **tareas de copia de seguridad en línea** panel, haz clic en **detener la copia de seguridad**.  
  
     Después de detener la copia de seguridad, un estado de **cancelado** aparece la copia de seguridad en el **historial de copia de seguridad** lista.  
  
###  <a name="BKMK_13"></a>Ver el estado de copia de seguridad en línea  
  
##### <a name="to-view-the-backup-status"></a>Para ver el estado de copia de seguridad  
  
1.  Iniciar sesión como administrador el panel.  
  
2.  En la barra de navegación, haz clic en **copia de seguridad en línea**.  
  
3.  Haz clic en el **historial de copia de seguridad** pestaña. La vista de lista muestra el estado de cada trabajo de copia de seguridad. Selecciona un trabajo de copia de seguridad para ver más detalles sobre ese trabajo.  
  
###  <a name="BKMK_14"></a>Ver y administrar alertas de copia de seguridad en línea  
 Al igual que muchos otros avisos, alertas de copia de seguridad de Azure se muestran en el Visor de alertas.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Para ver las alertas de copia de seguridad en línea en el Visor de alertas  
  
1.  Abra el panel.  
  
2.  Realiza una de las siguientes acciones:  
  
      Windows Server Essentials: En el panel de navegación, haz clic en el icono de alertas \ (puede ser crítico, advertencia o Informational\). Así abre el Visor de alertas.  
  
      Windows Server Essentials: En el **Home** página, haz clic en el **supervisión de estado** pestaña.  
  
3.  Revisa la lista de alertas para los problemas que se relacionan con la copia de seguridad de Azure.  
  
 Para obtener más información sobre cómo usar la pestaña del Visor de alertas o supervisión de estado para administrar alertas, consulta [administrar el estado del sistema](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_15"></a>Restablecer la copia de seguridad en línea a la configuración predeterminada  
 Windows Server Essentials proporciona a un asistente que te ayudará a configurar la configuración de copia de seguridad en línea. Si quieres restaurar la configuración predeterminada, ejecuta el **Configurar copia de seguridad en línea** de tareas y elegir el **quitar la directiva de copia de seguridad en línea** opción. A continuación, ejecuta el **Configurar copia de seguridad en línea** tarea nuevamente. Modifica los datos cargados previamente.  
  
###  <a name="BKMK_16"></a>Registrarse para el servicio de copia de seguridad de Azure  
 Para prepararse para integrar la copia de seguridad de Microsoft Azure con Windows Server Essentials, podrás iniciar sesión en el Portal de administración de Azure con tu cuenta de Microsoft Online Services y, a continuación, crear un almacén de copia de seguridad para almacenar las copias de seguridad en línea en Azure. A continuación, podrás descargar el módulo de integración de copia de seguridad de Azure y usar el archivo descargado para instalar la copia de seguridad de Azure Add-In en el servidor de Windows Server Essentials. Si no tienes una cuenta de Microsoft, puede registrarse para una prueba gratuita.  
  
 Para realizar esta configuración, completa las siguientes tareas:  
  
1.  Registrarse para una cuenta de Microsoft Online Services y la vista previa de copias de seguridad.  
  
2.  Crear un almacén de copia de seguridad para almacenar copias de seguridad en línea.  
  
3.  Descargar al agente de copia de seguridad de Azure.  
  
4.  Instalar la copia de seguridad de Azure Add-In en el servidor.  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>Registrarse para una cuenta de Microsoft Online Services y la vista previa de copias de seguridad  
  
1.  Iniciar sesión en el escritorio de Windows Server Essentials.  
  
2.  En el panel **Home** página, haz clic en el **ADD-INS** categoría, haz clic en **integrar con copia de seguridad de Azure**y, a continuación, haz clic en **, haga clic para registrarse para la copia de seguridad de Azure**.  
  
3.  En la Azure **recuperación servicios** página, en la **copia de seguridad (vista previa)** sección, revisa los detalles.  
  
4.  Si no tienes una suscripción de Azure, haz clic en **prueba gratuita**y, a continuación, sigue las instrucciones para obtener una suscripción de Azure.  
  
     Si ya tienes una suscripción de Azure, haz clic en **Portal** en la esquina superior derecha de la página web para ir al Portal de administración de Azure.  
  
5.  En la página de Portal de administración de Azure, verás **recuperación servicios** en el panel izquierdo. S donde todo puedes administrar la copia de seguridad depósitos las copias de seguridad en línea de que la tienda de Windows Server Essentials.  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Crear un almacén de copia de seguridad para almacenar copias de seguridad en línea  
  
1.  Inicia sesión en el [Portal de administración de Azure](https://manage.windowsazure.com)desde el explorador web en el servidor de Windows Server Essentials.  
  
2.  En el Portal de administración de Azure, haz clic en **nueva**, haz clic en **servicios de datos**, haz clic en **recuperación servicios**, haz clic en **depósito de copia de seguridad**y, a continuación, haz clic en **creación rápida**.  
  
3.  Escribe un nombre para el almacén de copia de seguridad, elige la región donde quieres almacenar las copias de seguridad y, a continuación, haz clic en **crear rápido**.  
  
     La **recuperación servicios** área abre con el nuevo almacén de copia de seguridad muestra.  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Descargar al agente de copia de seguridad de Azure  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En el panel **Home** página, haz clic en el **Introducción** pestaña, haz clic en el **ADD-INS** categoría, haz clic en **integrar con copia de seguridad de Azure**y, a continuación, haz clic en **haga clic para descargar el módulo de integración de copia de seguridad de Azure**.  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Instalar la copia de seguridad de Azure Add-In en el servidor  
  
1.  Inicia sesión en el servidor mediante una cuenta de administrador y, a continuación, ejecuta el **OnlineBackupAddin.wssx** archivo descargado en el paso anterior.  
  
2.  La **términos de licencia del Software** se muestran. Si estás de acuerdo con los términos y condiciones, haz clic en **Aceptar** para continuar la instalación.  
  
3.  En la **instalar el complemento** página, haz clic en **instalar el complemento**.  
  
    > [!NOTE]
    >  Puede que tengas que reiniciar el servidor para instalar cualquier software necesario.  
  
     La **instalación** se muestra la página. Un indicador de progreso se muestra cuando la instalación se inicia y muestra el progreso de la instalación. Una vez completada la instalación, recibirás un mensaje de que la copia de seguridad de Azure Add-in se instaló correctamente.  
  
4.  Haz clic en **finalizar**.  
  
5.  Cierra y vuelve a abrir el panel.  
  
     Una nueva pestaña, **copia de seguridad en línea**, se agrega al panel. En esta pestaña, puedes registrar tu servidor, configurar copia de seguridad y abrir **recuperación servicios** en el Portal de administración de Azure para administrar los almacenes de copia de seguridad para los servidores.  
  
 Una vez completados estos pasos, haz lo siguiente:  
  
1.  En Windows Server Essentials, cargar el certificado que utilizarás para copias de seguridad en línea. Para obtener más información, consulta [cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
2.  Registrar el servidor en el almacén de copia de seguridad de Azure. Para obtener más información, consulta [registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
3.  Configurar copia de seguridad en línea del servidor. Para obtener más información, consulta [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_17"></a>Integrar la copia de seguridad de Azure con Windows Server Essentials  
 El módulo de integración de copia de seguridad de Microsoft Azure es una nueva característica de Windows Server Essentials que le permite cifrar y hacer una copia archivos y carpetas desde el servidor en un sistema de almacenamiento de Azure hospedado proporcionado por Microsoft. Si usas copia de seguridad de Azure para cifrar y hacer copia de seguridad de datos en el servidor, puedes ayudar a evitar la pérdida de catastrófico de datos fundamentales para la empresa debido a incendio, inundación, robo u otros desastres. Cuando usas copia de seguridad de Azure para hacer copia de seguridad de datos del servidor, la información se cifra con la frase de contraseña antes de que se carga en un centro de datos segura en Internet. Obtener acceso a datos en línea copia de seguridad, debe tener un servidor que será autenticado por un certificado y debes proporcionar la frase de contraseña.  
  
 Después de integrar y registrar el servidor con la copia de seguridad de Azure, puedes configurar la configuración de copia de seguridad en línea para realizar copias de seguridad programadas periódicamente. También se puede iniciar una copia de seguridad en línea en cualquier momento haciendo clic en el **iniciar copia de seguridad ahora** tareas en el panel de copia de seguridad en línea.  
  
 La configuración de copia de seguridad en línea incluye los siguientes pasos:  
  
1.  [Suscribirse a servicio de copia de seguridad de Azure de](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) : después de iniciar sesión al servicio de copia de seguridad, crear un almacén de copia de seguridad, descargará e instalará el módulo de integración de copia de seguridad de Azure.  
  
2.  [Cargar un certificado en el almacén de copia de seguridad de Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Registrar este servidor copia de seguridad](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurar copia de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Copia de seguridad de Azure, usa la frase de contraseña para cifrar archivos y carpetas a la copia de seguridad en línea. Cambiar la contraseña de cifrado reemplazará la frase de contraseña que especificaste cuando te registraste en el servidor. La frase de contraseña acepta solo los caracteres ASCII en el código.  
  
###  <a name="BKMK_18"></a>Proteger las carpetas de copia de seguridad en línea en Windows Server Essentials  
 La **carpetas protegidas** subsección en la sección de copia de seguridad en línea del panel, muestra una lista de todas las carpetas compartidas en el servidor. La siguiente tabla describe la información que se incluye en la lista.  
  
|Columna|Descripción|  
|------------|-----------------|  
|**Nombre de carpeta:**|El nombre de la carpeta que se incluye en la copia de seguridad en línea.<br /><br /> Para agregar o excluir una carpeta, ejecuta el **Configurar copia de seguridad en línea** tarea.|  
|**Ruta de acceso de la carpeta:**|La ubicación de la carpeta.|  
|**Estado:**|Existen tres tipos de estado **Protected**, **no protegidas**, y **desconocido**.|  
  
###  <a name="BKMK_19"></a>Historial de copia de seguridad en línea en Windows Server Essentials  
 La **historial de copia de seguridad** subsección en la sección de copia de seguridad en línea del panel muestra una lista de recientes copias de seguridad en línea. Puedes usar las copias de seguridad correctas para restaurar archivos y carpetas. La siguiente tabla describe la información que se incluye en la lista.  
  
|Columna|Descripción|  
|------------|-----------------|  
|**Operación:**|Hay dos tipos de operaciones - **copia de seguridad** y **restaurar**.|  
|**Tiempo:**|Este es el tiempo que se registra para el estado más reciente.|  
|**Estado:**|Hay cinco tipos de estado **éxito**, **en curso**, **cancelado**, **advertencia**, y **erróneos**.|  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
