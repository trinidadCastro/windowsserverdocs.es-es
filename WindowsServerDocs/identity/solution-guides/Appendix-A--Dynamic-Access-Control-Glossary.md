---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: 'Apéndice a: Glosario de Control de acceso dinámico'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856706"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Apéndice A: Glosario de Control de acceso dinámico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Estos son la lista de términos y definiciones que se incluyen en el escenario de Control de acceso dinámico.  
  
|Término|Definición|  
|--------|--------------|  
|Clasificación automática|Clasificación que se produce según las propiedades de clasificación que vienen determinadas por las reglas de clasificación configuradas por un administrador.|  
|CAPID|Identificador de directiva de acceso central. Este identificador hace referencia a una directiva de acceso central específica y se utiliza para hacer referencia a la directiva desde el descriptor de seguridad de archivos y carpetas.|  
|Regla de acceso central|Una regla que incluye una condición y una expresión de acceso.|  
|Directiva de acceso central|Directivas que se crean y se hospedan en Active Directory.|  
|Control de acceso basado en notificaciones|Un paradigma que utiliza notificaciones para tomar decisiones de control de acceso a los recursos.|  
|Clasificación|El proceso de determinación de las propiedades de clasificación de recursos y asignación de estas propiedades a los metadatos que están asociados a los recursos. Vea también \h REF AutomaticClassification \\* la clasificación automática COMFORMATO, REF InheritedClassification \h \\ \* COMFORMATO heredan la clasificación y REF ManualClassification \h \\ \* Clasificación Manual COMFORMATO.|  
|Notificación de dispositivo|Una notificación que está asociada con el sistema.  Con las notificaciones de usuario, se incluye en el símbolo (token) de un usuario intenta acceder a un recurso.|  
|Lista de control de acceso discrecional (DACL)|Una lista de control de acceso que identifica los elementos de confianza que se permiten o deniegan el acceso a un recurso protegible. Se puede modificar a discreción del propietario del recurso.|  
|Propiedad de recurso|Propiedades (por ejemplo, etiquetas) que describen un archivo y se asignan a los archivos mediante la clasificación automática o la clasificación manual. Algunos ejemplos son: Período de sensibilidad, proyecto y la retención.|  
|Administrador de recursos del servidor de archivos|Una característica en el sistema operativo Windows Server que ofrece administración de cuotas de carpeta, filtrado de archivos, los informes de almacenamiento, la clasificación de archivos y los trabajos de administración de archivos en un servidor de archivos.|  
|Las etiquetas y propiedades de la carpeta|Las propiedades y etiquetas que describen una carpeta y se asignan manualmente por los administradores y propietarios de las carpetas. Estas propiedades asignan valores de propiedad de predeterminada a los archivos dentro de estas carpetas, por ejemplo, la confidencialidad o el departamento.|  
|Directiva de grupo|Un conjunto de reglas y directivas que controla el entorno de trabajo de usuarios y equipos en un entorno de Active Directory.|  
|Cerca de la clasificación en tiempo real|Clasificación automática que se realiza poco después de que se crea o modifica un archivo.|  
|Cerca de las tareas de administración de archivos en tiempo real|Tareas de administración que se realizan poco después de archivos (un archivo se crea o modifica. Estas tareas se desencadenan la clasificación casi en tiempo real.|  
|Unidad organizativa (OU)|Un contenedor de Active Directory que representa las estructuras jerárquicas lógicas dentro de una organización. Es el ámbito más pequeño para la directiva de grupo de configuración se aplica.|  
|Propiedad segura|Una propiedad de clasificación que puede confiar en el tiempo de ejecución de autorización para que sea una aserción válida sobre el recurso en un determinado en un momento. En el control de acceso basado en notificaciones, una propiedad segura que se asigna a un recurso se trata como una notificación de recursos.|  
|descriptor de seguridad|Una estructura de datos que contiene información de seguridad asociada a un recurso protegible, como listas de control de acceso.|  
|Lenguaje de definición de descriptores de seguridad|Una especificación que describe la información de un descriptor de seguridad como una cadena de texto.|  
|Directiva de almacenamiento provisional|Una directiva de acceso central que no está aún en vigor.|  
|Lista de control de acceso de sistema (SACL)|Una lista de control de acceso que especifica los tipos de personas de confianza determinadas para que los registros de auditoría deben generarse los intentos de acceso.|  
|Notificación de usuario|Atributos de un usuario que se proporcionan en el token de seguridad del usuario. Algunos ejemplos son: Limpieza de departamento, compañía, proyecto y seguridad.  También se puede considerar la información del token de usuario desde sistemas anteriores a Windows Server 2012, como los grupos de seguridad que el usuario forma parte de las notificaciones de usuario. Se proporcionan algunas notificaciones de usuario a través de Active Directory y otros se calculan dinámicamente, por ejemplo, si el usuario ha iniciado sesión con una tarjeta inteligente.|  
|Token de usuario|Un objeto de datos que identifica un usuario y las notificaciones de usuario y notificaciones de dispositivo que están asociadas a ese usuario. Sirve para autorizar el acceso del usuario a los recursos.|  
  
## <a name="see-also"></a>Vea también  
[Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


