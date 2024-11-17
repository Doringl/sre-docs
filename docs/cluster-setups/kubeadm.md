# Kubeadm Cluster Kurulumu

<div style="display: flex; align-items: center; flex-wrap: wrap; justify-content: center">
  <div style="margin-right: 20px;">
    <img src="/sre-docs/_media/kubeadm-logo.svg" alt="kubeadm-logo" style="max-width: 200px;" width="200" height="200" />
  </div>
  <div style="flex: 1; text-align: left;">
    <blockquote>
      Kubeadm, Kubernetes Cluster'ın hızlı ve kolay bir şekilde kurulmasını, yapılandırılmasını ve birleştirilmesini sağlayan bir komut satırı aracıdır. Kubernetes bileşenlerini manuel olarak kurmaya kıyasla, Kubeadm, gerekli adımları otomatikleştirerek kurulum sürecini oldukça kolaylaştırır.
    </blockquote>
  </div>
</div>

## **Sunucu Hakkında Bilgi Edinme**

- Sunucu işletim sistemini öğrenme:

  ```bash
  lsb_release -a
  ```

- Sunucu kaynaklarının kontrol edilmesi (CPU, RAM):

  ```bash
  htop
  ```

- Sunucu depolama alanının kontrol edilmesi:

  ```bash
  df -h
  ```

## **k8s_installer**

```bash
https://git.detaysoft.com/sre/k8s/k8s-installer
```

<!-- ## **k8s_installer İndirme Linkleri**

- Ubuntu 20.04:

  ```bash
  curl --header "PRIVATE-TOKEN: token" "https://git.detaysoft.com/api/v4/projects/1741/repository/files/k8s_installer_20_04_latest/raw?ref=main" -o k8s_installer_20_04_latest
  ```

- Ubuntu 22.04:

  ```bash
  curl --header "PRIVATE-TOKEN: token" "https://git.detaysoft.com/api/v4/projects/1741/repository/files/k8s_installer_22_04_latest/raw?ref=main" -o k8s_installer_22_04_latest
  ```

- Ubuntu 24.04:

  ```bash
  curl --header "PRIVATE-TOKEN: token" "https://git.detaysoft.com/api/v4/projects/1741/repository/files/k8s_installer_24_04_latest/raw?ref=main" -o k8s_installer_24_04_latest
  ```

- **Dosyaya çalıştırma izni verme:**

  ```bash
  sudo chmod +x k8s_installer_<UBUNTU_VERSION>_latest
  ``` -->

## **Scripti Çalıştırma ve Cluster Ayarlamaları**

```bash
sudo ./k8s_installer_<UBUNTU_VERSION>_latest
```

```bash
 _    ___          _           _        _ _
| | _( _ ) ___    (_)_ __  ___| |_ __ _| | | ___ _ __
| |/ / _ \/ __|   | | '_ \/ __| __/ _` | | |/ _ \ '__|
|   < (_) \__ \   | | | | \__ \ || (_| | | |  __/ |
|_|\_\___/|___/___|_|_| |_|___/\__\__,_|_|_|\___|_|
            |_____|
v2.0

> Master Setup
  Worker Setup
  Exit
```

- Bu aşamada Master Node kurulacaksa:

  > **Not:** **--pod-network-cidr 192.168.0.0/16** ve **--service-cidr 172.24.0.0/16** varsayılan olarak kullanılabilir, **sunucu IP bilgisine göre çakışıklık olmaması için değiştirilebilir**.

- Worker Node kurulumunda kullanılmak üzere `kubeadm join --` komutunun alınabilmesi için Master Node'da aşağıdaki komut kullanılabilir;

  ```bash
  cat k8s_installer.log
  ```

  > **Not:** k8s_installer.log script çalıştıktan sonra oluşmaktadır.

<details>
  <summary><b>:warning: Master Node'da CNI hatası alınması durumunda</b></summary>
  <ul>
    <li>
     <b>Calico ayar dosyası açılır ve mtu değeri 1440 yapılıp kaydedilir</b>.
    </li>
  </ul>

```bash
sudo vim /etc/cni/net.d/10-calico.conflist
```

  <ul>
    <li>
     <b>Değişikliklerin yansıması için containerd yeniden başlatılır</b>.
    </li>
  </ul>

```bash
sudo systemctl restart containerd
```

</details>

- **Single Node cluster** ise aşağıdaki komutu ile **Master Node**'dan **NoSchedule** tainti kaldırılır.

```bash
kubectl taint nodes node1 key1=value1:NoSchedule-
```
