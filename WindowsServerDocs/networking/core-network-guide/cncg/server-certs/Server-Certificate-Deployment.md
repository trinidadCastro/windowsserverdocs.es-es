---
title: Implementación de certificados de servidor
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858496"
---
# <a name="server-certificate-deployment"></a>Implementación de certificados de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para instalar una entidad de certificación de raíz (CA) empresarial e implementar certificados de servidor para su uso con PEAP y EAP.  
  
> [!IMPORTANT]  
> Antes de instalar servicios de certificados de Active Directory, debe el nombre del equipo, configure el equipo con una dirección IP estática y unir el equipo al dominio. Después de instalar AD CS, no se puede cambiar el nombre del equipo o la pertenencia al dominio del equipo, sin embargo, puede cambiar la dirección IP si es necesario. Para obtener más información sobre cómo realizar estas tareas, vea Windows Server&reg; 2016 [Guía de red principal](../../Core-Network-Guide.md).  

  
-   [Instalar el WEB1 de servidor Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Crear un registro de alias (CNAME) en DNS para WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurar WEB1 para distribuir listas de revocación de certificados (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparar el archivo inf CAPolicy](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Instalar la entidad de certificación](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurar las extensiones CDP y AIA en CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copie el certificado de CA y la CRL en el directorio virtual](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurar la plantilla de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurar la inscripción automática de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Actualizar directiva de grupo](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Compruebe el servidor de inscripción de un certificado de servidor](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Los procedimientos de esta guía no incluyen instrucciones para los casos en que se abre el cuadro de diálogo **Control de cuentas de usuario** para solicitar permiso para continuar. Si aparece este cuadro de diálogo en respuesta a sus acciones mientras realiza los procedimientos de esta guía, haga clic en **Continuar**.  
  


