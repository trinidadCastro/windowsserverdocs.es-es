---
title: Implementación del entorno de Escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
description: Pasos básicos para implementar un entorno de escritorio remoto.
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: a5a56d56038d94869c5246f8d4d3eae2796616a3
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297444"
---
# Implementación del entorno de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Usar los siguientes pasos para implementar los servidores de escritorio remoto en tu entorno. Puede instalar los roles de servidor en equipos físicos o máquinas virtuales, dependiendo de si vas a crear un locales, basada en la nube o entorno híbrido. 

Si estás usando las máquinas virtuales para cualquiera de los servidores de servicios de escritorio remoto, asegúrate de que tienes [preparado dichas máquinas virtuales](rds-prepare-vms.md).
  
  
1.  Agrega todos los servidores que vas a usar para servicios de escritorio remoto al administrador de servidores:  
    1.  En el Administrador de servidores, haz clic en **Administrar > agregar servidores**.  
    2.  Haz clic en **Buscar ahora**.  
    3.  Haz clic en cada servidor de la implementación (por ejemplo, Contoso Cb1, Contoso WebGw1 y Sh1 de Contoso) y haz clic en **Aceptar**.  
2.  Crear una implementación basada en la sesión para implementar los componentes de servicios de escritorio remoto:  
    1.  En el Administrador de servidores, haz clic en **Administrar > agregar Roles y características**.  
    2.  Haz clic en la **instalación de servicios de escritorio remoto**, **Implementación estándar**y la **implementación de escritorio basada en la sesión**.  
    3.  Selecciona los servidores adecuados para el servidor de agente de conexión a Escritorio remoto, el servidor de acceso Web de RD y el servidor Host de sesión de escritorio remoto (por ejemplo, Contoso Cb1, Contoso WebGw1 y SH1 de Contoso, respectivamente).  
    4.  Seleccione **reiniciar el servidor de destino automáticamente si es necesario**y, a continuación, haga clic en **implementar**.  
    5.  Espera a que la implementación completar correctamente  
3.  Agregue el servidor de licencias de escritorio remoto:  
    1.  En el Administrador de servidores, haz clic en **Servicios de escritorio remoto > Introducción > + licencias de escritorio remoto**.  
    2.  Seleccione la máquina virtual donde se instalará el servidor de licencias de escritorio remoto (por ejemplo, Contoso Cb1).  
    3.  Haz clic en **siguiente**y, a continuación, haz clic en **Agregar**.  
4.  Activar el servidor de licencias de escritorio remoto y agregarla al grupo de servidores de licencias:  
    1.  En el Administrador de servidores, haz clic en **las herramientas > Terminal Services > el Administrador de licencias de escritorio remoto**.  
    2.  En el Administrador de licencias de escritorio remoto, selecciona el servidor y, a continuación, haz clic en **acción > activar servidor**.  
    3.  Acepta los valores predeterminados en el Asistente para activar de servidor acepte las opciones predeterminadas hasta llegar a la página de **información de la empresa** . A continuación, escribe la información de su organización.  
    4.  Acepta los valores predeterminados para el resto de las páginas hasta que la página final. **Iniciar el Asistente para instalar licencias ahora**de borrar y, a continuación, haga clic en **Finalizar**.  
    5.  Haz clic en **acción > Revisar configuración > se agrega al grupo > Aceptar**. Escribe las credenciales de un usuario en el grupo de administradores de AAD DC y registrarse como SCP. Este paso no funcionen si estás usando los servicios de dominio de Azure AD, pero pueden ignorar las advertencias o errores.  
5.  Agrega el nombre de servidor y el certificado de puerta de enlace de escritorio remoto:  
    1.  En el Administrador de servidores, haz clic en **Servicios de escritorio remoto > Introducción > + puerta de enlace de escritorio remoto**.  
    2.  En el Asistente para agregar servidores de puerta de enlace de escritorio remoto, seleccione la máquina virtual donde quieres instalar al servidor de puerta de enlace de escritorio remoto (por ejemplo, Contoso-WebGw1).  
    3.  Escribe el nombre del certificado SSL para el servidor de puerta de enlace de escritorio remoto mediante el externo totalmente DNS nombre completo (FQDN) del servidor de puerta de enlace de escritorio remoto. En Azure, esto es la etiqueta de **nombre DNS** y usa el formato servicename.location.cloudapp.azure.com. Por ejemplo, contoso.westus.cloudapp.azure.com.  
    4.  Haz clic en **siguiente**y, a continuación, haz clic en **Agregar**.
6.  Crear e instalar certificados autofirmados para los servidores de puerta de enlace de escritorio remoto y agente de conexión a Escritorio remoto.

       > [!NOTE]
       > Si estás proporcionando y la instalación de certificados de una entidad de certificación de confianza, realice los procedimientos de h paso a paso k para cada rol. Debes tener el archivo .pfx disponible para cada uno de estos certificados.
       
    1.  En el Administrador de servidores, haz clic en **Servicios de escritorio remoto > Introducción > tareas > editar las propiedades de implementación**.  
    2.  Expanda **certificados**y, a continuación, desplázate hacia abajo en la tabla. Haz clic en la **puerta de enlace de escritorio remoto > crear nuevo certificado**.  
    3.  Escribe el nombre del certificado, con el FQDN externo del servidor de puerta de enlace de escritorio remoto (por ejemplo, contoso.westus.cloudapp.azure.com) y, a continuación, escribe la contraseña.  
    4.  Selecciona **este certificado de la tienda** y, a continuación, busca la carpeta compartida que ha creado para los certificados en un paso anterior. (Por ejemplo, \Contoso Cb1\Certificates).  
    5.  Escribe un nombre de archivo para el certificado (por ejemplo, ContosoRdGwCert) y, a continuación, haz clic en **Guardar**.  
    6.  Selecciona **Permitir que el certificado se agregará al almacén de certificados entidades de certificación raíz de confianza en los equipos de destino**y, a continuación, haz clic en **Aceptar**.  
    7.  Haga clic en **Aplicar**y, a continuación, espera a que el certificado que se aplicará correctamente en el servidor de puerta de enlace de escritorio remoto.  
    8.  Haz clic en el **acceso Web de RD > Seleccionar certificado existente**.  
    9.  Busca el certificado creado para el servidor de puerta de enlace de escritorio remoto (por ejemplo, ContosoRdGwCert) y, a continuación, haga clic en **Abrir**.  
    10. Escribe la contraseña para el certificado, selecciona **Permitir que el certificado se agregará al almacén de certificados raíz de confianza en los equipos de destino**y, a continuación, haz clic en **Aceptar**.  
    11. Haga clic en **Aplicar**y, a continuación, espera a que el certificado que se aplicará correctamente en el servidor de acceso Web de RD.  
    12. Repite los pasos 1-11 para el **Agente de conexión a Escritorio remoto - servicios de publicación**, con el FQDN interno del servidor de agente de conexión a Escritorio remoto para el nuevo nombre del certificado (por ejemplo y el **Agente de conexión a Escritorio remoto - habilitar inicio de sesión único en** Contoso-Cb1.Contoso.com).  
7.  Exportar pública autofirmados y copiarlos en un equipo cliente. Si estás usando los certificados de una entidad de certificación de confianza, puedes omitir este paso.  
    1.  Iniciar certlm.msc.  
    2.  Expanda **Personal**y, a continuación, haga clic en **certificados**.  
    3.  En el panel derecho, haz clic en el certificado de agente de conexión a Escritorio remoto pensado para la autenticación de cliente, por ejemplo, **Contoso Cb1.Contoso.com**.  
    4.  Haz clic en la **exportación de todas las tareas >**.  
    5.  Acepte que las opciones predeterminadas del Asistente para exportación de certificados aceptan valores predeterminados hasta llegar a la página de **archivo de exportación** .  
    6.  Busca la carpeta compartida que creaste para los certificados, por ejemplo \Contoso-Cb1\Certificates.  
    7.  Escribe un nombre de archivo, por ejemplo ContosoCbClientCert y, a continuación, haz clic en **Guardar**.  
    8.  Haz clic en **siguiente**y, a continuación, haga clic en **Finalizar**.  
    9.  Repita los pasos 1 a 8 para la puerta de enlace de escritorio remoto y el certificado de Web, (por ejemplo contoso.westus.cloudapp.azure.com), lo que da el certificado exportado un nombre de archivo adecuada, por ejemplo **ContosoWebGwClientCert**.  
    10. En el Explorador de archivos, ve a la carpeta donde se almacenan los certificados, por ejemplo \Contoso-Cb1\Certificates.  
    11. Selecciona los dos certificados de cliente exportado, a continuación, haz clic en ellos y haz clic en la **copia**.  
    12. Pegar el públicamente en el equipo cliente local.  
8.  Configurar las propiedades de implementación de puerta de enlace de escritorio remoto y licencias de escritorio remoto:  
    1.  En el Administrador de servidores, haz clic en **Servicios de escritorio remoto > Introducción > tareas > editar las propiedades de implementación**.  
    2.  Expande la **Puerta de enlace de escritorio remoto** y desactiva la opción **servidor de puerta de enlace de escritorio remoto de omisión para direcciones locales** .  
    3.  Expande **licencias de escritorio remoto** y selecciona **Por usuario**  
    4.  Haz clic en **Aceptar**.  
10. Crear una colección de sesión. Estos pasos, crean una colección básica. Echa un vistazo a [crear una colección de servicios de escritorio remoto para equipos de escritorio y ejecutar aplicaciones](rds-create-collection.md) para obtener más información acerca de las colecciones.
 
    1.  En el Administrador de servidores, haz clic en **Servicios de escritorio remoto > colecciones > tareas > crear la colección de sesión**.  
    2.  Escribe una nombre (por ejemplo, ContosoDesktop) de la colección.  
    3.  Seleccionar un servidor de Host de sesión de escritorio remoto (Contoso Sh1), acepta los grupos de usuarios de forma predeterminada (Contoso\Domain usuarios) y escribe la ruta de acceso de convención de nomenclatura Universal (UNC) en los discos de perfil de usuario creados anteriormente (\Contoso-Cb1\UserDisks).  
    4.  Establecer un tamaño máximo y, a continuación, haga clic en **crear**.  
  

Ahora, has creado una infraestructura básica de servicios de escritorio remoto. Si es necesario crear una implementación altamente disponible, puedes agregar un [clúster de agente de conexión](rds-connection-broker-cluster.md) o un [segundo servidor Host de sesión de escritorio remoto](rds-scale-rdsh-farm.md).

