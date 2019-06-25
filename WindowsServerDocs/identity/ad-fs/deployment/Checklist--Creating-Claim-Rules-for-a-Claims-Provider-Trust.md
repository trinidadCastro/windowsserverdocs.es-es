---
ms.assetid: 503733f8-be0c-429c-81f0-cd4205e8b118
title: 'Lista de comprobación: creación de reglas de notificación para un proveedor de notificaciones de confianza'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 708919691f88cc49d1f2bd74d8f4255e1a854353
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192392"
---
# <a name="checklist-creating-claim-rules-for-a-claims-provider-trust"></a>Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza


Esta lista de comprobación incluye tareas para planear, diseñar, y para implementar reglas de notificación que están asociadas con un proveedor de notificaciones de confianza en los servicios de federación de Active Directory \(AD FS\).  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![creación de reglas de notificación](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Creación de un conjunto de reglas de notificación para una confianza de proveedor de notificaciones**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Revise los conceptos sobre notificaciones, las reglas de notificación, conjuntos de reglas de notificación y las plantillas de reglas y cómo están vinculados con relaciones de confianza federadas de notificación.|![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of Claims](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of Claim Rules](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Revisar conceptos acerca de cómo fluye una notificación a través de todas las fases de la canalización de emisión de notificaciones y cómo se procesan las reglas el motor de emisión de notificaciones.|![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of the Claims Pipeline](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Para planear e implementar las notificaciones de salida a través de esta relación de confianza de proveedor de notificaciones se emitirán eficazmente, determinar si son necesarios uno o más reglas de notificación y que debe usar con esta relación de confianza de proveedor de notificaciones de reglas de notificación.|![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinar el tipo de notificación de plantilla de regla para uso](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Revisar conceptos al crear una notificación de regla frente a otro y cómo puede usar el lenguaje de reglas de notificación para proporcionar una lógica más compleja que las reglas estándares para proporcionar un resultado deseado en la salida ideal conjunto de notificaciones.|![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo usar un paso a través o la regla de filtro de notificación](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe usar una regla de notificación de transformación](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe utilizar a Send LDAP Attributes as Claims Rule](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe usar una pertenencia al grupo de envío como una regla de notificación](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe usar una regla de notificación personalizada](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![creación de reglas de notificación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[The Role of el lenguaje de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Se debe crear una descripción de notificación si no existe ya que satisfagan las necesidades de su organización. AD FS se suministra con un conjunto predeterminado de descripciones de notificaciones que se exponen en el complemento Administración de AD FS\-en.|![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[agregar una descripción de notificación](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![creación de reglas de notificación](media/icon_checkboxo.gif)|Según las necesidades de su organización, cree una o varias reglas de notificación para el conjunto de reglas de transformación de aceptación que está asociado a esta relación de confianza de proveedor de notificaciones para que las notificaciones se emitirán adecuadamente.|![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[cree una regla de paso a través o filtrar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para enviar pertenencia a grupo como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para enviar una notificación de método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para enviar AD FS 1.x de notificación Compatible](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![creación de reglas de notificación](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear una regla para enviar notificaciones mediante regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
  
