---
title: Implementar certificados raíz de acceso condicional en AD local
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 200d3b96ee24b5e1264b4bf2e42d636f9e07fbef
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469680"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Paso 7.4. Implementar certificados de raíz de acceso condicional en local AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

En este paso, implementará el certificado raíz de acceso condicional como certificado raíz de confianza para la autenticación de VPN a sus instalaciones en AD.

- [**Anterior:** Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)
- [**Siguiente:** Paso 7.5. Crear perfiles de VPNv2 basados en OMA-DM para dispositivos con Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. En el **conectividad VPN** página, seleccione **Descargar certificado**.

   >[!NOTE]
   >El **descargar base64 certificado** opción está disponible para algunas configuraciones que requieren certificados de base64 para la implementación.

2. Inicie sesión en un equipo unido al dominio con derechos de administrador de empresa y ejecute estos comandos desde un símbolo del sistema de administrador para agregar a la nube raíz o los certificados en el *Enterprise NTauth* almacenar:

   >[!NOTE]
   >Para entornos donde el servidor VPN no está unido al dominio de Active Directory, los certificados de raíz en la nube deben agregarse a la _entidades emisoras raíz de confianza_ almacenar de forma manual.

   | Comando | Descripción |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Crea dos **gen. 1 de entidad de certificación raíz de VPN de Microsoft** contenedores en el **CN = AIA** y **CN = entidades de certificación** contenedores y se publica cada certificado raíz como un valor en el _el certificado de CA_ atributo de ambos **gen. 1 de entidad de certificación raíz de VPN de Microsoft** contenedores. |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crea una **CN = NTAuthCertificates** contenedor bajo el **CN = AIA** y **CN = entidades de certificación** contenedores y se publica cada certificado raíz como un valor en el _el certificado de CA_ atributo de la **CN = NTAuthCertificates** contenedor. |
   | `gpupdate /force` | Acelera la adición de los certificados raíz para los equipos cliente y servidor de Windows. |

3. Compruebe que los certificados raíz están presentes en el almacén NTauth de empresa y mostrar como de confianza:
   1. Inicie sesión en un servidor con derechos de administrador de empresa que tiene el **las herramientas de administración de autoridad de certificado** instalado.

   >[!NOTE]
   >De forma predeterminada el **las herramientas de administración de autoridad de certificado** son servidores de entidad emisora de certificados instalados. Puede instalarse en otros servidores miembros como parte de la **herramientas de administración de roles** en Administrador del servidor.

   1. En el servidor VPN, en el menú Inicio, escriba **pkiview.msc** para abrir el cuadro de diálogo de PKI de empresa.
   1. En el menú Inicio, escriba **pkiview.msc** para abrir el cuadro de diálogo de PKI de empresa.
   1. Haga clic en **Enterprise PKI** y seleccione **administrar AD contenedores**.
   1. Compruebe que está presente en cada certificado de gen. 1 entidad de certificación raíz de VPN de Microsoft:
      - NTAuthCertificates
      - Contenedor AIA
      - Contenedor de entidades emisoras de certificados

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.5. Creación de OMA-DM basado VPNv2 perfiles para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): En este paso, puede crear configuración de OMA-DM en función de los perfiles de VPNv2 mediante Intune para implementar una directiva de configuración del dispositivo VPN. Si desea SCCM o la secuencia de comandos de PowerShell para crear perfiles de VPNv2, consulte [configuración de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obtener más detalles.
