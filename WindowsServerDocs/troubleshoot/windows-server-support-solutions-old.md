---
title: Soluciones principales de soporte técnico para Windows Server
description: Obtener vínculos a soluciones para problemas de Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 1eb52f28fcd5afe62df33cd56208f2ab506e7194
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453149"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Soluciones principales de soporte técnico para Windows Server 2016

Microsoft publica periódicamente actualizaciones y soluciones para Windows Server. Para garantizar que los servidores pueden recibir actualizaciones futuras, incluidas las actualizaciones de seguridad, es importante mantenerlos actualizados. Echa un vistazo a [Historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) para obtener una lista completa de las actualizaciones publicadas.

Estas son las mejores soluciones del Soporte técnico de Microsoft para la mayoría de problemas comunes experimentados con Windows Server 2016. Los siguientes vínculos incluyen vínculos a artículos KB, actualizaciones y artículos de la biblioteca.

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluciones para la instalación o actualización de Windows Server

- [Resolver errores de actualización de Windows 10: Información técnica para profesionales de TI](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Servicio de actualización de la pila para Windows 10 versión 1607 y Windows Server 2016: 8 de agosto de 2017](https://support.microsoft.com/en-US/help/4035631)
- [Actualización de compatibilidad para la actualización a Windows 10 versión 1607 y Windows Server 2016: 3 de agosto de 2017](https://support.microsoft.com/en-US/help/4033524)
- [No se admite una actualización del sistema local en máquinas virtuales de Azure basado en Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opciones de actualización y conversión para Windows Server 2016](../get-started/supported-upgrade-paths.md)
- [Matriz de actualización y migración del rol de servidor para Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Actualización e instalación de Windows Server](../get-started/installation-and-upgrade.md)
- [Notas de la versión: Problemas importantes en Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Recomendaciones para migrar a Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluciones de activación por volumen
- [Activación de Windows Server 2016](../get-started/server-2016-activation.md)
- [Revisar y seleccionar métodos de activación](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Códigos de Error de activación para activación de volumen](https://technet.microsoft.com/library/dn502528.aspx)
- [Cómo solucionar problemas del servicio de administración de claves (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Solución de problemas de activación de volumen](https://technet.microsoft.com/library/ff793439.aspx)
- [Códigos de Error de activación](https://technet.microsoft.com/library/ff793399.aspx)
- [Instalación de Windows puede producir el error "la clave de producto especificada no coincide con cualquiera de las imágenes de Windows disponibles para la instalación. Escriba una clave de producto diferente"](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluciones relacionados con DCPromo y la instalación de controladores de dominio
- [Active Directory y los requisitos de puerto de servicios de dominio Active Directory](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Puertos de Firewall de Active Directory: vamos a intentar realizar esta sencilla](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Soporte técnico de Exchange Server para Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Utilizar Ntdsutil.exe para transferir o asumir los roles FSMO a un controlador de dominio](https://support.microsoft.com/kb/255504)
- [Solución de problemas de implementación de controladores de dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Solucionar problemas de Asistente para instalación de Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemas conocidos de instalación y desinstalación de AD DS](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluciones de Servicios de federación de Active Directory (AD FS)
- [Cómo configurar el registro automático de dispositivos Unidos a un dominio de Windows con Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configuración de la emisión de notificaciones](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Protegerse de ataques de contraseña](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Actualización a AD FS en Windows Server 2016 mediante una base de datos WID](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Inicio de sesión de Windows 10 en: habilitar la autenticación de dispositivos con AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Administración de certificados SSL en AD FS y WAP en Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Directivas de Control de acceso en Windows Server 2016 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluciones relacionadas con la replicación de Active Directory

- [Solución de problemas de replicación de Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Descargue la herramienta de estado de replicación de Active Directory desde el centro de descarga de Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e: Cómo solucionar errores comunes de replicación de Active Directory](https://support.microsoft.com/kb/3108513)
- [Solución de problemas de errores de replicación de AD 8606: Se especificaron atributos suficientes para crear un objeto](https://support.microsoft.com/kb/2028495)
- [Id. de evento 2108 y 1084 se producen durante una replicación entrante de Active Directory en Windows 2000 Server y en Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Solución de problemas de errores de replicación de AD 8451: La operación de replicación encontró un error de base de datos](https://support.microsoft.com/kb/2645996)
- [Solución de problemas de errores de replicación de AD 1127: Acceso al disco duro, error en una operación de disco incluso tras varios intentos](https://support.microsoft.com/kb/2025726)
- [Limpiar los metadatos del servidor](https://technet.microsoft.com/library/cc816907.aspx)