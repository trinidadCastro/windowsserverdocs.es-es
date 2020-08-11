---
title: Actualización de Windows Server 2016 a Windows Server 2019 | Microsoft Docs
description: Obtén información acerca de cómo realizar una actualización en contexto para pasar de Windows Server 2016 a Windows Server 2019.
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 2ed2ed1859ca69f2251543a78dec0b856ce3f7e2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997217"
---
# <a name="upgrade-windows-server-2016-to-windows-server-2019"></a>Actualización de Windows Server 2016 a Windows Server 2019

Si quieres mantener el mismo hardware y todos los roles de servidor que has configurado sin eliminar el formato del servidor, puedes realizar una actualización en contexto. Una actualización en contexto te permite pasar de un sistema operativo anterior a uno más reciente, y la configuración, los roles de servidor y los datos se mantienen intactos. Este artículo te ayuda a pasar de Windows Server 2016 a Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de comenzar la actualización en contexto

Antes de iniciar la actualización de Windows Server, se recomienda recopilar información de tus dispositivos, con fines de diagnóstico y solución de problemas. Dado que esta información está destinada a usarse solo si se produce un error en la actualización, debes asegurarte de almacenar la información en algún lugar en el que puedas acceder a ella fuera del dispositivo.

### <a name="to-collect-your-info"></a>Para recopilar la información

1. Abre un símbolo del sistema, ve a `c:\Windows\system32` y escribe **systeminfo.exe**.

2. Copia, pega y almacena la información del sistema resultante en una ubicación fuera del dispositivo.

3. Escribe **ipconfig /all** en el símbolo del sistema y después copia y pega la información de configuración resultante en la misma ubicación que se indicó anteriormente.

4. Abre el editor del registro, ve a la clave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` y, a continuación, copia y pega las propiedades **BuildLabEx** (versión) y **EditionID** (edición) de Windows Server en la misma ubicación que se indicó anteriormente.

Después de recopilar toda la información relacionada con Windows Server, se recomienda realizar una copia de seguridad del sistema operativo, las aplicaciones y las máquinas virtuales. También debes **apagar**, **migrar rápidamente** o **migrar en vivo** todas las máquinas virtuales que están en ejecución actualmente en el servidor. Ninguna de las máquinas virtuales puede estar en ejecución durante la actualización local.

## <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1. Asegúrate de que el valor **BuildLabEx** indica que estás ejecutando Windows Server 2016.

2. Busca los medios de instalación de Windows Server 2019 y, a continuación, selecciona **setup.exe**.

    ![Explorador de Windows que muestra el archivo setup.exe](media/upgrade-2016-2019/setup-2019.png)

3. Selecciona **Sí** para iniciar el proceso de instalación.

    ![Control de cuentas de usuario solicitando permiso para iniciar el programa de instalación](media/upgrade-2016-2019/start-setup-uac-box.png)

4. Para los dispositivos conectados a Internet, selecciona la opción **Download updates, drivers and optional features (recommended)** [Descargar actualizaciones, controladores y características opcionales (recomendado)] y, después, selecciona **Siguiente**.

    ![Pantalla para optar por obtener las actualizaciones importantes de Windows en línea](media/upgrade-2016-2019/online-updates-win-setup.png)

5. El programa de instalación comprueba la configuración del dispositivo; debes esperar a que finalice y, a continuación, seleccionar **Siguiente**.

6. En función del canal de distribución del que recibiste los medios de Windows Server (versión comercial, licencia por volumen, OEM, ODM, etc.) y la licencia del servidor, se te podría solicitar que escribas una clave de licencia para continuar.

7. Selecciona la edición de Windows Server 2019 que quieres instalar y, a continuación, selecciona **Siguiente**.

    ![Pantalla para elegir la edición de Windows Server 2019 que se va a instalar](media/upgrade-2016-2019/select-os-edition.png)

8. Selecciona **Aceptar** para aceptar los términos del contrato de licencia, en función del canal de distribución (por ejemplo, versión comercial, licencia por volumen, OEM, ODM, etc.).

    ![Pantalla para aceptar el contrato de licencia](media/upgrade-2016-2019/license-terms.png)

9. Selecciona **Conservar los archivos personales y las aplicaciones** para elegir la opción de actualización local y, a continuación, selecciona **Siguiente**.

    ![Pantalla para elegir el tipo de instalación](media/upgrade-2016-2019/choose-install-upgrade.png)

10. Después de que el programa de instalación analice el dispositivo, te pedirá que selecciones **Instalar** para continuar con la actualización.

    ![Pantalla que muestra que estás listo para iniciar la actualización](media/upgrade-2016-2019/ready-to-install.png)

    Se inicia la actualización local y se muestra la pantalla **Actualizando Windows** con su progreso. Una vez finalizada la actualización, el servidor se reiniciará.

    ![Pantalla que muestra el progreso de la actualización](media/upgrade-2016-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Una vez finalizada la actualización

Una vez completada la actualización, debes asegurarte de que la actualización a Windows Server 2019 se realizó correctamente.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para asegurarse de que la actualización se realizó correctamente

1. Abre el editor del registro, ve a la clave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` y consulta **ProductName**. Debería mostrarse tu edición de Windows Server 2019; por ejemplo **Windows Server 2019 Datacenter**.

2. Asegúrate de que todas las aplicaciones se están ejecutando y de que las conexiones de cliente a las aplicaciones se realizaron correctamente.

Si crees que algo podría haber ido mal durante la actualización, copia y comprime el directorio `%SystemRoot%\Panther` (normalmente `C:\Windows\Panther`) y ponte en contacto con el soporte técnico de Microsoft.

## <a name="related-articles"></a>Artículos relacionados

- Para obtener más detalles e información sobre Windows Server 2019, consulta [Introducción a Windows Server 2019](../get-started-19/get-started-19.md).