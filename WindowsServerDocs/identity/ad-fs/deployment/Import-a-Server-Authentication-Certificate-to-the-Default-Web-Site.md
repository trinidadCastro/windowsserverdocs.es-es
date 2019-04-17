---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: "Importar un certificado de autenticación de servidor en el sitio Web predeterminado"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importar un certificado de autenticación de servidor en el sitio Web predeterminado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de obtener un servidor de certificados de autenticación de una entidad de certificación \(CA\), debe instalar manualmente ese certificado en el sitio Web predeterminado para cada servidor de federación o proxy del servidor de federación en una granja de servidores.  
  
Para servidores Web, debes instalar manualmente el certificado de autenticación de servidor en el sitio Web o adecuado directorio virtual donde reside su aplicación federada.  
  
Si estás configurando una granja de servidores, asegúrate de realizar este procedimiento de forma idéntica: con la misma configuración exacta; en cada uno de los servidores de la batería.  
  
> [!NOTE]  
> La administración de AD FS en snap\ hace referencia a los certificados de autenticación de servidor para servidores de federación como certificados de comunicación de servicio.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Para importar un certificado de autenticación de servidor en el sitio Web predeterminado  
  
1.  En la **inicio** , escriba**\(IIS\) Internet Information Services Manager**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haz clic en **ComputerName**.  
  
3.  En el panel central, double\ clic **certificados de servidor**.  
  
4.  En la **acciones** panel, haz clic en **importar**.  
  
5.  En la **importar certificado** cuadro de diálogo, haz clic en el **...** botón.  
  
6.  Busca la ubicación del archivo de certificado pfx, resáltala y, a continuación, haz clic en **abrir**.  
  
7.  Escribe una contraseña para el certificado y, a continuación, haz clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para Proxies de servidor de federación](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

