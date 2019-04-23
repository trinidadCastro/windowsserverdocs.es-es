---
title: Configurar las extensiones AIA y CDP en CA1
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 2bdd03636d1cbbcf5b0355358cbedeb63f97af1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834226"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar las extensiones AIA y CDP en CA1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el punto de distribución de lista de revocación de certificados (CRL) (CDP) y la configuración de acceso de la información de entidad emisora (AIA) en CA1.  
  
Para llevar a cabo este procedimiento, debe ser un miembro de Admins. del dominio.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar las extensiones de CDP y AIA en CA1  
  
1.  En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Entidad de certificación**.  
  
2.  En el árbol de consola entidad de certificación, haga clic en **CA de corp-CA1**y, a continuación, haga clic en **propiedades**.  
  
    > [!NOTE]  
    > El nombre de la entidad de certificación es diferente si no dio el nombre del equipo CA1 y el nombre de dominio es diferente del que en este ejemplo. El nombre de entidad emisora de certificados está en el formato *dominio*-*NombreDeEquipoCA*- CA.  
  
3.  Haga clic en la pestaña **Extensiones**. Asegúrese de que **Seleccionar extensión** está establecido en **punto de distribución de CRL (CDP)** y en el **especificar ubicaciones desde la que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, Haga lo siguiente:  
  
    1.  Seleccione la entrada `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
    2.  Seleccione la entrada `https://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
    3.  Seleccione la entrada que empieza con la ruta de acceso `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
4.  En **especificar ubicaciones desde la que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga clic en **agregar**. El **Agregar ubicación** abre el cuadro de diálogo.  
  
5.  En **Agregar ubicación**, en **ubicación**, tipo `https://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
6.  En el **extensiones** ficha, active las casillas siguientes:  
  
    -   **Incluir en las CRL. Los clientes usan para encontrar las ubicaciones de CRL Delta**  
  
    -   **Incluir en la extensión CDP de los certificados emitidos**  
  
7.  En **especificar ubicaciones desde la que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga clic en **agregar**. El **Agregar ubicación** abre el cuadro de diálogo.  
  
8.  En **Agregar ubicación**, en **ubicación**, tipo `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
9. En el **extensiones** ficha, active las casillas siguientes:  
  
    -   **Publicar CRL en esta ubicación**  
  
    -   **Publicar diferencias CRL en esta ubicación**  
  
10. Cambio **Seleccionar extensión** a **acceso de la información de entidad emisora (AIA)** y en el **especificar ubicaciones desde la que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, hacer los elementos siguientes:  
  
    1.  Seleccione la entrada que empieza con la ruta de acceso `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
    2.  Seleccione la entrada `https://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
    3.  Seleccione la entrada `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **Sí**.  
  
11. En **especificar ubicaciones desde la que los usuarios pueden obtener el certificado para esta entidad de certificación**, haga clic en **agregar**. El **Agregar ubicación** abre el cuadro de diálogo.  
  
12. En **Agregar ubicación**, en **ubicación**, tipo `https://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
13. En el **extensiones** ficha, seleccione **incluir en el AIA de los certificados emitidos**.  
  
14. Cuando se le pida que reinicie los servicios de certificados de Active Directory, haga clic en **No**. Se reiniciará el servicio más adelante.  
  

