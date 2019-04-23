---
ms.assetid: 3647b7e3-54a4-46c6-ab68-82fcf3bfacda
title: Actualizaciones para todo el bosque de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 90b7bb4c8012e7e1a29ea2c1b542d78e75e7708f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816836"
---
# <a name="forest-wide-updates"></a>Actualizaciones para todo el bosque

>Se aplica a: Windows Server

Puede revisar el siguiente conjunto de cambios para ayudar a entender y prepararse para las actualizaciones del esquema que realizan adprep /forestprep en Windows Server 2019.

A partir de Windows Server 2012, comandos Adprep se ejecutan automáticamente según sea necesario durante la instalación de AD DS. También se puede ejecutar por separado antes de la instalación de AD DS. Para obtener más información, consulta [Ejecutar Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Para obtener más información sobre cómo interpretar las cadenas de entrada (ACE) del control de acceso, consulte [cadenas ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Para obtener más información sobre cómo interpretar las cadenas de identificador (SID) de seguridad, consulte [cadenas SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-2016-forest-wide-updates"></a>Windows Server 2016: Actualizaciones para todo el bosque

Después de las operaciones realizadas por el **forestprep** comandos en Windows Server 2016 (operaciones 136 142) están completos, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = ForestUpdates, CN = Configuration, DC = dominioRaízDelBosque objeto se establece en **16**.

|Número de operación y el GUID|Descripción|Atributos|Permisos|
|-----------------------------|---------------|--------------|---------------|
|**Operación 136**: {328092FB-16E7-4453-9AB8-7592DB56E9C4}|Conceder el "CN = envío-como CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 137**: {3A1C887F-DF0A-489F-B3F2-2D0409095F6E}|Conceder el "CN = recepción-como CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 138**: {232E831F-F988-4444-8E3E-8A352E2FD411}|Conceder el "CN = información Personal, CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 139**: {DDDDCF0C-BEC9-4A5A-AE86-3CFE6CC6E110}|Conceder el "CN = información pública, CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 140**: {A0A45AAC-5550-42DF-BB6A-3CC5C46B52F2}|Conceder el "CN = validado-SPN, CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 141**: {3E7645F3-3EA5-4567-B35A-87630449C70C}|Conceder el "CN = Allowed-a-Authenticate, CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|
|**Operación 142**: {E634067B-E2C4-4D79-B6E8-73C619324D5E}|Conceder el "CN = MS-TS-GatewayAccess, CN = Extended Rights" para las cuentas gMSA.|N/D|N/D|

## <a name="windows-server-2012-r2-forest-wide-updates"></a>Windows Server 2012 R2: Actualizaciones para todo el bosque

Después de las operaciones realizadas por el **forestprep** comandos en Windows Server 2012 R2 (operaciones 131 135) están completos, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = ForestUpdates, CN = Configuration, DC = dominioRaízDelBosque objeto se establece en **15**.

|Número de operación y el GUID|Descripción|Atributos|Permisos|
|-----------------------------|---------------|--------------|---------------|
|**Operación 131**: {b83818c1-01a6-4f39-91b7-a3bb581c3ae3}|Crea un objeto de contenedor de configuración de directiva de autenticación nuevo CN = Configuración de directiva de autorización, CN = Services en la partición de configuración.|-objectClass: contenedor<br />-   displayName: Configuración de directiva de autenticación<br />: Descripción: Contiene configuración de directiva de autenticación.<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 132**: {bbbb9db0-4009-4368-8c40-6674e980d3c3}|Crea un nuevo objeto de directivas de autenticación CN = AuthN Policies, CN = Configuración de directiva de autorización, CN = Services en la partición de configuración.|-   objectClass: msDS-AuthNPolicies<br />-   displayName: Authentication Policies<br />: Descripción: Contiene los objetos de directiva de autenticación.<br />-   showInAdvancedViewOnly: True|(A;;RPWPCRCCDCLCLOLORCWOWDSDDTDTSW;;;EA)<br />(A;;RPWPCRCCDCLCLORCWOWDSDDTSW;;;SY)<br />(A; RPLCLORC;; AU)|
|**Operación 133**: {f754861c-3692-4a7b-b2c2-d0fa28ed0b0b}|Creada una silos de directivas de autenticación nuevo objeto CN = Silos de autenticación, CN = Configuración de directiva de autorización, CN = Services en la partición de configuración.|-   objectClass: msDS-AuthNPolicySilos<br />-   displayName: Authentication Policy Silos<br />: Descripción: Contiene objetos de silo de directivas de autenticación.<br />-   showInAdvancedViewOnly: True|(A;;RPWPCRCCDCLCLOLORCWOWDSDDTDTSW;;;EA)<br />(A;;RPWPCRCCDCLCLORCWOWDSDDTSW;;;SY)<br />(A; RPLCLORC;; AU)|
|**Operación 134**: {d32f499f-3026-4af0-a5bd-13fe5a331bd2}|Crea un objeto de tipo de notificación de silo de autenticación nuevo CN = ad: / / ext/AuthenticationSilo, CN = Claim Types, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ClaimType<br />-   displayname: AuthenticationSilo<br />-   name: ad://ext/AuthenticationSilo<br />-Habilitado: True<br />-   msDS-ClaimIsValueSpaceRestricted: True<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-ClaimSourceType: Construido<br />-   msDS-ClaimValueType: 3<br />-   msDS-ClaimTypeAppliesToClass: CN = usuario, CN = Schema, %ws<br />-   msDS-ClaimTypeAppliesToClass: CN = Computer, CN = Schema, %ws<br />-   msDS-ClaimTypeAppliesToClass: CN = ms-DS-de-cuenta de servicio administrada, CN = Schema, %ws<br />-   msDS-ClaimTypeAppliesToClass: CN=ms-DS-Group-Managed-Service-Account,CN=Schema,%WS|(A;;RPWPCRCCDCLCLORCWOWDSDDTSW;;;EA)<br />(A;;RPWPCRCCDCLCLORCWOWDSDDTSW;;;SY)<br />(A; RPLCLORC;; AU)|
|**Funcionamiento 135**: {38618886-98ee-4e42-8cf1-d9a2cd9edf8b}|Establecer el atributo msDS-ClaimIsValueSpaceRestricted en el nuevo tipo de notificación de silo de autenticación en false|-   msDS-ClaimIsValueSpaceRestricted: False|N/D|

## <a name="windows-server-2012-forest-wide-updates"></a>Windows Server 2012: Actualizaciones para todo el bosque

Después de las operaciones realizadas por el **forestprep** comandos en Windows Server 2012 (operaciones de 84 130) están completos, la **revisión** atributo CN = ActiveDirectoryUpdate, CN = ForestUpdates, CN = Configuration, DC = dominioRaízDelBosque objeto se establece en **11**.
  
|Número de operación y el GUID|Descripción|Atributos|Permisos|
|-----------------------------|---------------|--------------|---------------|
|**Operación 84**: {4664e973-cb20-4def-b3d5-559d6fe123e0}|Crea un nuevo contenedor CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-objectClass: contenedor|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 85**: {2972d92d-a07a-44ac-9cb0-bf243356f345}|Crea un nuevo objeto CN = tipos de notificación, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ClaimTypes<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 86**: {09a49cb3-6c54-4b83-ab20-8370838ba149}|Crea un nuevo objeto CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperties<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 87**: {77283e65-ce02-4dc3-8c1e-bf99b22527c2}|Crea un nuevo contenedor CN = listas de propiedades de recursos, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-objectClass: contenedor<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 88**: {0afb7f53-96bd-404b-a659-89e65c269420}|Crea un nuevo objeto CN = Sam dominio en la partición del esquema.|N/D|Crea la entrada de control de acceso (ACE) para conceder la propiedad de escritura a automático Principal en el objeto siguiente:<br /><br />(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Operación 89**: {c7f717ef-fdbe-4b4b-8dfc-fa8b839fbcfa}|Crea un nuevo objeto CN = dominio DNS en la partición del esquema.|N/D|Crea la entrada de control de acceso (ACE) para conceder la propiedad de escritura a automático Principal en el objeto siguiente:<br /><br />(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Operación 90**: {00232167-f3a4-43c6-b503-9acb7a81b01c}|Llamada de función para actualizar los especificadores de presentación.|N/D|N/D|
|**Operación 91**: {73a9515b-511c-44d2-822b-444a33d3bd33}|Crea un nuevo contenedor CN = Microsoft SPP, CN = Services en la partición de configuración.|-objectClass: contenedor<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 92**: {e0c60003-2ed7-4fd3-8659-7655a7e79397}|Crea un nuevo contenedor de objetos de activación, CN = objetos de activación, CN = Microsoft SPP, CN = Services en la partición de configuración.|-   objectClass: msSPP-ActivationObjectsContainer<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 93**: {ed0c8cca-80ab-4b6b-ac5a-59b1d317e11f}|Crea un nuevo contenedor de directivas de acceso Central, CN = directivas de acceso Central, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msAuthz-CentralAccessPolicies<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 94**: {b6a6c19a-afc9-476b-8994-61f5b14b3f05}|Crea un nuevo contenedor de entradas de la directiva de acceso Central, CN = reglas de acceso Central, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msAuthz-CentralAccessRules<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 95**: {defc28cd-6cb6-4479-8bcb-aabfb41e9713}|Crea un nuevo contenedor de servicio de distribución de la clave de grupo CN = servicio de distribución de claves de grupo, CN = Services en la partición de configuración.|-objectClass: contenedor<br />: Descripción: El contenedor contiene la configuración y los datos para el servicio de distribución de claves de grupo.<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 96**: {d6bd96d4-e66b-4a38-9c6b-e976ff58c56d}|Crea un nuevo contenedor de claves raíz de Master, CN = claves maestras de raíz, CN = servicio de distribución de claves de grupo, CN = Services en la partición de configuración.|-objectClass: contenedor<br />: Descripción: El contenedor contiene claves raíz maestra de servicio de distribución de claves de grupo.<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 97**: {bb8efc40-3090-4fa2-8a3f-7cd1d380e695}|Crea un nuevo contenedor de configuración del servidor, CN = Server Configuration, CN = servicio de distribución de claves de grupo, CN = Services en la partición de configuración.|-objectClass: contenedor<br />: Descripción: El contenedor contiene configuraciones de servicio de distribución de claves de grupo.<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 98**: {2d6abe1b-4326-489e-920c-76d5337d2dc5}|Crea un nuevo contenedor de objetos de configuración de servidor vacío CN = Configuración del servidor de servicio de distribución de grupo clave, CN = Server Configuration, CN = servicio de distribución de claves de grupo, CN = Services en la partición de configuración.|-   objectClass: msKds-ProvServerConfiguration<br />: Descripción: La configuración de los algoritmos de criptografía usados por el servicio de distribución de claves de grupo.<br />-   msKds-Version: 1<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 99**: {6b13dfb5-cecc-4fb8-b28d-0505cea24175}|Crea un nuevo contenedor de configuración de directivas de transformación de notificaciones CN = directivas de transformación de notificaciones, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ClaimsTransformationPolicies<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCDCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 100**: {92e73422-c68b-46c9-b0d5-b55f9c741410}|Crea un nuevo contenedor de configuración de los tipos de valor CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-objectClass: contenedor<br />-   showInAdvancedViewOnly: True|(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 101**: {c0ad80b4-8e84-4cc4-9163-2f84649bcc42}|Crea un objeto de configuración de tipo de valor de SinglevaluedChoice nuevo CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo para crear una propiedad de recurso. Al asignar el valor a una propiedad de recurso de este tipo de valor, un usuario puede elegir sólo una entrada de una lista de valores sugeridos.<br />-   displayname: Elección de un solo valor<br />-   msDS-ClaimValueType: 3<br />-   msDS-ClaimIsValueSpaceRestricted: True<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: True<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 102**: {992fe1d0-6591-4f24-a163-c820fcb7f308}|Crea un objeto de configuración de tipo de valor de SíNo nuevo CN = MS-DS-SíNo, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Los valores válidos para este tipo son Yes o no.<br />-   displayname: Sí/No<br />-   msDS-ClaimValueType: 6<br />-   msDS-ClaimIsValueSpaceRestricted: False<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: False<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 103**: {ede85f96-7061-47bf-b11b-0c0d999595b5}|Crea un nuevo número valor tipo configuración objeto CN = MS-DS-Number, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo de recurso de autor que contienen un número único.<br />-   displayname: Número<br />-   msDS-ClaimValueType: 1<br />-   msDS-ClaimIsValueSpaceRestricted: False<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: False<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 104**: {ee0f3271-eb51-414a-bdac-8f9ba6397a39}|Crea un objeto de configuración de tipo de valor de fecha y hora nueva CN = MS-DS-DateTime, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo a las propiedades de recursos de autor que tienen el formato de fecha y hora.<br />-   displayname: Fecha y hora<br />-   msDS-ClaimValueType: 1<br />-   msDS-ClaimIsValueSpaceRestricted: False<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: False<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 105**: {587d52e0-507e-440e-9d67-e6129f33bb68}|Crea un objeto de configuración de tipo de valor de OrderedList nuevo CN = MS-DS-OrderedList, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo de recurso de autor que contienen una entrada única opción que puede compararse con otras propiedades de recursos del mismo tipo. Normalmente, un usuario elige la entrada de una lista de valores sugeridos ordenadas proporcionadas por ms-DS-notificación-posible-Values en las propiedades del recurso.<br />-   displayname: Lista ordenada<br />-   msDS-ClaimValueType: 1<br />-   msDS-ClaimIsValueSpaceRestricted: True<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: True<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 106**: {ce24f0f6-237e-43d6-ac04-1e918ab04aac}|Crea un objeto de configuración de tipo de valor de texto nuevo CN = MS-DS-Text, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo a las propiedades de recursos de autor que contienen una entrada de texto única.<br />-   displayname: Text<br />-   msDS-ClaimValueType: 3<br />-   msDS-ClaimIsValueSpaceRestricted: False<br />-   msDS-ClaimIsSingleValued: True<br />-   msDS-IsPossibleValuesPresent: False<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 107**: {7f77d431-dd6a-434f-ae4d-ce82928e498f}|Crea un objeto de configuración de tipo de valor de MultivaluedText nuevo CN = MS-DS-MultivaluedText, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo a las propiedades de recursos de autor que pueden tener varias entradas de texto.<br />-   displayname: Texto con varios valores<br />-   msDS-ClaimValueType: 3<br />-   msDS-ClaimIsValueSpaceRestricted: False<br />-   msDS-ClaimIsSingleValued: False<br />-   msDS-IsPossibleValuesPresent: False<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 108**: {ba14e1f6-7cd1-4739-804f-57d0ea74edf4}|Crea un objeto de configuración de tipo de valor de MultivaluedChoice nuevo CN = MS-DS-MultivaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ValueType<br />: Descripción: Puede usar este tipo a las propiedades de recursos de autor que pueden tener varias entradas que no se pueden comparar. Normalmente, un usuario elige cada entrada de una lista de valores sugeridos proporcionadas por ms-DS-notificación-posible-Values en las propiedades del recurso.<br />-   displayname: Opción de valor múltiple<br />-   msDS-ClaimValueType: 3<br />-   msDS-ClaimIsValueSpaceRestricted: True<br />-   msDS-ClaimIsSingleValued: False<br />-   msDS-IsPossibleValuesPresent: True<br />-   showInAdvancedViewOnly: True|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|  
|**Operación 109**: {156ffa2a-e07c-46fb-a5c4-fbd84a4e5cce}|Crea un nuevo objeto de propiedad de recurso de información de identificación personal CN = PII_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de información identificación personal (PII) especifica si el recurso contiene información de identificación personal y si lo hace, ¿qué es el nivel de confidencialidad de dicha información.<br />-   displayname: Información de identificación personal<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-OrderedList, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 110**: {7771d7dd-2231-4470-aa74-84a6f56fc3b6}|Crea un nuevo objeto de propiedad de recurso de información sanitaria protegida CN = ProtectedHealthInformation_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de información de salud protegida (PHI) especifica si el recurso contiene todos los datos relacionados con una persona médicos o historial de pagos médicos.<br />-   displayname: Información sanitaria protegida<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SíNo, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 111**: {49b2ae86-839a-4ea0-81fe-9171c1b98e83}|Crea un nuevo objeto de propiedad de recurso de limpieza necesario CN = RequiredClearance_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de limpieza necesario especifica el nivel de permiso que debe poseer un usuario antes de intentar tener acceso al recurso.<br />-   displayname: Limpieza necesaria<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-OrderedList, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 112**: {1b1de989-57ec-4e96-b933-8279a8119da4}|Crea un nuevo objeto de propiedad de recurso de confidencialidad CN = Confidentiality_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad confidencialidad especifica el nivel de confidencialidad del recurso y el posible impacto de involuntario acceso o divulgación.<br />-   displayname: Confidencialidad<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-OrderedList, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 113**: {281c63f0-2c9a-4cce-9256-a238c23c0db9}|Crea un nuevo objeto de propiedad de recurso de conformidad CN = Compliancy_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de cumplimiento especifica los marcos de cumplimiento que se aplican a los recursos.<br />-   displayname: Cumplimiento<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-MultivaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 114**: {4c47881a-f15a-4f6c-9f49-2742f7a11f4b}|Crea un nuevo objeto de propiedad de recurso de detectabilidad CN = Discoverability_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de detectabilidad especifica si el recurso contiene la evidencia posibles que puede requerir la divulgación a opuesta al asesor legal durante el transcurso de litigios actuales o futuras.<br />-   displayname: Detectabilidad<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operation 115**: {2aea2dc6-d1d3-4f0c-9994-66c1da21de0f}|Crea un nuevo objeto de propiedad de recurso inmutable CN = Immutable_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad inmutable especifica si se debe permitir a un usuario para eliminar un recurso o cambiar su contenido.<br />-   displayname: Inmutable<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SíNo, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 116**: {ae78240c-43b9-499e-ae65-2b6e0f0e202a}|Crea un nuevo objeto de propiedad de recurso de la propiedad intelectual CN = IntellectualProperty_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de la propiedad intelectual (PI) especifica si el recurso contiene IP y, si es así, qué tipo.<br />-   displayname: Propiedad intelectual e industrial<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 117**: {261b5bba-3438-4d5c-a3e9-7b871e5f57f0}|Crea un nuevo objeto de propiedad de recurso de departamento CN = Department_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad del departamento especifica el nombre del departamento al que pertenece el recurso.<br />-   displayname: Departmento<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 118**: {3fb79c05-8ea1-438c-8c7a-81f213aa61c2}|Crea un nuevo objeto de propiedad de recurso de impacto CN = Impact_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de impacto especifica el grado de impacto en la organización contra el acceso inadecuado o pérdida del recurso.<br />-   displayname: Impacto<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-OrderedList, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN = < dominio raíz del bosque<br />-   msDS-ClaimPossibleValues: Alto - empresarial alta afectar impacto de mediana empresa (HBI) - 3000, moderado - (MBI) - 2000, baja: bajo impacto de negocio (LBI) - 1000 >|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 119**: {0b2be39a-d463-4c23-8290-32186759d3b1}|Crea un nuevo objeto de propiedad de recurso de uso Personal CN = PersonalUse_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad Use Personal especifica si el archivo es para uso personal (no relacionados con la empresa).<br />-   displayname: Uso personal<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SíNo, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 120**: {f0842b44-bc03-46a1-a860-006e8527fccd}|Crea un nuevo objeto de propiedad de recurso de proyecto CN = Project_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de proyecto especifica los nombres de uno o varios proyectos que son relevantes para el recurso.<br />-   displayname: Proyecto<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-MultivaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 121**: {93efec15-4dd9-4850-bc86-a1f2c8e2ebb9}|Crea un nuevo objeto de propiedad de recurso de período de retención CN = RetentionPeriod_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de período de retención especifica el período máximo para el que debe conservarse el archivo.<br />-   displayname: Período de retención<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 122**: {9e108d96-672f-40f0-b6bd-69ee1f0b7ac4}|Crea un nuevo objeto de propiedad de recurso de retención Start Date CN = RetentionStartDate_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad fecha de inicio de retención define la fecha de inicio para un período de retención. El período de retención comenzaría en la fecha de inicio de retención.<br />-   displayname: Fecha de inicio de retención<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: False<br />-   msDS-ValueTypeReference: CN = MS-DS-DateTime, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 123**: {1e269508-f862-4c4a-b01f-420d26c4ff8c}|Crea un nuevo objeto de propiedad de recursos de empresa CN = Company_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La compañía de propiedad especifica que el recurso de la empresa que pertenece.<br />-   displayname: Compañía<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: True<br />-   msDS-ValueTypeReference: CN = MS-DS-SinglevaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 125**: {e1ab17ed-5efb-4691-ad2d-0424592c5755} **Nota:** Se eliminó la operación 124.|Crea un nuevo objeto de propiedad de recurso de uso de la carpeta CN = FolderUsage_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourceProperty<br />: Descripción: La propiedad de uso de carpeta especifica el propósito de la carpeta y el tipo de los archivos almacenados en él.<br />-   displayname: Uso de carpeta<br />-Habilitado: False<br />-   msDS-IsUsedAsResourceSecurityAttribute: False<br />-   msDS-AppliestoResourceTypes: MS-DS-Container<br />-   msDS-ValueTypeReference: CN = MS-DS-MultivaluedChoice, CN = tipos de valor, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)| 
|**Operación 126**: {0e848bd4-7c70-48f2-b8fc-00fbaa82e360}|Crea un nuevo objeto de configuración de lista de propiedades de recursos Global CN = lista de propiedades de recursos Global, CN = listas de propiedades de recursos, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|-   objectClass: msDS-ResourcePropertyList<br />: Descripción: Esto es global fuera de la lista de propiedades de recurso de cuadro que contiene todas las propiedades de recursos que pueden consumir las aplicaciones.<br />-   showInAdvancedViewOnly: True<br />-   msDS-MembersOfResourcePropertyList: CN = PII_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = ProtectedHealthInformation_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = RequiredClearance_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Confidentiality_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Compliancy_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Discoverability_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Immutable_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = IntellectualProperty_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Department_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Impact_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = PersonalUse_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Project_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = RetentionPeriod_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = RetentionStartDate_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = Company_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain><br />-   msDS-MembersOfResourcePropertyList: CN = FolderUsage_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services, CN = Configuration, CN =<forest root domain>|(D.; SDDT;; WD)<br />(A; RPLCLORC;; AU)<br />(A;;RPWPCRLCLOCCRCWDWOSW;;;EA)<br />(A; RPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)|
|**Operación 127**: {016f23f7-077d-41fa-a356-de7cfdb01797}|Llamada de función para actualizar los especificadores de presentación.|N/D|N/D|
|**Operación 128**: {49c140db-2de3-44c2-a99a-bab2e6d2ba81}|Actualizar las cadenas para el objeto de propiedad de recurso de uso de carpeta CN = FolderUsage_MS, CN = propiedades del recurso, CN = Configuración de notificaciones, CN = Services en la partición de configuración.|: Descripción: La propiedad de uso de carpeta especifica el propósito de la carpeta y el tipo de los archivos almacenados en él.|N/D|
|**Operation 129**: {e0b11c80-62c5-47f7-ad0d-3734a71b8312}|Agrega la ACE que se va a conceder la propiedad Principal de escritura de autoservicio y la propiedad de lectura en CN = objeto de dominio de Sam.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|
|**Operación 130**: {2ada1a2d-b02f-4731-b4fe-59f955e24f71}|Agrega la ACE que se va a conceder la propiedad Principal de escritura de autoservicio y la propiedad de lectura en CN = objeto de dominio DNS.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|