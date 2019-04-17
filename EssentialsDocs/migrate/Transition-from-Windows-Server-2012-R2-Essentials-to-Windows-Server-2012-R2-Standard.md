---
title: "Transición de Windows Server Essentials para Windows Server 2012 R2 Standard"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transición de Windows Server Essentials para Windows Server 2012 R2 Standard

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 es el sistema operativo de uso de la nube que admite las cargas de trabajo actuales al tiempo que agregaban nuevas tecnologías que hacen más fácil realizar la transición a la informática en nube. Contenido de Windows Server 2016 le ayuda a Listo.

 Windows Server Essentials admite hasta 25 usuarios y dispositivos de 50. Cuando tu negocio necesita exceda el límite, puedes realizar una transición de licencia en lugar de Windows Server Essentials para Windows Server 2012 R2 Standard permanezca compatible con licencia.  
  
 Después de que la transición a Windows Server 2012 R2 Standard, se quitan los límites de dispositivos y la cuenta de usuario, pero las características que son únicas para Windows Server Essentials (por ejemplo, el panel del acceso Web remoto y copia de seguridad de equipo cliente) permanezcan disponibles. Sin embargo, las limitaciones técnicas de estas características admiten un máximo de dispositivos de 200 y 100 cuentas de usuario. La funcionalidad de copia de seguridad del equipo cliente será compatible con la copia de seguridad de dispositivos hasta 75.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo en su entorno. Esto es diferente de Windows Server Essentials, que no usa el modelo CAL y no se incluye en cualquier CAL. Al realizar la transición desde Windows Server Essentials para Windows Server 2012 R2 Standard, tendrás que comprar el número adecuado y el tipo de CAL para su entorno (CAL de usuario de compra de la mayoría de los clientes).  
  
## <a name="before-the-transition"></a>Antes de la transición  
  
-   Antes de realizar la transición desde Windows Server Essentials para Windows Server 2012 R2 Standard, se deben completamente la copia de seguridad de los datos del servidor.  
  
    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no puede restaurar el servidor al estado que tenía antes de la transición.  
  
-   Además, asegúrate de que puedes leerán y comprensión el contrato de licencia de usuario final (CLUF) para Windows Server 2012 R2 Standard. Para ver al CLUF:  
  
    1.  Abre una ventana comando como administrador.  
  
    2.  Ejecuta el siguiente comando:  
  
         **DISM / en línea/establece-edition: ServerStandard /geteula:** *ruta de acceso de CLUF* (donde *ruta de acceso de CLUF* representa la ubicación a la que quieres guardar el archivo de CLUF; por ejemplo: C:\ws8std_eula.rtf). Asegúrate de usar .rtf como la extensión de nombre de archivo.  
  
    3.  Abre la ubicación donde guardaste el archivo y, a continuación, haz doble clic en el archivo para abrirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transición a Windows Server 2012 R2 Standard  
 Una vez haya decidido realizar la transición desde Windows Server Essentials para Windows Server 2012 R2 Standard, completa estos dos pasos:  
  
1.  Adquirir una licencia para Windows Server 2012 R2 Standard y el número de licencias de acceso de cliente de dispositivo para el entorno y de usuario adecuado.  
  
     Puedes comprar una licencia para Windows Server 2012 R2 Standard desde una tienda, un distribuidor, o con la Ayuda de un [Partner de Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si has adquirido Windows Server 2012 R2 Standard inicialmente y ejercitar los derechos para cambiar a instalar una de las dos instancias virtuales como Windows Server Essentials, no es necesario comprar nada adicional.  
    >   
    >  Si has comprado Windows Server 2012 R2 Standard a través del canal de licencias por volumen, puedes descargar una imagen ISO y una clave de producto de Windows Server 2012 R2 Standard desde el centro de servicio de licencias de volumen (VLSC).  
    >   
    >  Si has comprado Windows Server 2012 R2 Standard de ningún otro canal, puedes descargar una imagen ISO y una clave de producto de la evaluación para Windows Server Essentials desde el [centro de evaluación de TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Realizar la transición, como se describe en el siguiente paso convertirá el producto de evaluación en un producto compatible y totalmente con licencia.  
  
2.  Abre Windows PowerShell como administrador y, a continuación, ejecuta el siguiente comando:  
  
     **DISM / en línea/establece-edition: ServerStandard /accepteula /productkey:** *clave de producto* (donde *clave de producto* es la clave de producto para la copia de Windows Server 2012 R2 Standard).  
  
     Reinicia el servidor para finalizar el proceso de transición.  
  
 Después de la transición, las características de Windows Server Essentials permanecen en el servidor y son compatibles con hasta 100 usuarios y dispositivos de 200.  
  
## <a name="see-also"></a>Consulta también  
  

-   [Migrar los datos del servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar los datos del servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

