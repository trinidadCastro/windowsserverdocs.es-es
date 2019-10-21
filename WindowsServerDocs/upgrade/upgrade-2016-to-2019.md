---
title: Actualizar Windows Server 2016 a Windows Server 2019 | Microsoft Docs
description: Obtenga información acerca de cómo realizar una actualización en contexto para pasar de Windows Server 2016 a Windows Server 2019.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 62fe4f00cef121e6241a403ee339047cda9488b5
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591087"
---
# <a name="upgrade-windows-server-2016-to-windows-server-2019"></a>Actualizar Windows Server 2016 a Windows Server 2019

Si desea mantener el mismo hardware y todos los roles de servidor que ya ha configurado sin reducir el servidor, querrá realizar una actualización en contexto de. Una actualización en contexto permite pasar de un sistema operativo anterior a otro más reciente, a la vez que mantiene la configuración, los roles de servidor y los datos intactos. Este artículo le ayuda a pasar de Windows Server 2016 a Windows Server 2019.

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de comenzar la actualización en contexto

Antes de iniciar la actualización de Windows Server, se recomienda recopilar información de los dispositivos, con fines de diagnóstico y solución de problemas. Dado que esta información está destinada a usarse solo si se produce un error en la actualización, debe asegurarse de que almacena la información en algún lugar en el que pueda acceder a ella fuera del dispositivo.

### <a name="to-collect-your-info"></a>Para recopilar información

1. Abra un símbolo del sistema, vaya a `c:\Windows\system32` y escriba **SystemInfo. exe**.

2. Copie, pegue y almacene la información del sistema resultante en el lugar de su dispositivo.

3. Escriba **ipconfig/all** en el símbolo del sistema y, a continuación, copie y pegue la información de configuración resultante en la misma ubicación anterior.

4. Abra el editor del registro, vaya a la clave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` y, a continuación, copie y pegue Windows Server **BuildLabEx** (versión) y **EditionID** (edición) en la misma ubicación que el anterior.

Después de recopilar toda la información relacionada con Windows Server, le recomendamos encarecidamente que realice una copia de seguridad del sistema operativo, las aplicaciones y las máquinas virtuales. También debe **apagar**, **migrar rápidamente**o **migrar en vivo** las máquinas virtuales que se ejecutan actualmente en el servidor. No se puede ejecutar ninguna máquina virtual durante la actualización en contexto.

## <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1. Asegúrese de que el valor **BuildLabEx** indica que está ejecutando Windows Server 2016.

2. Busque los medios de instalación de Windows Server 2019 y, a continuación, seleccione **setup. exe**.

    ![Explorador de Windows que muestra el archivo Setup. exe](media/upgrade-2016-2019/setup-2019.png)

3. Seleccione **sí** para iniciar el proceso de instalación.

    ![Control de cuentas de usuario que solicita permiso para iniciar el programa de instalación](media/upgrade-2016-2019/start-setup-uac-box.png)

4. Para dispositivos conectados a Internet, seleccione la opción **Descargar actualizaciones, controladores y características opcionales (recomendado)** y, a continuación, seleccione **siguiente**.

    ![Pantalla para elegir conectarse para obtener actualizaciones importantes de Windows](media/upgrade-2016-2019/online-updates-win-setup.png)

5. El programa de instalación comprueba la configuración del dispositivo, debe esperar a que finalice y, a continuación, seleccionar **siguiente**.

6. En función del canal de distribución que haya recibido de Windows Server media (versión comercial, licencia por volumen, OEM, ODM, etc.) y la licencia del servidor, es posible que se le pida que escriba una clave de licencia para continuar.

7. Seleccione la edición 2019 de Windows Server que desea instalar y, a continuación, seleccione **siguiente**.

    ![Pantalla para elegir la edición 2016 de Windows Server que se va a instalar](media/upgrade-2016-2019/select-os-edition.png)

8. Seleccione **Aceptar** para aceptar los términos del contrato de licencia, en función del canal de distribución (por ejemplo, venta directa, licencia por volumen, OEM, ODM, etc.).

    ![Pantalla para aceptar el contrato de licencia](media/upgrade-2016-2019/license-terms.png)

9. Seleccione **mantener archivos y aplicaciones personales** para elegir realizar una actualización en contexto y, a continuación, seleccione **siguiente**.

    ![Pantalla para elegir el tipo de instalación](media/upgrade-2016-2019/choose-install-upgrade.png)

10. Después de que el programa de instalación analice el dispositivo, le pedirá que continúe con la actualización; para ello, seleccione **instalar**.

    ![Pantalla que muestra que está listo para iniciar la actualización](media/upgrade-2016-2019/ready-to-install.png)

    Se inicia la actualización en contexto, que muestra la pantalla de **actualización de Windows** con su progreso. Una vez finalizada la actualización, el servidor se reiniciará.

    ![Pantalla que muestra el progreso de la actualización](media/upgrade-2016-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>Una vez finalizada la actualización

Una vez finalizada la actualización, debe asegurarse de que la actualización a Windows Server 2019 se realizó correctamente.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para asegurarse de que la actualización se realizó correctamente

1. Abra el editor del registro, vaya a la clave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` y vea el **NombreProducto**. Debería ver su edición de Windows Server 2019, por ejemplo, **Windows server 2019 Datacenter**.

2. Asegúrese de que todas las aplicaciones se están ejecutando y que las conexiones de cliente a las aplicaciones se han realizado correctamente.

Si cree que algo podría haber ido mal durante la actualización, copie y comprima el directorio `%SystemRoot%\Panther` (normalmente `C:\Windows\Panther`) y póngase en contacto con el soporte técnico de Microsoft.

## <a name="related-articles"></a>Artículos relacionados

- Para obtener más detalles e información sobre Windows Server 2019, consulte Introducción [a Windows server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19).
