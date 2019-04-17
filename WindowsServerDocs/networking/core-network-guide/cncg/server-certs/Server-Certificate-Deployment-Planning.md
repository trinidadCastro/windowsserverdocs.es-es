---
title: Planear la implementación de certificados de servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>Planear la implementación de certificados de servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Antes de implementar los certificados de servidor, debes planear los siguientes elementos:  
  
-   [Planear la configuración básica del servidor](#bkmk_basic)  
  
-   [Planear el acceso de dominio](#bkmk_domain)  
  
-   [Planear la ubicación y el nombre del directorio virtual en el servidor Web](#bkmk_virtual)  
  
-   [Planear un registro de alias (CNAME) de DNS para el servidor Web](#bkmk_cname)  
  
-   [Configuración del plan de CAPolicy.inf](#bkmk_capolicy)  
  
-   [Configuración del plan de las extensiones de CDP y AIA en CA1](#bkmk_cdp)  
  
-   [Planear la operación de copia entre la entidad de certificación y el servidor Web](#bkmk_copy)  
  
-   [Planear la configuración de la plantilla de certificado de servidor en la entidad emisora](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Planear la configuración básica del servidor  
Después de instalar Windows Server 2016 en los equipos que vas a usar como la entidad de certificación y el servidor Web, debes cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.  
  
Para obtener más información, consulta el Windows Server 2016 [guía básica de red](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Planear el acceso de dominio  
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y la cuenta de usuario debe crearse en AD DS antes del intento de inicio de sesión. Además, la mayoría de los procedimientos en esta guía requiere que la cuenta de usuario es miembro de los grupos Administradores de empresa o administradores de dominio de usuarios de Active Directory y equipos, por lo que debe iniciar sesión en la CA con una cuenta que tenga la pertenencia al grupo apropiado.  
  
Para obtener más información, consulta el Windows Server 2016 [guía básica de red](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Planear la ubicación y el nombre del directorio virtual en el servidor Web  
Para proporcionar acceso a la CRL y el certificado de CA en otros equipos, debes almacenar estos elementos en un directorio virtual en el servidor Web. En esta guía, el directorio virtual se encuentra en el servidor Web WEB1. Esta carpeta está en la unidad "C:" y se denomina "pki". Puedes encontrar el directorio virtual en tu servidor Web en cualquier ubicación de carpeta que sea apropiado para la implementación.  
  
## <a name="bkmk_cname"></a>Planear un registro de alias (CNAME) de DNS para el servidor Web  
Registros de recursos de alias (CNAME) también se denominan registros de recursos de nombre canónico. Con estos registros, puedes usar más de un nombre para apuntar a un único host, lo que facilita a hacer cosas tales como host de un servidor de protocolo de transferencia de archivos (FTP) y un servidor Web en el mismo equipo. Por ejemplo, los nombres de servidor conocidos (ftp, www) se registran mediante los registros de recursos de alias (CNAME) que se asignan en el nombre de host de sistema de nombres de dominio (DNS), como WEB1, para el equipo servidor que hospeda estos servicios.  
  
Esta guía brinda instrucciones para configurar tu servidor Web para hospedar la lista de revocación de certificados (CRL) para la entidad de certificación (CA). Dado que es posible que también quieres usar tu servidor Web para otros fines, como para hospedar un sitio Web o FTP, es una buena idea para crear un registro de alias de recursos en DNS para tu servidor Web. En esta guía, el registro CNAME se denomina "pki", pero puedes elegir un nombre que sea apropiado para la implementación.  
  
## <a name="bkmk_capolicy"></a>Configuración del plan de CAPolicy.inf  
Antes de instalar AD CS, debes configurar CAPolicy.inf en la entidad de certificación con la información correcta para la implementación. Un archivo CAPolicy.inf contiene la siguiente información:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
Debes planear los siguientes elementos para este archivo:  
  
-   **Dirección URL**. El archivo CAPolicy.inf de ejemplo tiene un valor de la dirección URL de **http://pki.corp.contoso.com/pki/cps.txt**. Esto es porque el servidor Web en esta guía se denomina WEB1 y tiene un registro de recursos de DNS CNAME de pki. El servidor Web también está unido al dominio corp.contoso.com. Además, hay un directorio virtual en el servidor Web denominado "pki" se almacena en la lista de revocación de certificados. Asegúrate de que el valor que proporcionas para la dirección URL en el archivo de CAPolicy.inf apunta a un directorio virtual en el servidor Web de tu dominio.  
  
-   **RenewalKeyLength**. La longitud de clave de renovación de forma predeterminada para los AD CS en Windows Server 2012 es 2048. La longitud de clave que selecciones debe ser siempre posible sin dejar de ofrecer compatibilidad con las aplicaciones que vayas a usar.  
  
-   **RenewalValidityPeriodUnits**. El archivo CAPolicy.inf de ejemplo tiene un valor de RenewalValidityPeriodUnits de 5 años. Esto es porque la vida útil prevista de la CA es alrededor de diez años. El valor de RenewalValidityPeriodUnits debería reflejar el período de validez general de la entidad de certificación o el mayor número de años para el que quieres proporcionar inscripción.  
  
-   **CRLPeriodUnits**. El archivo de CAPolicy.inf de ejemplo tiene un valor CRLPeriodUnits de 1. Esto es porque el intervalo de actualización de ejemplo para la lista de revocación de certificados en esta guía es de 1 semana. El valor de intervalo que puedes especificar con esta configuración, debes publicar la CRL en la entidad de certificación en el directorio virtual del servidor Web donde se almacena la CRL y proporcionar acceso a él para equipos que están en el proceso de autenticación.  
  
-   **AlternateSignatureAlgorithm**. Este CAPolicy.inf implementa un mecanismo de mejorar la seguridad mediante la implementación de formatos de firma alternativos. No debería implementar esta configuración si todavía tienes los clientes de Windows XP que requieran certificados de esta CA.  
  
Si no tienes previsto sobre cómo agregar cualquier subordinadas a la infraestructura de clave pública en un momento posterior, y si quieres evitar la adición de CA subordinadas, puedes agregar la clave de PathLength al archivo CAPolicy.inf con un valor de 0. Para agregar esta clave, copia y pega el siguiente código en el archivo:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> No se recomienda cambiar otras opciones de configuración en el archivo CAPolicy.inf a menos que tengas un motivo específico para hacerlo.  
  
## <a name="bkmk_cdp"></a>Configuración del plan de las extensiones de CDP y AIA en CA1  
Al configurar el punto de distribución de la lista de revocación de certificados (CRL) (CDP) y la configuración de acceso de la información de entidad (AIA) en CA1, necesitas el nombre de tu servidor Web y el nombre de dominio. También necesitas el nombre del directorio virtual que crees en el servidor Web donde se almacenan la lista de revocación de certificados (CRL) y el certificado de entidad de certificación.  
  
La ubicación de CDP que tienes que escribir durante este paso de implementación tiene el formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Por ejemplo, si tu servidor Web se denomina WEB1 y su DNS alias un registro CNAME para el servidor Web es "pki", tu dominio es corp.contoso.com y el directorio virtual se denomina la infraestructura de clave pública, la ubicación de CDP es:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
La ubicación de AIA que tienes que escribir tiene el formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Por ejemplo, si tu servidor Web se denomina WEB1 y su DNS alias un registro CNAME para el servidor Web es "pki", tu dominio es corp.contoso.com y el directorio virtual se denomina la infraestructura de clave pública, la ubicación AIA es:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Planear la operación de copia entre la entidad de certificación y el servidor Web  
Para publicar el certificado CRL y entidad de certificación de la entidad de certificación en el directorio virtual del servidor Web, puedes ejecutar el comando - crl certutil después de configurar las ubicaciones de CDP y AIA en la CA. Asegúrate de que configures las rutas de acceso correctas en las propiedades de la CA **extensiones** pestaña antes de ejecutar este comando siguiendo las instrucciones de esta guía. Además, para copiar el certificado de CA de empresa en el servidor Web, debe haber ya creado el directorio virtual en el servidor Web y configurado la carpeta como una carpeta compartida.  
  
## <a name="bkmk_template"></a>Planear la configuración de la plantilla de certificado de servidor en la entidad emisora  
Para implementar certificados de servidor de inscripción automática, debes copiar la plantilla de certificado denominada **RAS y servidor IAS**. De manera predeterminada, esta copia se denomina **RAS de copiar y servidor IAS**. Si quieres cambiar el nombre de esta copia de la plantilla, planear el nombre que quieres usar durante este paso de implementación.  
  
> [!NOTE]  
> Las secciones de implementación últimos tres en esta guía, que permiten configurar el servidor de inscripción, actualizar la directiva de grupo en los servidores y comprueba que los servidores han recibido un certificado de servidor válido de la CA - no requieren pasos adicionales de planeación.  
  


