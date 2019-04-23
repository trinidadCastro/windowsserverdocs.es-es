---
title: Planeación de la implementación del certificado de servidor
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0d14ed33c9bf389433f59774c04ff15e37256cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839536"
---
# <a name="server-certificate-deployment-planning"></a>Planeación de la implementación del certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de implementar certificados de servidor, debe planear los siguientes elementos:  
  
-   [Planear la configuración básica del servidor](#bkmk_basic)  
  
-   [Planeación de acceso de dominio](#bkmk_domain)  
  
-   [Planear la ubicación y el nombre del directorio virtual en el servidor Web](#bkmk_virtual)  
  
-   [Planear un registro de alias (CNAME) de DNS para el servidor Web](#bkmk_cname)  
  
-   [Planear la configuración de CAPolicy.inf](#bkmk_capolicy)  
  
-   [Planear la configuración de las extensiones de CDP y AIA en CA1](#bkmk_cdp)  
  
-   [Planear la operación de copia entre la entidad de certificación y el servidor Web](#bkmk_copy)  
  
-   [Planificar la configuración de la plantilla de certificado de servidor en la entidad de certificación](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planear la configuración básica del servidor  
Después de instalar Windows Server 2016 en los equipos que va a utilizar como la entidad de certificación y el servidor Web, debe cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.  
  
Para obtener más información, vea Windows Server 2016 [Guía de red principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planeación de acceso de dominio  
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y se debe crear la cuenta de usuario en AD DS antes del intento de inicio de sesión. Además, la mayoría de los procedimientos de esta guía requiere que la cuenta de usuario es miembro de los grupos Administradores de organización o Admins. del dominio de usuarios de Active Directory y los equipos, por lo que debe iniciar sesión en la entidad de certificación con una cuenta que tenga la pertenencia al grupo adecuado.  
  
Para obtener más información, vea Windows Server 2016 [Guía de red principal](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planear la ubicación y el nombre del directorio virtual en el servidor Web  
Para proporcionar acceso a la CRL y el certificado de CA en otros equipos, debe almacenar estos elementos en un directorio virtual en el servidor Web. En esta guía, el directorio virtual se encuentra en el servidor Web WEB1. Esta carpeta se encuentra en la unidad "C:" y se denomina "pki". Puede encontrar el directorio virtual en el servidor Web en cualquier ubicación de carpeta que sea adecuado para su implementación.  
  
## <a name="bkmk_cname"></a>Planear un registro de alias (CNAME) de DNS para el servidor Web  
Registros de recursos de alias (CNAME) a veces también se denominan registros de recursos de nombre canónico. Con estos registros, puede usar más de un nombre para que apunte a un solo host, lo que facilita realizar operaciones tales como host de un servidor de protocolo de transferencia de archivos (FTP) y un servidor Web en el mismo equipo. Por ejemplo, los nombres de servidor conocidos (ftp, www) se registran con registros de recursos de alias (CNAME) que se asignan para el nombre de host del sistema de nombres de dominio (DNS), por ejemplo, WEB1, para el equipo del servidor que hospeda estos servicios.  
  
Esta guía proporciona instrucciones para configurar el servidor Web para hospedar la lista de revocación de certificados (CRL) para la entidad de certificación (CA). Dado que también puede usar el servidor Web para otros propósitos, como hospedar un sitio FTP o Web, es una buena idea para crear un registro de recursos de alias de DNS para el servidor Web. En esta guía, el registro CNAME se denomina "pki", pero puede elegir un nombre que sea adecuado para su implementación.  
  
## <a name="bkmk_capolicy"></a>Planear la configuración de CAPolicy.inf  
Antes de instalar AD CS, debe configurar el archivo CAPolicy.inf en la CA con la información correcta para su implementación. Un archivo CAPolicy.inf contiene la información siguiente:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=https://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Debe planear los siguientes elementos para este archivo:  
  
-   **URL**. El archivo CAPolicy.inf de ejemplo tiene un valor de dirección URL de **https://pki.corp.contoso.com/pki/cps.txt**. Esto es porque el servidor Web en esta guía se denomina WEB1 y tiene un registro de recursos CNAME de DNS de la pki. El servidor Web también está unido al dominio corp.contoso.com. Además, hay un directorio virtual en el servidor Web denominado "pki" donde se almacena la lista de revocación de certificados. Asegúrese de que el valor que proporcione para dirección URL en el archivo de CAPolicy.inf apunta a un directorio virtual en el servidor Web en el dominio.  
  
-   **RenewalKeyLength**. La longitud de clave de renovación predeterminado para AD CS en Windows Server 2012 es 2048. La longitud de clave que seleccione debe ser siempre que sea posible mientras sigue proporcionando compatibilidad con las aplicaciones que se va a utilizar.  
  
-   **RenewalValidityPeriodUnits**. El archivo CAPolicy.inf de ejemplo tiene un valor de RenewalValidityPeriodUnits de 5 años. Esto es porque la duración prevista de la entidad de certificación es aproximadamente diez años. El valor de RenewalValidityPeriodUnits debe reflejar el período de validez general de la entidad de certificación o la mayor cantidad de años para la que desea permitir la inscripción.  
  
-   **CRLPeriodUnits**. El archivo CAPolicy.inf de ejemplo tiene un valor de CRLPeriodUnits de 1. Esto es porque el intervalo de actualización de ejemplo para la lista de revocación de certificados en esta guía es 1 semana. En el valor de intervalo que se especifique con esta configuración, debe publicar la CRL en la entidad de certificación en el directorio virtual del servidor Web donde se almacena la CRL y proporcionar acceso a él para equipos que están en el proceso de autenticación.  
  
-   **AlternateSignatureAlgorithm**. Este archivo CAPolicy.inf se implementa un mecanismo de seguridad mejorada mediante la implementación de formatos de firma alternativos. No debe implementar esta configuración si sigue teniendo los clientes de Windows XP que requieren certificados de esta entidad de certificación.  
  
Si no tiene previsto agregar cualquier CA subordinadas a la infraestructura de clave pública en un momento posterior, y si desea impedir la adición de las CA subordinadas, puede agregar la clave PathLength en el archivo CAPolicy.inf con un valor de 0. Para agregar esta clave, copie y pegue el código siguiente en el archivo:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> No se recomienda que cambie las opciones en el archivo CAPolicy.inf a menos que tenga una razón concreta para hacerlo.  
  
## <a name="bkmk_cdp"></a>Planear la configuración de las extensiones de CDP y AIA en CA1  
Al configurar el punto de distribución de lista de revocación de certificados (CRL) (CDP) y la configuración de acceso de la información de entidad emisora (AIA) en CA1, necesita el nombre de su servidor Web y el nombre de dominio. También necesitará el nombre del directorio virtual que cree en el servidor Web donde se almacenan la lista de revocación de certificados (CRL) y el certificado de entidad de certificación.  
  
La ubicación de CDP que debe especificar durante este paso de implementación tiene el formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Por ejemplo, si el servidor Web se denomina WEB1 y su registro de alias CNAME para el servidor Web DNS es "pki", el dominio es corp.contoso.com y el directorio virtual se denomina pki, la ubicación de CDP es:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
La ubicación de AIA que debe escribir tiene el formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Por ejemplo, si el servidor Web se denomina WEB1 y su registro de alias CNAME para el servidor Web DNS es "pki", el dominio es corp.contoso.com y el directorio virtual se denomina pki, la ubicación de AIA es:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planear la operación de copia entre la entidad de certificación y el servidor Web  
Para publicar el certificado CRL y entidad de certificación desde la CA en el directorio virtual del servidor Web, puede ejecutar el comando de certutil - crl después de configurar las ubicaciones de CDP y AIA en la CA. Asegúrese de configurar las rutas de acceso correctas en las propiedades de entidad de certificación **extensiones** pestaña antes de ejecutar este comando siguiendo las instrucciones de esta guía. Además, para copiar el certificado de CA de empresa en el servidor Web, debe haber ya creado el directorio virtual en el servidor Web y configura la carpeta como una carpeta compartida.  
  
## <a name="bkmk_template"></a>Planificar la configuración de la plantilla de certificado de servidor en la entidad de certificación  
Para implementar certificados de servidor de inscripción automática, debe copiar la plantilla de certificado denominada **servidor RAS e IAS**. De forma predeterminada, esta copia se denomina **copia de RAS e IAS Server**. Si desea cambiar el nombre de esta copia de la plantilla, el nombre que desea usar durante este paso de implementación del plan.  
  
> [!NOTE]  
> Las secciones de tres últimas implementación en esta guía, que le permiten configurar la inscripción automática de certificado de servidor, actualizar la directiva de grupo en servidores y comprobación que los servidores han recibido un certificado de servidor válido de la entidad de certificación - no requieren una planeación adicional pasos.  
  


