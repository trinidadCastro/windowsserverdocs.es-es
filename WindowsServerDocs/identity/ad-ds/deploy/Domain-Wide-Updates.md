---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Actualizaciones de esquema de todo el dominio de Active Directory
description: Las actualizaciones de esquema de todo el dominio que se realizan adprep /domainprep al promover un controlador de dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>Actualizaciones de esquema de todo el dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes revisar el siguiente conjunto de cambios para ayudar a comprender y preparar para las actualizaciones de esquema que se realizan por adprep /domainprep en Windows Server. 

A partir de Windows Server 2012, se ejecutan automáticamente según sea necesario durante la instalación de AD DS Adprep comandos. Se pueden también ejecutar por separado antes de la instalación de AD DS. Para obtener más información, consulta [Adprep.exe ejecutando](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Para obtener más información sobre cómo interpretar las cadenas de entrada (ACE) del control de acceso, consulta [cadenas ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Para obtener más información sobre cómo interpretar las cadenas de identificador (SID) de seguridad, consulta [cadenas SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (Channel anual comas): Actualiza todo el dominio

Después de las operaciones realizadas por **domainprep** de 2016 de servidor de Windows (operación 89) completa, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = sistema, DC = ForestRootDomain objeto se establece en **16**.

|Número de operaciones y el GUID|Descripción|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Eliminar la ACE otorgar el Control total a los administradores de empresa clave y agregar una ACE de concesión de empresa clave administradores Control total sobre solo el atributo msdsKeyCredentialLink.|Eliminar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave) <br /> <br />Agregar (OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de clave)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: Actualizaciones de todo el dominio

Después de las operaciones realizadas por **domainprep** de 2016 de servidor de Windows (operaciones 82 88) completa, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = sistema, DC = ForestRootDomain objeto se establece en **15**.

|Número de operaciones y el GUID|Descripción|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Crear CN = contenedor de claves en la raíz del dominio|-objectClass: contenedor<br />-Descripción: contenedor predeterminado para los objetos de claves de credenciales<br />-ShowInAdvancedViewOnly: TRUE|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; EA)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; D. A)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; D. D.)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; ED)|
|**Operación 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Agregar el Control total permitir ACE a CN = contenedor de claves de "domain\Key administradores" y "rootdomain\Enterprise administradores clave".|N/D|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave)|
|**Operación de 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modificar el atributo otherWellKnownObjects para apuntar a CN = contenedor de claves.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN = teclas, %ws|N/D|
|**Operación 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modificar el dominio CN para permitir que las "domain\Key administradores" y "rootdomain\Enterprise administradores clave" para modificar el atributo msds KeyCredentialLink. |N/D|(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de clave)<br />(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Administradores de organización clave en el dominio raíz, pero en dominios no raíz aparecía una ACE relativa del dominio ficticia con un SID-527 no se puede resolver)|
|**Operación 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Conceder el coche DS validado-escritura-equipo Creador propietario y self|N/D|(OA; CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11D0-a285-00aa003049e2;PS)<br />(OA; CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11D0-a285-00aa003049e2;CO)|
|**Operación 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Eliminar la ACE otorgar el Control total para el grupo de administradores de organización clave incorrecto relativa al dominio y agregar una ACE conceder Control total al grupo Administradores de organización clave. |N/D|Eliminar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave)<br /> <br />Agregar (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Administradores de clave)|
|**Operación 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Agregar "msDS ExpirePasswordsOnSmartCardOnlyAccounts" en el objeto de dominio CN y establecer el valor predeterminado "false"|N/D|N/D|

Los grupos Administradores de la clave de empresa y administradores de clave solo se crean después de un controlador de dominio de Windows Server 2016 se promueve y se encarga de la función FSMO de emulador PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: Actualiza todo el dominio

Aunque no operaciones se realizan en **domainprep** en Windows Server 2012 R2, cuando finalice el comando, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = sistema, DC = ForestRootDomain objeto se establece en **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: Actualizaciones de todo el dominio

Después de las operaciones realizadas por **domainprep** en 2012 de servidor de Windows (operaciones 78, 79, 80 y 81) completa, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = sistema, DC = ForestRootDomain objeto se establece en **9**.

|Número de operaciones y el GUID|Descripción|Atributos|Permisos|
|------------------------------|---------------|--------------|---------------|
|**Operación 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Crea un nuevo objeto CN = TPM dispositivos en la partición de dominio.|Clase de objeto: OwnerInformation InformationObjectsContainer|N/D|
|**Operación 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Crea una entrada de control de acceso para el servicio TPM.|N/D|(OA; CIIO; WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11D0-a285-00aa003049e2;PS)|
|**Operación 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Conceder "Clone DC" derecho extendido para **clonación controladores de dominio** grupo|N/D|(OA; CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e; *SID del dominio*-522)|
|**Operación 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Concede ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity a automático Principal en todos los objetos.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|
