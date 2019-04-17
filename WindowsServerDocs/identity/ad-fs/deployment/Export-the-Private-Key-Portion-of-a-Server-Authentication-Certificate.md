---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: "Exportar la parte de la clave privada de un certificado de autenticación de servidor"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar la parte de la clave privada de un certificado de autenticación de servidor

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada servidor de federación de un conjunto de servicios de federación de Active Directory \(AD FS\) debe tener acceso a la clave privada del certificado de autenticación del servidor. Si estás implementando una granja de servidores de federación servidores o servidores Web, debes tener un certificado de autenticación solo. Este certificado debe ser emitido por una entidad de certificación de la empresa \(CA\) y deben tener una clave privada puede exportar. Para que puedan realizar disponibles para todos los servidores de la granja de servidores, debe ser puede exportar la clave privada del certificado de autenticación del servidor.  
  
Este mismo concepto es true federación proxy conjuntos de servidores en el sentido de que todos los servidores proxy del servidor de federación de un conjunto de deben compartir la parte de la clave privada del certificado de autenticación del mismo servidor.  
  
> [!NOTE]  
> La administración de AD FS en snap\ hace referencia a los certificados de autenticación de servidor para servidores de federación como certificados de comunicación de servicio.  
  
Según la función que se reproducirá a este equipo, usa este procedimiento en el equipo de servidor de federación o el equipo de federación servidor proxy que se instaló el certificado de autenticación de servidor con la clave privada. Cuando termine el procedimiento, a continuación, puede importar este certificado en el sitio Web predeterminado de cada servidor de la batería. Para obtener más información, consulta [importar un certificado de autenticación de servidor en el sitio Web predeterminado](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar la parte de la clave privada de un certificado de autenticación de servidor  
  
1.  En la **inicio** , escriba**\(IIS\) Internet Information Services Manager**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haz clic en **ComputerName**.  
  
3.  En el panel central, double\ clic **certificados de servidor**.  
  
4.  En el panel central, right\ y haga clic en el certificado que quieras exportar y, a continuación, haz clic en **exportar**.  
  
5.  En la **Exportar certificado** cuadro de diálogo, haz clic en el **...** botón.  
  
6.  En **nombre de archivo**, tipo **C:\\***NameofCertificate*y, a continuación, haz clic en **abrir**.  
  
7.  Escribe una contraseña para el certificado, confirmarla y, a continuación, haz clic en **Aceptar**.  
  
8.  Validar el éxito de la exportación confirmar que se crea el archivo especificado en la ubicación especificada.  
  
    > [!IMPORTANT]  
    > Por lo que puede importar este certificado al almacén de certificados local en el servidor nuevo, debe transferir el archivo al medios físicos y proteger su seguridad durante el transporte con el servidor nuevo. Es muy importante proteger la seguridad de la clave privada. Si esta clave se ve comprometida, la seguridad de toda la implementación de AD FS \ (incluidos los recursos dentro de la organización y en el recurso partner organizations\) está en peligro.  
  
9. Importa el certificado de autenticación de servidor exportado en el almacén de certificados en el servidor nuevo antes de instalar el servicio de federación. ¿Para obtener información sobre cómo importar el certificado, consulta importar un certificado de servidor \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para Proxies de servidor de federación](https://technet.microsoft.com/library/dd807054.aspx)  
  

