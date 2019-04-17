---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: "Glosario de Control de acceso dinámico Apéndice a:"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Apéndice A: Glosario de Control de acceso dinámico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes son la lista de términos y definiciones que se incluyen en el escenario de Control de acceso dinámico.  
  
|Término|Definición|  
|--------|--------------|  
|Clasificación automática|Clasificación que se produce en función de las propiedades de clasificación que viene determinadas por las reglas de clasificación que un administrador configuradas.|  
|CAPID|Id. de directiva de acceso central Este identificador hace referencia a una directiva de acceso central específicos, y se usa para hacer referencia a la directiva de descriptor de seguridad de archivos y carpetas.|  
|Reglas de acceso central|Una regla que incluya una condición y una expresión de acceso.|  
|Directiva de acceso central|Directivas creadas y alojadas en Active Directory.|  
|Control de acceso basado en notificaciones|Un paradigma que usa notificaciones para tomar decisiones de control de acceso a los recursos.|  
|Clasificación|El proceso de determinación de las propiedades de clasificación de recursos y asignar estas propiedades a los metadatos que está asociado a los recursos. Consulta también REF AutomaticClassification \h \\ * clasificación COMFORMATO automática, REF InheritedClassification \h \\\ * COMFORMATO heredado de clasificación y REF ManualClassification \h \\\ * clasificación COMFORMATO Manual.|  
|Notificación de dispositivo|Una reclamación que está asociada con el sistema.  Con las notificaciones de usuario, que se incluye en el token de un usuario intenta acceder a un recurso.|  
|Lista de control de acceso discrecional (DACL)|Una lista de control de acceso que identifica los administradores de confianza que se permite o deniega el acceso a un recurso protegible. Se puede modificar la discreción de propietario de recursos.|  
|Propiedad de recurso|Propiedades (como las etiquetas) que describen un archivo y se asignan a archivos mediante el uso de clasificación automática o manual clasificación. Algunos ejemplos son: período de retención, proyecto y sensibilidad.|  
|Administrador de recursos del servidor de archivos|Una característica en el sistema operativo Windows Server que ofrece administración de cuotas de carpeta, filtrado de archivos, informes de almacenamiento, clasificación de archivos y trabajos de administración de archivos en un servidor de archivos.|  
|Etiquetas y propiedades de carpeta|Las propiedades y etiquetas que describen una carpeta y se les asigna manualmente los administradores y los propietarios de carpetas. Estas propiedades asignan valores de propiedad de forma predeterminada a los archivos de estas carpetas, por ejemplo, el secreto o el departamento.|  
|Directiva de grupo|Un conjunto de reglas y directivas que controla el entorno de trabajo de usuarios y equipos en un entorno de Active Directory.|  
|Cerca de clasificación en tiempo real|Clasificación automática que se realiza poco después de que se crea o modifica un archivo.|  
|Cerca de las tareas de administración de archivos en tiempo real|Tareas de administración que se llevan a cabo poco después del archivo (un archivo se crea o modifica. Estas tareas se desencadenan por la clasificación casi en tiempo real.|  
|Unidad organizativa (OU)|Un contenedor de Active Directory que representa estructuras lógicas jerárquicas dentro de una organización. Es el ámbito más pequeño para la directiva de grupo de configuración se aplica.|  
|Propiedad segura|Una propiedad de clasificación que pueda confiar en el tiempo de ejecución de autorización para que sea una aserción válida sobre el recurso en un determinado en un momento. En el control de acceso basado en notificaciones, una propiedad segura que se asigna a un recurso se trata como una reclamación de recursos.|  
|Descriptor de seguridad|Estructura de datos que contiene información de seguridad asociada a un recurso protegible, como listas de control de acceso.|  
|Lenguaje de definición de descriptores de seguridad|Especificación que describe la información de un descriptor de seguridad como una cadena de texto.|  
|Directiva de almacenamiento provisional|Una directiva de acceso central que no está aún en vigor.|  
|Lista de control de acceso del sistema (SACL)|Una lista de control de acceso que especifica los tipos de intentos de acceso concretos administradores de confianza para el que deben generarse los registros de auditoría.|  
|Notificación de usuario|Atributos de un usuario que se proporcionan en el token de seguridad del usuario. Algunos ejemplos son: limpieza departamento, la empresa, proyecto y seguridad.  Información del token de usuario de sistemas anteriores a Windows Server 2012, como los grupos de seguridad que el usuario forma parte, también puede considerarse notificaciones de usuario. Se proporcionan algunas notificaciones de usuario a través de Active Directory y otros se calculan de forma dinámica, como si la sesión del usuario con una tarjeta inteligente.|  
|Token de usuario|Un objeto de datos que identifica un usuario y las notificaciones de usuario y reclamaciones de dispositivos que están asociados a ese usuario. Sirve para autorizar el acceso del usuario a los recursos.|  
  
## <a name="see-also"></a>Consulta también  
[Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


