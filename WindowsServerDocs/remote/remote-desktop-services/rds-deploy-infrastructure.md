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
ms.openlocfilehash: 5b9ce1bb87a7a2ad8819235edc412fd095bc2985
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805132"
---
# <a name="deploy-your-remote-desktop-environment"></a>Implementación del entorno de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Siga estos pasos para implementar los servidores de escritorio remoto en su entorno. Puede instalar los roles de servidor en equipos físicos o máquinas virtuales, dependiendo de si está creando una local, basada en la nube o entorno híbrido. 

Si usa máquinas virtuales para cualquiera de los servidores de servicios de escritorio remoto, asegúrese de que haya [preparado esas máquinas virtuales](rds-prepare-vms.md).
  
  
1.  Agregue todos los servidores que se va a usar para servicios de escritorio remoto al administrador del servidor:  
    1.  En el administrador del servidor, haga clic en **administrar** > **agregar servidores**.  
    2.  Haz clic en **Buscar ahora**.  
    3.  Haga clic en cada servidor de la implementación (por ejemplo, Contoso Cb1, Contoso WebGw1 y Contoso-SHA-1) y haga clic en **Aceptar**.  
2.  Crear una implementación basada en sesión para implementar los componentes de servicios de escritorio remoto:  
    1.  En el administrador del servidor, haga clic en **administrar** > **agregar Roles y características**.  
    2.  Haga clic en **instalación de servicios de escritorio remoto**, **implementación estándar**, y **la implementación de escritorio basados en sesión**.  
    3.  Seleccione los servidores adecuados para el servidor de agente de conexión a Escritorio remoto, servidor de acceso Web de escritorio remoto y servidor Host de sesión de escritorio remoto (por ejemplo, Contoso Cb1, Contoso WebGw1 y Contoso-SHA-1, respectivamente).  
    4.  Seleccione **reiniciar automáticamente el servidor de destino si es necesario**y, a continuación, haga clic en **implementar**.  
    5.  Espere a que finalice correctamente la implementación  
3.  Agregar servidor de licencias de escritorio remoto:  
    1.  En el administrador del servidor, haga clic en **Remote Desktop Services > información general > + licencias de escritorio remoto**.  
    2.  Seleccione la máquina virtual donde se instalará el servidor de licencias de escritorio remoto (por ejemplo, Contoso Cb1).  
    3.  Haga clic en **siguiente**y, a continuación, haga clic en **agregar**.  
4.  Activar el servidor de licencias de escritorio remoto y agregarlo al grupo de servidores de licencias:  
    1.  En el administrador del servidor, haga clic en **Herramientas > Servicios de Terminal Server > Administrador de licencias de escritorio remoto**.  
    2.  En el Administrador de licencias de escritorio remoto, seleccione el servidor y, a continuación, haga clic en **acción > Activar servidor**.  
    3.  Acepte los valores predeterminados del Asistente para activación de servidor acepte los valores predeterminados hasta llegar a la **información de la empresa** página. A continuación, escriba la información de su empresa.  
    4.  Acepte los valores predeterminados para el resto de páginas hasta que la página final. Borrar **iniciar el Asistente para instalar licencias ahora**y, a continuación, haga clic en **finalizar**.  
    5.  Haga clic en **acción > Revisar configuración > Agregar a grupo > Aceptar**. Escriba las credenciales de un usuario en el grupo de administradores de DC de AAD y registrar como SCP. Este paso podría no funcionar si utiliza Azure AD Domain Services, pero puede pasar por alto las advertencias o errores.  
5.  Agregue el nombre de servidor y el certificado de puerta de enlace de escritorio remoto:  
    1.  En el administrador del servidor, haga clic en **Remote Desktop Services > información general > + puerta de enlace de escritorio remoto**.  
    2.  En el Asistente para agregar servidores de puerta de enlace de escritorio remoto, seleccione la máquina virtual donde desea instalar al servidor de puerta de enlace de escritorio remoto (por ejemplo, Contoso-WebGw1).  
    3.  Escriba el nombre del certificado SSL para el servidor de puerta de enlace de escritorio remoto mediante el externo totalmente DNS nombre completo (FQDN) del servidor de puerta de enlace de escritorio remoto. En Azure, se trata la **nombre DNS** etiquetar y usa el formato servicename.location.cloudapp.azure.com. Por ejemplo, contoso.westus.cloudapp.azure.com.  
    4.  Haga clic en **siguiente**y, a continuación, haga clic en **agregar**.
6.  Cree e instale los certificados autofirmados para los servidores de puerta de enlace de escritorio remoto y el agente de conexión a Escritorio remoto.

       > [!NOTE]
       > Si se proporciona y la instalación de certificados de una entidad de certificación de confianza, realice los procedimientos de h paso a paso k para cada rol. Debe tener el archivo .pfx disponible para cada uno de estos certificados.
       
    1.  En el administrador del servidor, haga clic en **Remote Desktop Services > Introducción > tareas > Editar las propiedades de implementación**.  
    2.  Expanda **certificados**y, a continuación, desplácese a la tabla. Haga clic en **puerta de enlace de escritorio remoto > crear un nuevo certificado**.  
    3.  Escriba el nombre del certificado, use el FQDN externo del servidor de puerta de enlace de escritorio remoto (por ejemplo, contoso.westus.cloudapp.azure.com) y, a continuación, escriba la contraseña.  
    4.  Seleccione **Store este certificado** y, a continuación, vaya a la carpeta compartida que creó para los certificados en el paso anterior. (Por ejemplo, \Contoso Cb1\Certificates.)  
    5.  Escriba un nombre de archivo del certificado (por ejemplo, ContosoRdGwCert) y, a continuación, haga clic en **guardar**.  
    6.  Seleccione **permitir que el certificado que se agregarán al almacén de certificados entidades emisoras de certificados raíz de confianza en los equipos de destino**y, a continuación, haga clic en **Aceptar**.  
    7.  Haga clic en **aplicar**y, a continuación, espere a que el certificado que se apliquen correctamente en el servidor de puerta de enlace de escritorio remoto.  
    8.  Haga clic en **acceso Web de RD > Seleccionar certificado existente**.  
    9.  Busque el certificado creado para el servidor de puerta de enlace de escritorio remoto (por ejemplo, ContosoRdGwCert) y, a continuación, haga clic en **abierto**.  
    10. Escriba la contraseña del certificado, seleccione **permitir que el certificado que se va a agregar al almacén de certificados raíz de confianza en los equipos de destino**y, a continuación, haga clic en **Aceptar**.  
    11. Haga clic en **aplicar**y, a continuación, espere a que el certificado que se aplicó correctamente al servidor de acceso Web de escritorio remoto.  
    12. Repita substeps 1 a 11 para el **RD Connection Broker - habilitar el inicio de sesión único** y **RD Connection Broker - publicación de servicios**, mediante el FQDN interno del servidor de agente de conexión a Escritorio remoto para el nuevo nombre del certificado (por ejemplo, Contoso-Cb1.Contoso.com).  
7.  Exportar certificados públicos autofirmados y copiarlos en un equipo cliente. Si utiliza certificados de una entidad de certificación de confianza, puede omitir este paso.  
    1.  Launch certlm.msc.  
    2.  Expanda **Personal**y, a continuación, haga clic en **certificados**.  
    3.  En el panel derecho, haga clic en el certificado de agente de conexión a Escritorio remoto diseñado para la autenticación de cliente, por ejemplo **Contoso Cb1.Contoso.com**.  
    4.  Haga clic en **todas las tareas > Exportar**.  
    5.  Acepte las opciones predeterminadas en el Asistente para exportar certificados acepten los valores predeterminados hasta llegar a la **archivo para exportar** página.  
    6.  Vaya a la carpeta compartida que creó para los certificados, por ejemplo \Contoso-Cb1\Certificates.  
    7.  Escriba un nombre de archivo, por ejemplo ContosoCbClientCert y, a continuación, haga clic en **guardar**.  
    8.  Haga clic en **Siguiente**y después en **Finalizar**.  
    9.  Repita los pasos 1-8 para el certificado de puerta de enlace de escritorio remoto y Web, (por ejemplo contoso.westus.cloudapp.azure.com), lo que proporciona el certificado exportado un nombre de archivo, por ejemplo **ContosoWebGwClientCert**.  
    10. En el Explorador de archivos, vaya a la carpeta donde se almacenan los certificados, por ejemplo \Contoso-Cb1\Certificates.  
    11. Seleccione los dos certificados de cliente exportado, a continuación, haga clic en ellos y haga clic en **copia**.  
    12. Pegue el dirija en el equipo cliente local.  
8.  Configure las propiedades de implementación de puerta de enlace de escritorio remoto y licencias de escritorio remoto:  
    1.  En el administrador del servidor, haga clic en **Remote Desktop Services > Introducción > tareas > Editar las propiedades de implementación**.  
    2.  Expanda **puerta de enlace de escritorio remoto** y desactive el **servidor de puerta de enlace de escritorio remoto de omisión para direcciones locales** opción.  
    3.  Expanda **licencias de escritorio remoto** y seleccione **por usuario**  
    4.  Haga clic en **Aceptar**.  
10. Crear una colección de sesiones. Estos pasos crea una colección básica. Desproteger [crear una colección de servicios de escritorio remoto para escritorios y aplicaciones se ejecuten](rds-create-collection.md) para obtener más información acerca de las colecciones.
 
    1.  En el administrador del servidor, haga clic en **Remote Desktop Services > colecciones > tareas > Crear colección de sesiones**.  
    2.  Escriba una nombre (por ejemplo, ContosoDesktop) de la colección.  
    3.  Seleccione un servidor Host de sesión de escritorio remoto (Contoso-SHA-1), acepte los grupos de usuarios de forma predeterminada (Contoso\Domain usuarios) y escriba la ruta de acceso de convención de nomenclatura Universal (UNC) a los discos de perfil de usuario que creó anteriormente (\Contoso-Cb1\UserDisks).  
    4.  Establecer un tamaño máximo y, a continuación, haga clic en **crear**.  
  

Ahora ha creado una infraestructura básica de servicios de escritorio remoto. Si necesita crear una implementación de alta disponibilidad, puede agregar un [clúster de agente de conexión](rds-connection-broker-cluster.md) o un [segundo servidor Host de sesión de escritorio remoto](rds-scale-rdsh-farm.md).

