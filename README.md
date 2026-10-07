Consul Kurulumu ve Eski Consul'dan Yeni Consul'a Migration
Bu doküman, Kubernetes üzerinde yeni bir HashiCorp Consul cluster'ının kurulması ve eski Consul ortamındaki KV (Key-Value) verilerinin yeni Consul ortamına aktarılması için gerekli adımları içerir.

Migration süreci temel olarak şu aşamalardan oluşmaktadır:

1-Local Path Provisioner kurulumu																																																																																																																					
2-Yeni Consul ortamının kurulması
3-Consul ACL yapılandırmasının yapılması
4-Eski Consul KV verilerinin export edilmesi
5-Backup dosyasının local makineye alınması
6-Backup dosyasının kontrol edilmesi
7-Yeni Consul ortamına KV verilerinin import edilmesi
8-Aktarılan verilerin doğrulanması
9-Gerekli ACL token'larının oluşturulması
