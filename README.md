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
