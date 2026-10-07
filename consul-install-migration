Yeni Consul Kurulumu-Eski Consul Migraiton Adımları
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.26/deploy/local-path-storage.yaml
values.yaml 

global:
  name: consul
  datacenter: dc1
  acls:
    manageSystemACLs: true

server:
  enabled: true
  replicas: 2
  bootstrapExpect: 2
  storage: 5Gi
  storageClass: local-path
  resources:
    requests:
      memory: "256Mi"
      cpu: "100m"
    limits:
      memory: "512Mi"
      cpu: "500m"

client:
  enabled: true
  grpc: true
  exposeGossipPorts: true
  hostPort:
    http: 8500
    grpc: 8502
  resources:
    requests:
      cpu: "100m"
      memory: "100Mi"
    limits:
      cpu: "100m"
      memory: "100Mi"

ui:
  enabled: true
  service:
    type: ClusterIP

connectInject:
  enabled: false

----------------------------------------------------------------------------
Çalıştırılan komutlar:
kubectl create namespace consul
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update
helm search repo hashicorp/consul --versions   ##En son latest versionu bul

##kurulum     --> helm install consul hashicorp/consul   --version 2.0.2   -f values.yaml   -n consul   --skip-crds 
##token şifre --> kubectl get secret consul-bootstrap-acl-token -n consul -o jsonpath='{.data.token}' | base64 --decode; echo

##Bu şifreyi değiştirmek için de --> kubectl exec -it consul-server-0 -n consul -- consul acl token create -policy-name="global-management" -secret="İSTEDİĞİN KEYİ YAZ" -token="DEFAULT KEY"

-------------------------------------------------------------------------------
Eski Consul ortamındaki tüm Key-Value (KV) verilerinin kesintisiz bir şekilde yedeklenmesi (export), doğruluğunun kontrol edilmesi, 
ACL koruması aktif olan Yeni Consul'a aktarılması (import) ve yeni Consul ortamı için yetkili özel erişim token'larının tanımlanması adımları;

** Lensden işlem yapıyoruz. İlk olarak eski consula terminalden bağlanıyoruz.

# 1. Eski Consul podu içinde verileri /tmp/backup.json dosyasına export etme
kubectl exec -it consul-server-0 -n consul -- sh -c "consul kv export -token='defaultkeysifremiz' > /tmp/backup.json"

# 2. Oluşturulan yedek dosyasını local makineye (PowerShell) kopyalama
kubectl cp "consul-server-0:/tmp/backup.json" ".\consul-backup.json" -n consul

# 3. Eski pod içindeki geçici dosyayı temizleme
kubectl exec -it consul-server-0 -n consul -- rm /tmp/backup.json

Kontrol edilir, consuldaki verinin boyutu
# 1. Dosya boyutunu kontrol etme (örn: ~173 KB başarıyla çekildi)
Get-Item .\consul_kv_backup.json
# 2. Dosya içeriğinin ilk satırlarını doğrulama
Get-Content .\consul_kv_backup.json -Head 15

-------------------------------------------------------------------------------
4. Yeni Consul Ortamına Aktarım İşlemleri (Import)
İndirdiğimiz consul_kv_backup.json dosyası, yeni Consul kümesinin aktif bootstrap token'ı kullanılarak aktarılmış ve veriler kontrol edilmiştir.

# 1. Yedeği yeni Consul ortama yükleme (Import)
Get-Content .\consul_kv_backup.json -Raw | kubectl exec -i consul-server-0 -n consul -- consul kv import -token="yeniconsuldefaultkeyi" -

# 2. Aktarılan Key-Value verilerini yeni ortamda doğrulama
kubectl exec -it consul-server-0 -n consul -- consul kv get -token="yeniconsuldefaultkeyi" -recurse
