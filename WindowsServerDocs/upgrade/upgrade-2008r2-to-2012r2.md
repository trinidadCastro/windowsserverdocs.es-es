---
title: Actualización de Windows Server 2008 R2 a Windows Server 2012 R2 | Microsoft Docs
description: Obtén información acerca de cómo realizar una actualización local para pasar de Windows Server 2008 R2 a Windows Server 2012 R2.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: d5051239f7269eb4b6361187121ac960e06f6d9e
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125085"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>Actualización de Windows Server 2008 R2 a Windows Server 2012 R2

Si quieres mantener el mismo hardware y todos los roles de servidor que has configurado sin eliminar el formato del servidor, puedes realizar una actualización local. Una actualización local te permite pasar de un sistema operativo anterior a uno más reciente, y al mismo tiempo, mantener intactos la configuración, los roles de servidor y los datos. Este artículo te ayuda a pasar de Windows Server 2008 R2 a Windows Server 2012 R2.

Para actualizar a Windows Server 2019, usa primero este tema para actualizar a Windows Server 2012 R2 y, a continuación, [actualiza de Windows Server 2012 R2 a Windows  Server 2019](upgrade-2012r2-to-2019.md).

## <a name="before-you-begin-your-in-place-upgrade"></a>Antes de comenzar la actualización local

Antes de iniciar la actualización de Windows Server, se recomienda recopilar información de tus dispositivos, con fines de diagnóstico y solución de problemas. Dado que esta información está destinada a usarse solo si se produce un error en la actualización, debes asegurarte de almacenar la información en algún lugar en el que puedas acceder a ella fuera del dispositivo.

### <a name="to-collect-your-info"></a>Para recopilar la información

1. Abre un símbolo del sistema, ve a `c:\Windows\system32` y escribe **systeminfo.exe**.

2. Copia, pega y almacena la información del sistema resultante en una ubicación fuera del dispositivo.

3. Escribe **ipconfig /all** en el símbolo del sistema y después copia y pega la información de configuración resultante en la misma ubicación que antes.

4. Abre el Editor del Registro, ve al subárbol HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion y, a continuación, copia y pega las propiedades **BuildLabEx** (versión) y **EditionID** (edición) de Windows Server en la misma ubicación que antes.

Después de recopilar toda la información relacionada con Windows Server, se recomienda realizar una copia de seguridad del sistema operativo, las aplicaciones y las máquinas virtuales. También debes **apagar**, **migrar rápidamente** o **migrar en vivo** todas las máquinas virtuales que están en ejecución actualmente en el servidor. Ninguna de las máquinas virtuales puede estar en ejecución durante la actualización local.

## <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1. Asegúrate de que el valor de **BuildLabEx** indica que estás ejecutando Windows Server 2008 R2.

2. Busca los medios de instalación de Windows Server 2012 R2 y, a continuación, selecciona **setup.exe**.

    ![Explorador de Windows que muestra el archivo setup.exe](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. Selecciona **Sí** para iniciar el proceso de instalación.

    ![Control de cuentas de usuario solicitando permiso para iniciar el programa de instalación](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. En la pantalla Windows Server 2012 R2, selecciona **Instalar ahora**.

5. En el caso de dispositivos conectados a Internet, selecciona **Conectarse para instalar actualizaciones (recomendado)** .

    ![Pantalla para optar por obtener las actualizaciones importantes de Windows en línea](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. Selecciona la edición de Windows Server 2012 R2 que quieres instalar y, a continuación, selecciona **Siguiente**.

    ![Pantalla para elegir la edición de Windows Server 2012 R2 que se va a instalar](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. Selecciona **Acepto los términos de la licencia** para aceptar los términos del contrato de licencia, en función del canal de distribución (por ejemplo, versión comercial, licencia por volumen, OEM, ODM, etc.) y, a continuación, selecciona **Siguiente**.

    ![Pantalla para aceptar el contrato de licencia](media/upgrade-2008r2-2012r2/license-terms.png)

8. Selecciona **Upgrade: Install Windows and keep files, settings, and applications** (Actualización: instalar Windows y mantener los archivos, la configuración y las aplicaciones) para optar por realizar una actualización local.

    ![Pantalla para elegir el tipo de instalación](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. El programa de instalación te recuerda que compruebes que las aplicaciones son compatibles con Windows Server 2012 R2. Para ello, usa la información de [Instalación y actualización de Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade) y, a continuación, selecciona **Siguiente**.

    ![Pantalla que recuerda que se debe comprobar la compatibilidad de las aplicaciones](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. Si ves una página que indica que no se recomienda la actualización, puedes ignorarla y seleccionar **Confirmar**. Se incluye para dar pie a instalaciones limpias, pero su aceptación no es obligatoria.

    ![Pantalla que muestra el progreso de la actualización](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    Se inicia la actualización local y se muestra la pantalla **Actualizando Windows** con su progreso. Una vez finalizada la actualización, el servidor se reiniciará.

## <a name="after-your-upgrade-is-done"></a>Una vez finalizada la actualización

Una vez completada la actualización, debes asegurarte de que la actualización a Windows Server 2012 R2 se realizó correctamente.

### <a name="to-make-sure-your-upgrade-was-successful"></a>Para asegurarte de que la actualización se realizó correctamente

1. Abre el Editor del Registro, ve al subárbol HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\WindowsNT\CurrentVersion y consulta el campo **ProductName**. Debería mostrarse tu edición de Windows Server 2012 R2; por ejemplo **Windows Server 2012 R2 Datacenter**.

2. Asegúrate de que todas las aplicaciones se están ejecutando y de que las conexiones de cliente a las aplicaciones se realizaron correctamente.

Si crees que algo podría haber ido mal durante la actualización, copia y comprime el directorio `%SystemRoot%\Panther` (normalmente `C:\Windows\Panther`) y ponte en contacto con el soporte técnico de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

Puedes realizar una o varias actualizaciones para pasar de Windows Server 2012 R2 a Windows Server 2019. Para obtener instrucciones detalladas, consulta [Actualización de Windows Server 2012 R2 a Windows Server 2019](upgrade-2012r2-to-2019.md).

## <a name="related-articles"></a>Artículos relacionados

- Para obtener más detalles e información sobre Windows Server 2012 R2, consulta [Windows Server 2012 R2 y Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11)).
