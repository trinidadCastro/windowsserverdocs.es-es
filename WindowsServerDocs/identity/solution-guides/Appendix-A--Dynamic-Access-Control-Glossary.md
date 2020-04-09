---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Apéndice A Dynamic Access Control Glosario
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 328d8c74e57150c94d0a6032ff88a3600fe0baf3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859288"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Anexo A: glosario de control de acceso dinámico

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A continuación se muestra la lista de términos y definiciones que se incluyen en el escenario de Access Control dinámico.  
  
|Término|Definición|  
|--------|--------------|  
|Clasificación automática|Clasificación que se produce en función de las propiedades de clasificación determinadas por las reglas de clasificación configuradas por un administrador.|  
|CAPId|IDENTIFICADOR de la Directiva de acceso central. Este identificador hace referencia a una directiva de acceso central específica y se usa para hacer referencia a la directiva desde el descriptor de seguridad de archivos y carpetas.|  
|Regla de acceso central|Una regla que incluye una condición y una expresión de acceso.|  
|Directiva de acceso central|Las directivas que se crean y hospedan en Active Directory.|  
|Control de acceso basado en notificaciones|Paradigma que emplea las notificaciones para tomar decisiones de control de acceso a los recursos.|  
|Clasificación|Proceso de determinar las propiedades de clasificación de los recursos y asignar estas propiedades a los metadatos que están asociados a los recursos. Vea también REF AutomaticClassification \h \\* comformación automática, REF InheritedClassification \h \\\* clasificación heredada de commergeformat y REF ManualClassification \h \\\* comformar la clasificación manual.|  
|Notificaciones de dispositivo|Una demanda asociada al sistema.  Con las notificaciones de usuario, se incluye en el token de un usuario que intenta tener acceso a un recurso.|  
|Lista de control de acceso discrecional (DACL)|Una lista de control de acceso que identifica a los usuarios de confianza a los que se permite o deniega el acceso a un recurso protegible. Se puede modificar a discreción del propietario del recurso.|  
|Propiedad de recurso|Propiedades (como etiquetas) que describen un archivo y se asignan a archivos mediante la clasificación automática o la clasificación manual. Algunos ejemplos son: sensibilidad, proyecto y período de retención.|  
|File Server Resource Manager|Característica del sistema operativo Windows Server que ofrece administración de cuotas de carpetas, filtrado de archivos, informes de almacenamiento, clasificación de archivos y trabajos de administración de archivos en un servidor de archivos.|  
|Propiedades y etiquetas de la carpeta|Propiedades y etiquetas que describen una carpeta y que los administradores y propietarios de carpetas asignan manualmente. Estas propiedades asignan los valores de propiedad predeterminados a los archivos de estas carpetas, por ejemplo, la confidencialidad o el Departamento.|  
|Directiva de grupo|Conjunto de reglas y directivas que controla el entorno de trabajo de usuarios y equipos en un entorno de Active Directory.|  
|Clasificación casi en tiempo real|Clasificación automática que se realiza poco después de crear o modificar un archivo.|  
|Tareas de administración de archivos casi en tiempo real|Tareas de administración de archivos que se realizan poco después (se crea o se modifica un archivo). Estas tareas se desencadenan por la clasificación casi en tiempo real.|  
|Unidad organizativa (OU)|Active Directory contenedor que representa las estructuras lógicas jerárquicas dentro de una organización. Es el ámbito más pequeño al que se aplica la configuración de directiva de grupo.|  
|Propiedad Secure|Propiedad de clasificación en la que el Runtime de autorización puede confiar para ser una aserción válida sobre el recurso en un momento determinado. En el control de acceso basado en notificaciones, una propiedad segura que se asigna a un recurso se trata como una notificación de recursos.|  
|Descriptor de seguridad|Una estructura de datos que contiene información de seguridad asociada a un recurso protegible, como listas de control de acceso.|  
|Lenguaje de definición de descriptores de seguridad|Especificación que describe la información de un descriptor de seguridad como una cadena de texto.|  
|Directiva de ensayo|Una directiva de acceso central que aún no está en vigor.|  
|Lista de control de acceso del sistema (SACL)|Una lista de control de acceso que especifica los tipos de intentos de acceso por parte de los datos de confianza específicos para los que se deben generar registros de auditoría.|  
|Notificaciones de usuario|Atributos de un usuario que se proporcionan en el token de seguridad del usuario. Algunos ejemplos son: Departamento, compañía, proyecto y autorización de seguridad.  La información del token de usuario de los sistemas anteriores a Windows Server 2012, como los grupos de seguridad de los que forma parte el usuario, también se puede considerar como notificaciones de usuario. Algunas notificaciones de usuario se proporcionan a través de Active Directory y otras se calculan dinámicamente, por ejemplo, si el usuario ha iniciado sesión con una tarjeta inteligente.|  
|Token de usuario|Objeto de datos que identifica un usuario y las notificaciones de usuario y notificaciones de dispositivo asociadas a ese usuario. Se usa para autorizar el acceso del usuario a los recursos.|  
  
## <a name="see-also"></a>Consulta también  
[Access Control dinámico: información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


