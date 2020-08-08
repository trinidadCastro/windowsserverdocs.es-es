---
title: Configurar las extensiones AIA y CDP en CA1
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.author: lizross
author: eross-msft
ms.date: 07/26/2018
ms.openlocfilehash: 86c6bb664fc011be4f08e792118f3ff232f8410f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969642"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar las extensiones AIA y CDP en CA1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el punto de distribución de la lista de revocación de certificados (CDP) y la configuración de acceso a la información de entidad (AIA) en CA1.

Para llevar a cabo este procedimiento, debe ser miembro del grupo Admins. del dominio.

#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar las extensiones de CDP y AIA en CA1

1.  En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Entidad de certificación**.

2.  En el árbol de la consola entidad de certificación, haga clic con el botón secundario en **Corp-CA1-CA**y, a continuación, haga clic en **propiedades**.

    > [!NOTE]
    > El nombre de la CA es diferente si no escribió el nombre del equipo CA1 y el nombre de dominio es diferente al de este ejemplo. El nombre de la entidad de certificación está en el formato *dominio* - *nombredeequipoca*-CA.

3.  Haga clic en la pestaña **extensiones** . Asegúrese de que **seleccionar extensión** está establecido en **punto de distribución de CRL (CDP)** y, en **especificar ubicaciones desde las que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga lo siguiente:

    1.  Seleccione la entrada `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

    2.  Seleccione la entrada `http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

    3.  Seleccione la entrada que empieza por la ruta de acceso `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

4.  En **especificar ubicaciones desde las que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar ubicación** .

5.  En **Agregar ubicación**, en **Ubicación**, escriba `http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo Propiedades de la entidad de certificación.

6.  En la pestaña **extensiones** , active las casillas siguientes:

    -   **Incluir en CRL. los clientes lo usan para buscar las ubicaciones de diferencias CRL.**

    -   **Incluir en la extensión CDP de los certificados emitidos**

7.  En **especificar ubicaciones desde las que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar ubicación** .

8.  En **Agregar ubicación**, en **Ubicación**, escriba `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl` y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo Propiedades de la entidad de certificación.

9. En la pestaña **extensiones** , active las casillas siguientes:

    -   **Publicar las listas de revocación de certificados (CRL) en esta ubicación**

    -   **Publicar diferencias CRL en esta ubicación**

10. Cambie **seleccionar extensión** a **acceso a la información de entidad (AIA)** y, en las **ubicaciones especificadas desde las que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haga lo siguiente:

    1.  Seleccione la entrada que empieza por la ruta de acceso `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

    2.  Seleccione la entrada `http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

    3.  Seleccione la entrada `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt` y, a continuación, haga clic en **quitar**. En **Confirmar eliminación**, haga clic en **sí**.

11. En **especificar ubicaciones desde las que los usuarios pueden obtener el certificado para esta CA**, haga clic en **Agregar**. Se abre el cuadro de diálogo **Agregar ubicación** .

12. En **Agregar ubicación**, en **Ubicación**, escriba `http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt` y, a continuación, haga clic en **Aceptar**. Esto le devuelve al cuadro de diálogo Propiedades de la entidad de certificación.

13. En la pestaña **extensiones** , seleccione **incluir en el AIA de los certificados emitidos**.

14. Cuando se le pida que reinicie Active Directory servicios de Certificate Server, haga clic en **no**. El servicio se reiniciará más adelante.


