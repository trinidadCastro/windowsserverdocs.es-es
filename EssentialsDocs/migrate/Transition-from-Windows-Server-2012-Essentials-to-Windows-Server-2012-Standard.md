---
title: Transición de Windows Server Essentials a Windows Server 2012 Standard
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882506"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transición de Windows Server Essentials a Windows Server 2012 Standard

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials admite hasta 25 usuarios y 50 dispositivos. Cuando sus necesidades empresariales superan el límite, puede realizar una transición de licencias local de Windows Server Essentials a Windows Server 2012 Standard para seguir cumpliéndolas.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Cómo afecta la transición al límite de usuarios y dispositivos  
 Después de realizar la transición a Windows Server 2012 Standard, se eliminan los límites de dispositivos y la cuenta de usuario, pero las características que son exclusivas de Windows Server Essentials (por ejemplo, el panel, acceso Web remoto y copias de seguridad de equipos cliente), siguen estando disponibles. Sin embargo, las limitaciones técnicas de estas características permiten un máximo de 75 cuentas de usuario y 75 dispositivos. Si es necesario agregar más de 75 cuentas de usuario o dispositivos, debe desactivar las características de Windows Server Essentials y utilizar las herramientas nativas de Windows Server 2012 Standard para administrar dispositivos y las cuentas de usuario.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo de su entorno. Esto es diferente de Windows Server Essentials, que no utiliza el modelo CAL y no incluye ninguna CAL.  Al realizar la transición desde Windows Server Essentials a Windows Server 2012 Standard, deberá comprar el número adecuado y el tipo de CAL para su entorno (la mayoría de los clientes compra CAL de usuario).  
  
## <a name="before-the-transition"></a>Antes de la transición  
  
-   Antes de realizar la transición desde Windows Server Essentials a Windows Server 2012 Standard, se deberían totalmente la copia de seguridad de los datos del servidor.  
  
    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no podrá restaurar el servidor al estado que tenía antes de la transición.  
  
-   Además, asegúrese de leer y comprender el contrato de licencia de usuario final (CLUF) para Windows Server 2012 Standard. Para ver los términos de licencia:  
  
    1.  Abra una ventana del símbolo del sistema como administrador.  
  
    2.  Ejecute el siguiente comando:  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         Donde **ruta de términos de licencia** representa la ubicación donde desea guardar al archivos de los términos de licencia. Por ejemplo: C:\ws8std_eula.rtf.  Asegúrese de usar .rtf como extensión de archivo.  
  
    3.  Abra la ubicación donde guardó el archivo y haga doble clic en él para abrirlo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transición a Windows Server 2012 Standard  
 Después de haber decidido realizar la transición desde Windows Server Essentials a Windows Server 2012 Standard, complete estos dos pasos:  
  
1.  Adquiera una licencia para Windows Server 2012 Standard y el número apropiado de licencias de acceso de cliente de usuario y/o dispositivo para su entorno.  
  
     Puede comprar una licencia para Windows Server 2012 Standard desde una tienda, un distribuidor, o con la Ayuda de un [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si inicialmente compró Windows Server 2012 Standard y ejercer sus derechos de degradación de instalar uno de sus dos instancias virtuales como Windows Server Essentials, no necesita comprar nada adicional.  
    >   
    >  Si adquiere Windows Server 2012 Standard a través del canal de licencias por volumen, puede descargar una imagen ISO y una clave de producto para Windows Server 2012 Standard desde el centro de servicio de licencias de volumen (VLSC).  
    >   
    >  Si adquiere Windows Server 2012 Standard de todos los otros canales puede descargar una imagen ISO y una clave de producto de evaluación para Windows Server Essentials desde el [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Al realizar la transición tal y como se describe en el siguiente paso, el producto de evaluación se convertirá en un producto con licencia y soporte completos.  
  
2.  Abra Windows PowerShell como administrador y ejecute el siguiente comando.  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Clave de producto*  
  
     Donde *clave de producto* es la clave de producto de su copia de Windows Server 2012 Standard.  
  
     El servidor se reinicia para terminar el proceso de transición.  
  
 Después de la transición, las características de Windows Server Essentials permanecen en el servidor y son compatibles con hasta 75 usuarios y 75 dispositivos. Si excede cualquiera de estos límites, debe usar las herramientas nativas de Windows Server 2012 Standard para administrar dispositivos y las cuentas de usuario.  
  
 Además, después de realizar la transición a Windows Server 2012 Standard, ya no están disponibles las características multimedia de Windows Server Essentials. Esto incluye las características multimedia de Acceso web remoto y la configuración multimedia del panel.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Desactivar las características de Windows Server Essentials  
 Si ya no necesita el panel de Windows Server Essentials ni otras características para administrar el servidor, puede desactivar las características y quitarlas del servidor.  
  
 El **desactivar al Asistente para características de Windows Server Essentials** ayuda a desinstalar las características. También limpia el servidor de archivos creados por el software de servidor de Windows Server Essentials.  Algunas operaciones de limpieza se realizan inmediatamente, mientras que otras se inician después de reiniciar el servidor.  
  
 El **desactivar al Asistente para características de Windows Server Essentials** necesario desinstalar manualmente todos los complementos para poder completar el asistente. Para ver una lista de los complementos instalados, abra la página Aplicación en el panel. El asistente le alerta si detecta complementos instalados y le pide que los desinstale.  
  
 El **desactivar al Asistente para características de Windows Server Essentials** le permite elegir si desea mantener los equipos de los archivos de copia de seguridad de cliente después de desactivar las características de Windows Server Essentials.  
  
 Hay dos maneras de ejecutar el **desactivar al Asistente para características de Windows Server Essentials** desde el panel:  
  
#### <a name="from-the-alert"></a>Desde la alerta  
  
1.  En el panel, abra el Visor de alertas.  
  
2.  En la lista Organizar, seleccione la alerta que contiene información sobre cómo desactivar características de Windows Server Essentials después de la transición.  
  
3.  En la alerta, haga clic en **desactivar las características de Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>Desde el panel Get Help and Support (Obtener Ayuda y soporte)  
  
1.  En la página Home (principal), haga clic en Get Help and Support (Obtener Ayuda y soporte).  
  
2.  Haga clic en **desactivar al Asistente para características de Windows Server Essentials**.  
  
 Es posible que algunas de las tareas realizadas por el **desactivar al Asistente para características de Windows Server Essentials** no se completará correctamente. En algunos casos, esto puede impedir que el panel se ejecute. Si esto ocurre, puede iniciar el asistente manualmente ejecutando el archivo:  
  
 **%SystemDrive%\Program programa\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Vea también  
  

-   [Transición a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transición a Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar datos del servidor a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

