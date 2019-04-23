---
title: El número de ejecución o debe ser máquinas virtuales configuradas dentro de los límites admitidos
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a971a48b2d8199a6c279f1bd3f1715039fa6e0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855356"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>El número de ejecución o debe ser máquinas virtuales configuradas dentro de los límites admitidos

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Más máquinas virtuales están configuradas que son compatibles o en ejecución.*  
  
## <a name="impact"></a>Impacto  
*Microsoft no admite el número actual de las máquinas virtuales ejecutan o configurado en este servidor.*  
  
## <a name="resolution"></a>Resolución  
*Mover una o más máquinas virtuales a otro servidor.*  
  
Para obtener más información sobre configuraciones admitidas máximas para Hyper-V, como el número de máquinas virtuales en funcionamiento, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Para mover una máquina virtual a otro servidor, hacer lo siguiente:  
  
- Exporte la máquina virtual desde el servidor actual y, a continuación, importarlo a un nuevo servidor como se describe a continuación.   
- Realizar una migración en vivo:   
    - Si este servidor pertenece a un clúster de conmutación por error, use las herramientas proporcionadas con la característica de clústeres de conmutación por error. Para obtener instrucciones, consulte [migración en vivo, migración rápida o mover una máquina Virtual de nodo a nodo](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - Si se trata de un servidor independiente, consulte las instrucciones en [configurar migración en vivo y migración de máquinas virtuales sin agrupación en clústeres de conmutación por error](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Para exportar una máquina virtual  
  
   > [!IMPORTANT]  
   > Si el host de Hyper-V que se va a exportar desde pertenece a un dominio y desea almacenar los archivos exportados en una ubicación remota, el host de Hyper-V debe configurarse para la delegación restringida. Una ubicación remota podría ser una carpeta de red compartida o una carpeta en el host que está importando. La delegación restringida permite que la cuenta de equipo del host de Hyper-V proporcione credenciales delegadas para el servicio de sistema de archivos Internet comunes (CIFS) al equipo remoto. Para obtener instrucciones sobre cómo configurar la delegación restringida, vea la sección siguiente de la exportación e importe las instrucciones, a continuación.  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, bajo **máquinas virtuales**, haga clic en una máquina virtual y, a continuación, haga clic en **exportar**.  
  
3.  En el **exportar una máquina Virtual** cuadro de diálogo, escriba o desplácese hasta una ubicación que tenga suficiente espacio libre para almacenar todos los recursos de máquina virtual. Al exportar una máquina virtual, se copian todos los discos duros virtuales (archivos .vhd o .vhdx archivos), los puntos de control (archivos .avhd) y los archivos de estado guardado asociados con la máquina virtual en la carpeta especificada.  
  
4.  Haga clic en **Exportar**.  
  
Después de exportar las máquinas virtuales, importe las máquinas virtuales en el otro servidor.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Para importar una máquina virtual a otro servidor  
  
1.  Conectarse al servidor que ejecuta Hyper-V y abra el Administrador de Hyper-V.  
  
2.  En el **acción** panel, haga clic en **Importar máquina Virtual**.  
  
3.  En el **Importar máquina Virtual** diálogo cuadro, especifique la ubicación donde desea exportar la máquina virtual. A menos que desee volver a importar esta máquina virtual, deje la configuración de importación, tal como están.  
  
4.  Haga clic en **Importar**.  
  
### <a name="to-configure-constrained-delegation"></a>Para configurar la delegación restringida  
  
Pertenencia a la **los administradores de dominio** grupo es necesario para completar este procedimiento.  
  
1.  En un equipo que tenga instalada, en la característica herramientas de servicios de dominio de Active Directory **herramientas administrativas**, abra **equipos y usuarios de Active Directory**y, a continuación, vaya a la cuenta de equipo el equipo que ejecuta Hyper-V.  
  
    > [!NOTE]  
    > Si **Usuarios y equipos de Active Directory** no aparece, instale la función Servicios de dominio de Active Directory. Para obtener instrucciones, consulte [instalar herramientas de administración remota de AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  Haga clic en la cuenta de equipo para el equipo que ejecuta Hyper-V y, a continuación, haga clic en **propiedades**.  
  
3.  En la ficha **Delegación**, haga clic en **Seleccionar este equipo para la delegación sólo a los servicios especificados** y después haga clic en **Usar cualquier protocolo de autenticación**.  
  
4.  Para permitir que la cuenta de equipo de Hyper-V presentar credenciales delegadas para el equipo remoto:  
  
    1.  Haz clic en **Agregar**.  
  
    2.  En el **agregar servicios** cuadro de diálogo, haga clic en **usuarios o equipos**, seleccione el equipo remoto y, a continuación, haga clic en **Aceptar**.  
  
    3.  En el **servicios disponibles** lista, seleccione el **cifs** (también conocido como el protocolo bloque de mensajes del servidor (SMB)) del protocolo y, a continuación, haga clic en **agregar**.  
  
  
  


