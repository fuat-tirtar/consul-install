Consul Kurulumu ve Eski Consul'dan Yeni Consul'a Migration
Bu doküman, Kubernetes üzerinde yeni bir HashiCorp Consul cluster'ının kurulması ve eski Consul ortamındaki KV (Key-Value) verilerinin yeni Consul ortamına aktarılması için gerekli adımları içerir.

Migration süreci temel olarak şu aşamalardan oluşmaktadır:

*Local Path Provisioner kurulumu																																																																																													
*Yeni Consul ortamının kurulması
*Consul ACL yapılandırmasının yapılması
*Eski Consul KV verilerinin export edilmesi
*Backup dosyasının local makineye alınması
*Backup dosyasının kontrol edilmesi
*Yeni Consul ortamına KV verilerinin import edilmesi
*Aktarılan verilerin doğrulanması
*Gerekli ACL token'larının oluşturulması

Gereksinimler
Migration işlemi öncesinde aşağıdaki araçların kurulu olması gerekir:

*Kubernetes / kubectl 
*Helm
*Consul CLI
*Kubernetes cluster erişimi
*Lens (opsiyonel)
