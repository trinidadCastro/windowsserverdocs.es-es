---
title: Transición de Windows Server Essentials a Windows Server 2012 Standard
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4cc616c0e23c58ab1298526784574f6bc8f65f23
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180391"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transición de Windows Server Essentials a Windows Server 2012 Standard

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server &reg; 2012 Essentials admite hasta 25 usuarios y dispositivos 50. Si sus necesidades empresariales superan el límite, puede realizar una transición de licencias local de Windows Server Essentials a Windows Server 2012 Standard para mantener la licencia conforme.

## <a name="how-the-transition-affects-user-and-device-limits"></a>Cómo afecta la transición al límite de usuarios y dispositivos
 Después de realizar la transición a Windows Server 2012 Standard, se quitan los límites de cuentas de usuario y dispositivos, pero las características que son exclusivas de Windows Server Essentials (como el panel, el acceso Web remoto y la copia de seguridad de equipos cliente) siguen estando disponibles. Sin embargo, las limitaciones técnicas de estas características permiten un máximo de 75 cuentas de usuario y 75 dispositivos. Si es necesario agregar más de 75 cuentas de usuario o dispositivos, debe desactivar las características de Windows Server Essentials y usar las herramientas nativas estándar de Windows Server 2012 para administrar las cuentas de usuario y los dispositivos.

> [!IMPORTANT]
>   Windows Server 2012 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo de su entorno. Esto es diferente de Windows Server Essentials, que no usa el modelo CAL y no incluye ninguna cal.  Al pasar de Windows Server Essentials a Windows Server 2012 Standard, debe comprar el número y tipo de cal adecuados para su entorno (la mayoría de los clientes compran cal de usuario).

## <a name="before-the-transition"></a>Antes de la transición

-   Antes de pasar de Windows Server Essentials a Windows Server 2012 Standard, debe realizar una copia de seguridad completa de los datos del servidor.

    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no podrá restaurar el servidor al estado que tenía antes de la transición.

-   Además, asegúrese de leer y comprender el contrato de licencia para el usuario final (CLUF) para Windows Server 2012 Standard. Para ver los términos de licencia:

    1.  Abra una ventana del símbolo del sistema como administrador.

    2.  Ejecute el siguiente comando:

         **DISM/online/Set-Edition: ServerStandard/geteula: ruta de acceso del CLUF**

         Donde **ruta de términos de licencia** representa la ubicación donde desea guardar al archivos de los términos de licencia. Por ejemplo: C:\ws8std_eula.rtf.  Asegúrese de usar .rtf como extensión de archivo.

    3.  Abra la ubicación donde guardó el archivo y haga doble clic en él para abrirlo.

## <a name="transition-to--windows-server-2012-standard"></a>Transición a Windows Server 2012 Standard
 Una vez que haya decidido realizar la transición de Windows Server Essentials a Windows Server 2012 Standard, siga estos dos pasos:

1. Compre una licencia para Windows Server 2012 Standard y el número adecuado de licencias de acceso de cliente de dispositivo o usuario para su entorno.

    Puede adquirir una licencia para Windows Server 2012 Standard a partir de una versión comercial, un distribuidor o con la ayuda de un [asociado de Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).

   > [!NOTE]
   >  Si adquirió inicialmente Windows Server 2012 Standard y ha ejecutado los derechos de degradación para instalar una de sus dos instancias virtuales como Windows Server Essentials, no es necesario adquirir nada más.
   >
   >  Si adquiere Windows Server 2012 Standard a través del canal de licencias por volumen, puede descargar una imagen ISO y una clave de producto para Windows Server 2012 Standard en el centro de servicios de licencias por volumen (VLSC).
   >
   >  Si adquiere Windows Server 2012 Standard desde el resto de los canales, puede descargar una imagen ISO y una clave de producto de evaluación para Windows Server Essentials en el [centro de evaluación de TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Al realizar la transición tal y como se describe en el siguiente paso, el producto de evaluación se convertirá en un producto con licencia y soporte completos.

2. Abra Windows PowerShell como administrador y ejecute el siguiente comando.

    **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Clave de producto*

    Donde *clave de producto* es la clave de producto de su copia de Windows Server 2012 Standard.

    El servidor se reinicia para terminar el proceso de transición.

   Después de la transición, las características de Windows Server Essentials permanecen en el servidor y se admiten hasta 75 usuarios y 75 dispositivos. Si supera alguno de estos límites, debe usar las herramientas nativas de Windows Server 2012 Standard para administrar cuentas de usuario y dispositivos.

   Además, después de realizar la transición a Windows Server 2012 Standard, las características multimedia de Windows Server Essentials ya no están disponibles. Esto incluye las características multimedia de Acceso web remoto y la configuración multimedia del panel.

## <a name="turn-off--windows-server-essentials-features"></a>Desactivar las características de Windows Server Essentials
 Si ya no necesita el panel de Windows Server Essentials ni otras características para administrar el servidor, puede desactivarlas y quitarlas de su servidor.

 El **Asistente para desactivar características de Windows Server Essentials:**

- ayuda a desinstalar las características de. También limpia el servidor de archivos creados por el software de servidor de Windows Server Essentials.  Algunas operaciones de limpieza se realizan inmediatamente, mientras que otras se inician después de reiniciar el servidor.

- requiere que desinstale manualmente todos los complementos para poder completar el asistente. Para ver una lista de los complementos instalados, abra la página Aplicación en el panel. El asistente le alerta si detecta complementos instalados y le pide que los desinstale.

- permite elegir si se deben conservar los archivos de copia de seguridad de los equipos cliente después de desactivar las características de Windows Server Essentials.

 Hay dos maneras de ejecutar el **Asistente para la desactivación de características de Windows Server Essentials** desde el panel:

#### <a name="from-the-alert"></a>Desde la alerta

1.  En el panel, abra el Visor de alertas.

2.  En la lista organizar, seleccione la alerta que informa sobre la desactivación de las características de Windows Server Essentials después de la transición.

3.  En la alerta, haga clic en **desactivar las características de Windows Server Essentials**.

#### <a name="from-the-get-help-and-support-pane"></a>Desde el panel Get Help and Support (Obtener Ayuda y soporte)

1. En la página Home (principal), haga clic en Get Help and Support (Obtener Ayuda y soporte).

2. Haga clic en **desactivar el Asistente para características de Windows Server Essentials**.

   Es posible que algunas tareas realizadas por el Asistente para la desactivación de **características de Windows Server Essentials** no se completen correctamente. En algunos casos, esto puede impedir que el panel se ejecute. Si esto ocurre, puede iniciar el asistente manualmente ejecutando el archivo:

   **%SystemDrive%\Archivos de Programa\windows Server\Bin\TurnOffFeaturesWizard.exe**

## <a name="additional-references"></a>Referencias adicionales


-   [Transición a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)

-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

