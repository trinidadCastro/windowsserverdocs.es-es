Busque los certificados de la protección HGS. Necesitará un certificado de firma y un certificado de cifrado para inicializar el clúster HGS.
La manera más fácil para proporcionar certificados a HGS es crear un archivo PFX protegido con contraseña para cada certificado que contiene las claves públicas y privadas. Si usa claves respaldadas con HSM u otros certificados que no es exportable, asegúrese de que el certificado está instalado en el almacén de certificados del equipo local antes de continuar.
Para obtener más información acerca de qué certificados utilizar, consulte [obtener certificados para HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs).

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->