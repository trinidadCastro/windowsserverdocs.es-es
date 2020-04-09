---
title: Transición de Windows Server Essentials a Windows Server 2012 R2 Standard
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f7914ff205382ed2c74cb130061f850e2c0675f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852298"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transición de Windows Server Essentials a Windows Server 2012 R2 Standard

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 es el sistema operativo preparado para la nube que es compatible con las cargas de trabajo actuales y presenta nuevas tecnologías que facilitan la transición a la informática en la nube. El contenido de Windows Server 2016 le ayuda a estar listo.

 Windows Server Essentials admite hasta 25 usuarios y 50 dispositivos. Si sus necesidades empresariales superan el límite, puede realizar una transición de licencias local de Windows Server Essentials a Windows Server 2012 R2 Standard para mantener la licencia conforme.  
  
 Después de realizar la transición a Windows Server 2012 R2 Standard, se quitan los límites de cuentas de usuario y dispositivos, pero las características que son exclusivas de Windows Server Essentials (como el panel, el acceso Web remoto y la copia de seguridad de equipos cliente) siguen estando disponibles. Sin embargo, las limitaciones técnicas de estas características permiten un máximo de 100 cuentas de usuario y 200 dispositivos. La función de copia de seguridad del equipo cliente admitirá la copia de seguridad de hasta 75 dispositivos.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo de su entorno. Esto es diferente de Windows Server Essentials, que no usa el modelo CAL y no incluye ninguna cal. Al realizar la transición de Windows Server Essentials a Windows Server 2012 R2 Standard, deberá comprar el número y tipo de cal adecuados para su entorno (la mayoría de los clientes compran cal de usuario).  
  
## <a name="before-the-transition"></a>Antes de la transición  
  
-   Antes de realizar la transición de Windows Server Essentials a Windows Server 2012 R2 Standard, debe hacer una copia de seguridad completa de los datos del servidor.  
  
    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no podrá restaurar el servidor al estado que tenía antes de la transición.  
  
-   Además, asegúrese de leer y comprender el contrato de licencia para el usuario final (CLUF) para Windows Server 2012 R2 Standard. Para ver los términos de licencia:  
  
    1.  Abra una ventana de comandos como administrador.  
  
    2.  Ejecuta el siguiente comando:  
  
         **DISM/online/Set-Edition: ServerStandard/geteula:** *EULA path* (donde *ruta del CLUF* representa la ubicación en la que desea guardar el archivo del cluf; por ejemplo: c:\ ws8std_eula. rtf). Asegúrese de usar .rtf como extensión de archivo.  
  
    3.  Abra la ubicación donde guardó el archivo y haga doble clic en él para abrirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transición a Windows Server 2012 R2 Standard  
 Una vez que haya decidido realizar la transición de Windows Server Essentials a Windows Server 2012 R2 Standard, siga estos dos pasos:  
  
1. Compre una licencia para Windows Server 2012 R2 Standard y el número adecuado de licencias de acceso de cliente de dispositivo o usuario para su entorno.  
  
    Puede adquirir una licencia para Windows Server 2012 R2 Standard a partir de una tienda comercial, un distribuidor o con la ayuda de un [asociado de Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Si adquirió inicialmente Windows Server 2012 R2 Standard y ha ejecutado los derechos de degradación para instalar una de sus dos instancias virtuales como Windows Server Essentials, no es necesario adquirir nada más.  
   >   
   >  Si adquiere Windows Server 2012 R2 Standard a través del canal de licencias por volumen, puede descargar una imagen ISO y una clave de producto para Windows Server 2012 R2 Standard en el centro de servicios de licencias por volumen (VLSC).  
   >   
   >  Si adquiere Windows Server 2012 R2 Standard desde cualquier otro canal, puede descargar una imagen ISO y una clave de producto de evaluación para Windows Server Essentials en el [centro de evaluación de TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Al realizar la transición tal y como se describe en el siguiente paso, el producto de evaluación se convertirá en un producto con licencia y soporte completos.  
  
2. Abra Windows PowerShell como administrador y ejecute el siguiente comando:  
  
    **DISM/online/Set-Edition: ServerStandard/ACCEPTEULA/ProductKey:** *clave* de producto (donde *Product Key* es la clave de producto de su copia de Windows Server 2012 R2 Standard).  
  
    El servidor se reinicia para terminar el proceso de transición.  
  
   Después de la transición, las características de Windows Server Essentials permanecen en el servidor y se admiten hasta 100 usuarios y 200 dispositivos.  
  
## <a name="see-also"></a>Vea también  
  

-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar datos del servidor a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

