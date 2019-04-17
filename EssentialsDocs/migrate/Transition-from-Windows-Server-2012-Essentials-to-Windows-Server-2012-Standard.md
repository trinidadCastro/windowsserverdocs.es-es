---
title: "Transición de Windows Server Essentials para Windows Server 2012 estándar"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transición de Windows Server Essentials para Windows Server 2012 estándar

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials admite hasta 25 usuarios y dispositivos de 50. Cuando tu negocio necesita exceda el límite, puedes realizar una transición de licencia en lugar de Windows Server Essentials para Windows Server 2012 Standard permanezca compatible con licencia.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Cómo afecta a la transición de usuarios y dispositivos limita  
 Después de que la transición a Windows Server 2012 Standard, se quitan los límites de dispositivos y la cuenta de usuario, pero las características que son únicas para Windows Server Essentials (por ejemplo, el panel, acceso Web remoto y copia de seguridad de cliente equipo), permanezcan disponibles. Sin embargo, las limitaciones técnicas de estas características admiten un máximo de 75 cuentas de usuario y 75 dispositivos. Si es necesario agregar más de 75 cuentas de usuario o dispositivos, debes desactivar las características de Windows Server Essentials y usar las herramientas nativas de Windows Server 2012 Standard para administrar dispositivos y cuentas de usuario.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo en su entorno. Esto es diferente de Windows Server Essentials, que no usa el modelo CAL y no se incluye en cualquier CAL.  Al realizar la transición desde Windows Server Essentials para Windows Server 2012 Standard, tendrás que comprar el número adecuado y el tipo de CAL para su entorno (CAL de usuario de compra de la mayoría de los clientes).  
  
## <a name="before-the-transition"></a>Antes de la transición  
  
-   Antes de realizar la transición desde Windows Server Essentials para Windows Server 2012 Standard, se deben completamente copia de seguridad de los datos del servidor.  
  
    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no puede restaurar el servidor al estado que tenía antes de la transición.  
  
-   Además, asegúrate de que puedes leerán y comprensión el contrato de licencia de usuario final (CLUF) para Windows Server 2012 Standard. Para ver al CLUF:  
  
    1.  Abre una ventana comando como administrador.  
  
    2.  Ejecuta el siguiente comando:  
  
         **DISM / en línea/establece-edition: ServerStandard /geteula: ruta de acceso de CLUF**  
  
         Donde **ruta de acceso de CLUF** representa la ubicación a la que quieres guardar el archivo de CLUF. Por ejemplo; C:\ws8std_eula.rtf.  Asegúrate de usar .rtf como la extensión de nombre de archivo.  
  
    3.  Abre la ubicación donde guardaste el archivo y, a continuación, haz doble clic en el archivo para abrirlo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transición a estándar de Windows Server 2012  
 Una vez haya decidido realizar la transición desde Windows Server Essentials para Windows Server 2012 Standard, completa estos dos pasos:  
  
1.  Adquirir una licencia para Windows Server 2012 Standard y el número apropiado de licencias de acceso de cliente de usuario o dispositivo para su entorno.  
  
     Puedes comprar una licencia para Windows Server 2012 Standard desde una tienda, un distribuidor, o con la Ayuda de un [Partner de Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si has adquirido Windows Server 2012 Standard inicialmente y ejercitar los derechos para cambiar a instalar una de las dos instancias virtuales como Windows Server Essentials, no es necesario comprar nada adicional.  
    >   
    >  Si has comprado Windows Server 2012 Standard a través del canal de licencias por volumen, puedes descargar una imagen ISO y una clave de producto de Windows Server 2012 Standard desde el centro de servicio de licencias de volumen (VLSC).  
    >   
    >  Si compras estándar de Windows Server 2012 desde todos los demás canales puede descargar una imagen ISO y una clave de producto de la evaluación de Windows Server Essentials de los [centro de evaluación de TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Realizar la transición, como se describe en el siguiente paso convertirá el producto de evaluación en un producto compatible y totalmente con licencia.  
  
2.  Abre Windows PowerShell como administrador y, a continuación, ejecuta el siguiente comando.  
  
     **DISM / en línea/establece-edition: ServerStandard /accepteula /productkey:** *clave de producto*  
  
     Donde *clave de producto* es la clave de producto para la copia de Windows Server 2012 Standard.  
  
     Reinicia el servidor para finalizar el proceso de transición.  
  
 Después de la transición, las características de Windows Server Essentials se admiten para hasta 75 usuarios y 75 dispositivos y permanecen en el servidor. Si excedes cualquiera de estos límites, debes usar las herramientas nativas de Windows Server 2012 Standard para administrar dispositivos y cuentas de usuario.  
  
 Además, después de que la transición a Windows Server 2012 Standard, ya no están disponibles las características de medios de Windows Server Essentials. Esto incluye las características de medios de acceso Web remoto y la configuración de multimedia en el panel.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Desactivar características de Windows Server Essentials  
 Si ya no necesita el escritorio de Windows Server Essentials u otras características de valor agregado para administrar el servidor, puedes desactivar las características y eliminarlos desde el servidor.  
  
 La **desactivar el Asistente para la características de Windows Server Essentials** te ayuda a desinstalar las características. También limpia el servidor de archivos creados por el software de servidor de Windows Server Essentials.  Algunas operaciones de limpieza se ejecutan inmediatamente, mientras que otros se inician después de reiniciar el servidor.  
  
 La **desactivar el Asistente para la características de Windows Server Essentials** requiere desinstalar todos los complementos manualmente para poder completar el asistente. Para ver una lista de complementos instalados, abre la página de la aplicación en el panel. El asistente te avisará si detecta los complementos instalados y le pide que desinstalarlas.  
  
 La **desactivar el Asistente para la características de Windows Server Essentials** te permite elegir si desea mantener archivos de copia de seguridad de cliente equipos después de desactivar las características de Windows Server Essentials.  
  
 Hay dos formas de ejecutar el **desactivar el Asistente para la características de Windows Server Essentials** desde el panel:  
  
#### <a name="from-the-alert"></a>Desde la alerta  
  
1.  En el panel, abre el Visor de alertas.  
  
2.  En la lista de organizar, seleccione la alerta que informa acerca de cómo desactivar las características de Windows Server Essentials después de la transición.  
  
3.  En la alerta, haz clic en **desactivar características de Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>Desde el panel de obtener ayuda y soporte técnico  
  
1.  En la página principal, haz clic en obtener ayuda y soporte técnico.  
  
2.  Haz clic en **desactivar el Asistente para la características de Windows Server Essentials**.  
  
 Es posible que algunas tareas que realiza la **desactivar el Asistente para la características de Windows Server Essentials** no se completará correctamente. En algunos casos, esto puede impedir que el panel de la ejecución. Si esto ocurre, puede iniciar al Asistente manualmente mediante la ejecución del archivo:  
  
 **%SystemDrive%\Program programa\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Consulta también  
  

-   [Transición a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar los datos del servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transición a Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar los datos del servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

