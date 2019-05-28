---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e3d1e5d86afbef2fdabd211047f513d31a40300
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190325"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado

En muchas redes de las directivas de firewall local no es posible que permitir el tráfico a través de puertos no estándares, como 49443. Esto se convirtió en un problema al intentar realizar la autenticación de certificados con AD FS antes de AD FS en Windows Server 2016. Esto es porque no podría tener enlaces diferentes para la autenticación de dispositivo y la autenticación de certificados de usuario en el mismo host. El puerto predeterminado 443 está enlazado a recibir certificados de dispositivo y no se puede modificar para admitir el enlace de varios en el mismo canal. El resultado fue que no funcionará la autenticación mediante tarjeta inteligente y los usuarios eran conscientes de lo que sucedió porque no hay ninguna indicación de lo que realmente sucedió.  
  
Con AD FS en Windows Server 2016 Esto puede realizarse.
  
En AD FS en Windows Server 2016 ha cambiado. Ahora se admiten dos modos, el primero usa el mismo host (es decir, adfs.contoso.com) con puertos diferentes (443, 49443). El segundo usa distintos hosts (adfs.contoso.com y certauth.adfs.contoso.com) con el mismo puerto (443). Esto requerirá un certificado SSL para admitir "certauth. < adfs-service-name >" como nombre de sujeto alternativo. Esto puede hacerse en el momento de la creación de la granja de servidores o más adelante a través de PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Cómo configurar el enlace del nombre de host alternativo para la autenticación de certificado  
Hay dos formas que puede agregar el enlace del nombre de host alternativo para la autenticación de certificado. La primera es al configurar una nueva granja de AD FS con AD FS para Windows Server 2016, si el certificado contiene un nombre alternativo del sujeto (SAN), a continuación, éste será automáticamente para que use el segundo método mencionado anteriormente. Es decir, realizará automáticamente la instalación de dos hosts distintos (sts.contoso.com y certauth.sts.contoso.com con el mismo puerto. Si el certificado no contiene una SAN, verá una advertencia indicando que los nombres alternativos del sujeto de certificado no admite certauth.*. Vea las capturas de pantalla siguiente. La primera de ellas muestra una instalación donde el certificado tuviera una SAN y el segundo muestra un certificado que no lo hizo.  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Del mismo modo, una vez que se ha implementado AD FS en Windows Server 2016 puede usar el cmdlet de PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Cuando se le solicite, haga clic en Sí para confirmar.  Y eso sería.

![enlace de nombre de host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Referencias adicionales

* [Administración de certificados SSL en AD FS y WAP en Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
