---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importar un certificado de autenticación de servidor al sitio web predeterminado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b1c602d0cdfa562469419de223f5691ec2ff4527
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359560"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importar un certificado de autenticación de servidor al sitio web predeterminado

Después de obtener un certificado de autenticación de servidor de una entidad de certificación \(CA\), debe instalar manualmente dicho certificado en el sitio web predeterminado para cada servidor de Federación o servidor proxy de Federación en una granja de servidores.  
  
En el caso de los servidores web, debes instalar manualmente el certificado de autenticación de servidor en el sitio web correspondiente o en el directorio virtual donde se encuentra la aplicación federada.  
  
Si estás configurando una granja, asegúrate de realizar este procedimiento de forma idéntica (usando exactamente la misma configuración) en todos los servidores de la granja.  
  
> [!NOTE]  
> El\-de administración de AD FS en se refiere a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Para importar un certificado de autenticación de servidor al sitio web predeterminado  
  
1.  En la pantalla **Inicio** , escriba**Internet Information Services \(administrador de\) IIS**y, a continuación, presione Entrar.  
  
2.  En el árbol de consola, haz clic en **nombreDeEquipo**.  
  
3.  En el panel central, haga doble\-haga clic en **certificados de servidor**.  
  
4.  En el panel **Acciones** , haz clic en **Importar**.  
  
5.  En el cuadro de diálogo **importar certificado** , haga clic en **...** de puntos suspensivos (...).  
  
6.  Ve a la ubicación del archivo de certificado pfx, resáltalo y haz clic en **Abrir**.  
  
7.  Escribe la contraseña del certificado y, después, haz clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para servidores proxy de federación](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

