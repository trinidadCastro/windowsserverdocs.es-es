---
title: Transición de Windows Server Essentials a Windows Server 2012 R2 Standard
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840086"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Transición de Windows Server Essentials a Windows Server 2012 R2 Standard

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 es el sistema operativo en la nube listo para que admita las cargas de trabajo actuales durante la introducción de nuevas tecnologías que facilitan la transición a la informática en nube. Contenido de Windows Server 2016 le ayuda a empezar listo.

 Windows Server Essentials admite hasta 25 usuarios y 50 dispositivos. Cuando sus necesidades empresariales superan el límite, puede realizar una transición de licencias local de Windows Server Essentials a Windows Server 2012 R2 Standard para seguir cumpliéndolas.  
  
 Después de realizar la transición a Windows Server 2012 R2 Standard, se eliminan los límites de dispositivos y la cuenta de usuario, pero siguen estando disponibles las características que son exclusivas de Windows Server Essentials (por ejemplo, el panel, acceso Web remoto y copias de seguridad de equipo cliente). Sin embargo, las limitaciones técnicas de estas características permiten un máximo de 100 cuentas de usuario y 200 dispositivos. La función de copia de seguridad del equipo cliente admitirá la copia de seguridad de hasta 75 dispositivos.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard requiere una licencia de acceso de cliente (CAL) para cada usuario o dispositivo de su entorno. Esto es diferente de Windows Server Essentials, que no utiliza el modelo CAL y no incluye ninguna CAL. Al realizar la transición desde Windows Server Essentials a Windows Server 2012 R2 Standard, deberá comprar el número adecuado y el tipo de CAL para su entorno (la mayoría de los clientes compra CAL de usuario).  
  
## <a name="before-the-transition"></a>Antes de la transición  
  
-   Antes de realizar la transición desde Windows Server Essentials a Windows Server 2012 R2 Standard, se deberían totalmente la copia de seguridad de los datos del servidor.  
  
    > [!IMPORTANT]
    >  Sin una copia de seguridad completa del servidor, no podrá restaurar el servidor al estado que tenía antes de la transición.  
  
-   Además, asegúrese de leer y comprender el contrato de licencia de usuario final (CLUF) para Windows Server 2012 R2 Standard. Para ver los términos de licencia:  
  
    1.  Abra una ventana del símbolo del sistema como administrador.  
  
    2.  Ejecute el siguiente comando:  
  
         **DISM / /Online/set-edition: ServerStandard/geteula:** *ruta del CLUF* (donde *ruta del CLUF* representa la ubicación a la que desea guardar el archivo del CLUF; por ejemplo: C:\ws8std_eula.rtf). Asegúrese de usar .rtf como extensión de archivo.  
  
    3.  Abra la ubicación donde guardó el archivo y haga doble clic en él para abrirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transición a Windows Server 2012 R2 Standard  
 Después de haber decidido realizar la transición desde Windows Server Essentials a Windows Server 2012 R2 Standard, complete estos dos pasos:  
  
1.  Adquiera una licencia para Windows Server 2012 R2 Standard y el número apropiado de licencias de acceso de cliente de dispositivo para su entorno y los usuarios.  
  
     Puede comprar una licencia para Windows Server 2012 R2 Standard desde una tienda, un distribuidor, o con la Ayuda de un [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Si inicialmente compró Windows Server 2012 R2 Standard y ejercer sus derechos de degradación de instalar uno de sus dos instancias virtuales como Windows Server Essentials, no necesita comprar nada adicional.  
    >   
    >  Si adquiere Windows Server 2012 R2 Standard a través del canal de licencias por volumen, puede descargar una imagen ISO y una clave de producto para Windows Server 2012 R2 Standard desde el centro de servicio de licencias de volumen (VLSC).  
    >   
    >  Si adquiere Windows Server 2012 R2 Standard en cualquiera de los otro canales, puede descargar una imagen ISO y una clave de producto de evaluación para Windows Server Essentials desde el [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Al realizar la transición tal y como se describe en el siguiente paso, el producto de evaluación se convertirá en un producto con licencia y soporte completos.  
  
2.  Abra Windows PowerShell como administrador y ejecute el siguiente comando:  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Clave de producto* (donde *clave de producto* es la clave de producto de su copia de Windows Server 2012 R2 Standard).  
  
     El servidor se reinicia para terminar el proceso de transición.  
  
 Después de la transición, las características de Windows Server Essentials se admiten para hasta 100 usuarios y 200 dispositivos y permanecen en el servidor.  
  
## <a name="see-also"></a>Vea también  
  

-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar datos del servidor a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

