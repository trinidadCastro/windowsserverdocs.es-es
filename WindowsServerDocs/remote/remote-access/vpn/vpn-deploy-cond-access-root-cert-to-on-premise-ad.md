---
title: Implementar certificados raíz de acceso condicional en AD local
description: ''
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 67d361db7a2dd3f2879e8beb924075dae68d52a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404318"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Paso 7.4. Implementar certificados raíz de acceso condicional en AD local

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

En este paso, implementará el certificado raíz de acceso condicional como certificado raíz de confianza para la autenticación de VPN en su instancia de AD local.

- [**Previo** Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)
- [**Nueva** Paso 7.5. Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. En la página **Conectividad VPN** , seleccione **Descargar certificado**.

   >[!NOTE]
   >La opción **Descargar certificado Base64** está disponible para algunas configuraciones que requieren certificados base64 para la implementación.

2. Inicie sesión en un equipo unido a un dominio con derechos de administrador de empresa y ejecute estos comandos desde un símbolo del sistema de administrador para agregar los certificados raíz de la nube en el almacén *Enterprise NTauth* :

   >[!NOTE]
   >En los entornos en los que el servidor VPN no está unido al dominio de Active Directory, los certificados raíz de la nube deben agregarse manualmente al almacén de _entidades de certificación raíz de confianza_ .

   | Comando | Descripción |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Crea dos contenedores de la entidad de certificación **raíz de Microsoft VPN de generación 1** en los contenedores **CN = AIA** y **CN = Certification Authority** y publica cada certificado raíz como un valor en el atributo _el certificado_ de la **raíz de Microsoft VPN.** Contenedores de gen. 1 de CA. |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crea un contenedor **CN = NTAuthCertificates** bajo los contenedores de entidades de certificación **CN = AIA** y **CN =** , y publica cada certificado raíz como un valor en el atributo _el certificado_ de **CN = Contenedor NTAuthCertificates** . |
   | `gpupdate /force` | Acelera la adición de los certificados raíz a los equipos cliente y servidor de Windows. |

3. Compruebe que los certificados raíz están presentes en el almacén Enterprise NTauth y que se muestran como de confianza:
   1. Inicie sesión en un servidor con derechos de administrador de organización que tenga instaladas las herramientas de administración de la **entidad de certificación** .

   >[!NOTE]
   >De forma predeterminada, las **herramientas de administración** de la entidad de certificación están instaladas en servidores de la entidad de certificación. Se pueden instalar en otros servidores miembros como parte de las **herramientas de administración de roles** en Administrador del servidor.

   1. En el servidor VPN, en el menú Inicio, escriba **pkiview. msc** para abrir el cuadro de diálogo Enterprise PKI.
   1. En el menú Inicio, escriba **pkiview. msc** para abrir el cuadro de diálogo Enterprise PKI.
   1. Haga clic con el botón secundario en **Enterprise PKI** y seleccione **administrar contenedores de ad**.
   1. Compruebe que cada certificado de entidad de certificación raíz de Microsoft VPN de generación 1 está presente en:
      - NTAuthCertificates
      - Contenedor de AIA
      - Contenedor de entidades de certificación

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.5. Creación de perfiles de VPNv2 basados en OMA-DM en dispositivos Windows 10 @ no__t-0: En este paso, puede crear perfiles de VPNv2 basados en OMA-DM mediante Intune para implementar una directiva de configuración de dispositivos VPN. Si quiere usar SCCM o el script de PowerShell para crear perfiles de VPNv2, consulte [configuración de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles.
