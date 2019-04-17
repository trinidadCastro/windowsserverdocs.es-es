---
title: Configurar las extensiones AIA y CDP en CA1
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80f6d68816a58b2ac30010e0917ce0f816a30234
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar las extensiones AIA y CDP en CA1

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para configurar el punto de distribución de la lista de revocación de certificados (CRL) (CDP) y la configuración de acceso de la información de entidad (AIA) en CA1.  
  
Para realizar este procedimiento, debe ser miembro del grupo Administradores de dominio de.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar las extensiones CDP y AIA en CA1  
  
1.  En el administrador del servidor, haz clic en **herramientas** y, a continuación, haz clic en **entidad de certificación**.  
  
2.  En el árbol de consola de la entidad de certificación, haz clic en **corp-CA1-CA**y, a continuación, haz clic en **propiedades**.  
  
    > [!NOTE]  
    > El nombre de la CA es diferente si el equipo CA1 no dio el nombre y el nombre de dominio es diferente a la que se muestra en este ejemplo. Es el nombre de entidad emisora de certificados en el formato *dominio*-*nombreEquipoCA*-CA.  
  
3.  Haz clic en el **extensiones** ficha Asegúrate de que **Seleccionar extensión** se establece en **punto de distribución CRL (CDP)**y en la **señalan las ubicaciones desde el que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haz lo siguiente:  
  
    1.  Selecciona la entrada `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
    2.  Selecciona la entrada `http:\/\/<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
    3.  Selecciona la entrada que comienza con la ruta de acceso `ldap:\/\/CN\=<CATruncatedName><CRLNameSuffix>,CN\=<ServerShortName>`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
4.  En **señalan las ubicaciones desde el que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haz clic en **agregar**. La **Agregar ubicación** abre el cuadro de diálogo.  
  
5.  En **Agregar ubicación**, en **ubicación**, tipo `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haz clic en **Aceptar**. Esto devuelve en el cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
6.  En la **extensiones** selecciona las casillas de verificación siguientes:  
  
    -   **Incluir en las CRL. Usada para encontrar las ubicaciones de diferencia CRL**  
  
    -   **Incluir en la extensión CDP de certificados emitidos**  
  
7.  En **señalan las ubicaciones desde el que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haz clic en **agregar**. La **Agregar ubicación** abre el cuadro de diálogo.  
  
8.  En **Agregar ubicación**, en **ubicación**, tipo `file:\/\/\\\\pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`y, a continuación, haz clic en **Aceptar**. Esto devuelve en el cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
9. En la **extensiones** selecciona las casillas de verificación siguientes:  
  
    -   **Publicar CRL en esta ubicación**  
  
    -   **Publicar las CRL de diferencias en esta ubicación**  
  
10. Cambiar **selecciona la extensión** a **acceso de la información de entidad (AIA)**y en la **señalan las ubicaciones desde el que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haz lo siguiente:  
  
    1.  Selecciona la entrada que comienza con la ruta de acceso `ldap:\/\/CN\=<CATruncatedName>,CN\=AIA,CN\=Public Key Services`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
    2.  Selecciona la entrada `http:\/\/<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
    3.  Selecciona la entrada `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`y, a continuación, haz clic en **quitar**. En **Confirmar eliminación**, haz clic en **Sí**.  
  
11. En **señalan las ubicaciones desde el que los usuarios pueden obtener una lista de revocación de certificados (CRL)**, haz clic en **agregar**. La **Agregar ubicación** abre el cuadro de diálogo.  
  
12. En **Agregar ubicación**, en **ubicación**, tipo `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`y, a continuación, haz clic en **Aceptar**. Esto devuelve en el cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
13. En la **extensiones**, selecciona **incluir en la AIA de los certificados emitidos**.  
  
14. En **Agregar ubicación**, en **ubicación**, tipo `file:\/\/\\\\pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`y, a continuación, haz clic en **Aceptar**. Esto devuelve en el cuadro de diálogo de propiedades de entidad emisora de certificados.  
  
    > [!IMPORTANT]  
    > Asegúrate de que **incluir en la extensión AIA de los certificados emitidos** no está seleccionado.  
  
15. Cuando aparezca el mensaje para reiniciar los servicios de certificados de Active Directory, haga clic en **No**. Se reiniciará el servicio más adelante.  
  


