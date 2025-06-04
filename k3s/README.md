# K3s Kubernetes Cluster Deployment (статью нужно проверять и обновлять)

K3s — это полностью совместимый с Kubernetes облегчённый дистрибутив.
- Официальная документация: [https://k3s.io/](https://k3s.io/)

---

## Настройка Firewall

Для корректной работы кластера откройте необходимые порты:

```bash
firewall-cmd --permanent --zone=public --add-port=6443/tcp
firewall-cmd --permanent --zone=public --add-port=8472/udp
firewall-cmd --permanent --zone=public --add-port=51820/tcp
firewall-cmd --permanent --zone=public --add-port=51821/tcp
firewall-cmd --permanent --zone=public --add-port=10250/tcp
firewall-cmd --permanent --zone=public --add-port=2379-2380/tcp
firewall-cmd --complete-reload
Установка K3s
Установка первого мастера (инициализация кластера)
curl -sfL https://get.k3s.io | K3S_TOKEN='l%TH]c4VvCT<Xj{' sh -s - server --cluster-init
С дополнительными параметрами (например, для настройки сети и путей):
curl -sfL https://get.k3s.io | K3S_TOKEN='l%TH]c4VvCT<Xj{' sh -s - server --cluster-init \
     --flannel-iface "ens33" \
     --cluster-cidr "10.223.0.0/18"  --service-cidr "10.224.0.0/18" --cluster-dns "10.223.0.10" \
     --default-local-storage-path "/var/k3s/storage" \
     --data-dir "/var/k3s/data"
Проверка нод:

kubectl get nodes
Установка дополнительных мастеров
Общее количество мастеров должно быть нечётным.

curl -sfL https://get.k3s.io | K3S_TOKEN='l%TH]c4VvCT<Xj{' sh -s - server --server https://адрес.local:6443
С дополнительными параметрами:

curl -sfL https://get.k3s.io | K3S_TOKEN='l%TH]c4VvCT<Xj{' sh -s - server --server https://адрес.local:6443 \
     --flannel-iface "ens33" \
     --cluster-cidr "10.223.0.0/18"  --service-cidr "10.224.0.0/18" --cluster-dns "10.223.0.10" \
     --default-local-storage-path "/var/k3s/storage" \
     --data-dir "/var/k3s/data"
Установка Worker нод
На мастер-ноде получаем токен:
cat /var/lib/rancher/k3s/server/token
# или, если используется другой путь
cat /var/k3s/data/server/token
Используем токен для установки на воркер-ноде:
curl -sfL https://get.k3s.io | K3S_URL="https://адрес.local:6443" \
      K3S_TOKEN='ВАШ_ТОКЕН_ЗДЕСЬ' \
      sh -
Если нужны дополнительные параметры, задайте их через переменную INSTALL_K3S_EXEC:
curl -sfL https://get.k3s.io | K3S_URL="https://адрес.local:6443" \
      K3S_TOKEN='ВАШ_ТОКЕН_ЗДЕСЬ' \
      INSTALL_K3S_EXEC="--flannel-iface ens33 --data-dir /var/k3s/data" \
      sh -
Полезные ссылки
Официальный сайт: https://k3s.io/
Документация: https://rancher.com/docs/k3s/latest/en/
