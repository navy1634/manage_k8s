# Registry

Registry は Helmfile から `charts/registry` を適用して管理する。

## 永続化

dev では 1 台構成を前提にしているため、chart で静的 PV を作成する。PV は `hostPath: /var/lib/registry` を使い、`values/registry-dev.yml` の `persistentVolume.nodeName` で対象ノードに固定する。

prd では chart で PV を作成しない。PVC だけを作成し、実際の PV はクラスタの StorageClass による動的プロビジョニングに任せる。複数台構成で hostPath の静的 PV を使うと、Pod が別ノードへ移動したときに registry のデータを参照できなくなるため。

prd では default StorageClass が存在するか、`values/registry-prd.yml` で `persistence.storageClassName` を明示する必要がある。
