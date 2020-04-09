---
title: El número de máquinas virtuales en ejecución o configuradas debe estar dentro de los límites admitidos
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 49013d6a4c9dda6e79d6a803bae0f5641d826817
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854628"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>El número de máquinas virtuales en ejecución o configuradas debe estar dentro de los límites admitidos

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto que aparece en la herramienta Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Hay más máquinas virtuales en ejecución o configuradas que se admiten.*  
  
## <a name="impact"></a>Impacto  
*Microsoft no admite el número actual de máquinas virtuales que se ejecutan o se configuran en este servidor.*  
  
## <a name="resolution"></a>Resolución  
*Mueva una o más máquinas virtuales a otro servidor.*  
  
Para obtener más información acerca de las configuraciones máximas admitidas para Hyper-V, como el número de máquinas virtuales en ejecución, consulte [planear la escalabilidad de Hyper-v en Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Para trasladar una máquina virtual a otro servidor, puede:  
  
- Exporte la máquina virtual del servidor actual y, a continuación, impórtela en un nuevo servidor, tal y como se describe a continuación.   
- Realice una migración en vivo:   
    - Si este servidor pertenece a un clúster de conmutación por error, use las herramientas proporcionadas con la característica de clústeres de conmutación por error. Para obtener instrucciones, consulte [migración en vivo, migración rápida o movimiento de una máquina virtual de un nodo a otro](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - Si se trata de un servidor independiente, consulte las instrucciones en [configurar migración en vivo y migrar virtual machines sin clústeres de conmutación por error](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Para exportar una máquina virtual  
  
   > [!IMPORTANT]  
   > Si el host de Hyper-V del que está exportando pertenece a un dominio y desea almacenar los archivos exportados en una ubicación remota, el host de Hyper-V debe estar configurado para la delegación restringida. Una ubicación remota puede ser una carpeta de red compartida o una carpeta del host en el que se va a importar. La delegación restringida permite a la cuenta de equipo del host de Hyper-V proporcionar credenciales delegadas para el servicio del sistema de archivos de Internet común (CIFS) en el equipo remoto. Para obtener instrucciones sobre la configuración de la delegación restringida, consulte la sección siguiente sobre las instrucciones de exportación e importación que se indican a continuación.  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, en **virtual machines**, haga clic con el botón secundario en una máquina virtual y, a continuación, haga clic en **exportar**.  
  
3.  En el cuadro de diálogo **exportar una máquina virtual** , escriba o busque una ubicación que tenga suficiente espacio libre para almacenar todos los recursos de la máquina virtual. Cuando se exporta una máquina virtual, todos los discos duros virtuales (archivos. vhd o. vhdx), los puntos de control (archivos. avhd) y los archivos de estado guardados asociados a la máquina virtual se copian en la carpeta especificada.  
  
4.  Haga clic en **Exportar**.  
  
Después de exportar las máquinas virtuales, importe las máquinas virtuales al otro servidor.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Para importar una máquina virtual a otro servidor  
  
1.  Conéctese al servidor que ejecuta Hyper-V y abra el administrador de Hyper-V.  
  
2.  En el panel **acción** , haga clic en **importar máquina virtual**.  
  
3.  En el cuadro de diálogo **importar máquina virtual** , especifique la ubicación en la que exportó la máquina virtual. A menos que quiera volver a importar esta máquina virtual, deje la configuración de importación tal como están.  
  
4.  Haga clic en **Importar**.  
  
### <a name="to-configure-constrained-delegation"></a>Para configurar la delegación restringida  
  
Para completar este procedimiento, es necesario pertenecer al grupo **administradores de dominio** .  
  
1.  En un equipo que tenga instalada la característica herramientas de Active Directory Domain Services, en **herramientas administrativas**, Abra **Active Directory usuarios y equipos**y, a continuación, navegue a la cuenta de equipo para el equipo que ejecuta Hyper-V.  
  
    > [!NOTE]  
    > Si **Usuarios y equipos de Active Directory** no aparece, instale la función Servicios de dominio de Active Directory. Para obtener instrucciones, consulte [instalación de herramientas de administración remota del servidor para AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  Haga clic con el botón secundario en la cuenta de equipo para el equipo que ejecuta Hyper-V y, a continuación, haga clic en **propiedades**.  
  
3.  En la ficha **Delegación**, haga clic en **Seleccionar este equipo para la delegación sólo a los servicios especificados** y después haga clic en **Usar cualquier protocolo de autenticación**.  
  
4.  Para permitir que la cuenta de equipo de Hyper-V presente credenciales delegadas en el equipo remoto:  
  
    1.  Haga clic en **Agregar**.  
  
    2.  En el cuadro de diálogo **Agregar servicios** , haga clic en **usuarios o equipos**, seleccione el equipo remoto y, a continuación, haga clic en **Aceptar**.  
  
    3.  En la lista **servicios disponibles** , seleccione el protocolo **CIFS** (también conocido como el protocolo de bloque de mensajes del servidor (SMB)) y, a continuación, haga clic en **Agregar**.  
  
  
  


