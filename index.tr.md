---
title: Kubernetes Laboratuvarları
lang: tr
slug: index
section: index
order: 0
status: updated
upstream: index.md
---
# Kubernetes Laboratuvarları

Pod'lardan üretim operasyonlarına kadar uygulamalı Kubernetes
laboratuvarları.

> 🇬🇧 [English version](index.md)

## Ön Koşullar

İlk lab'a başlamadan **önce** şunları yapın:

1. **[Ortamınızı kurun](setup/README.md)** — Docker, Git ve bir terminal.
2. **Lab dosyalarını indirin:**

   ```
   git clone https://github.com/yasinoz/kubernetes.git
   cd kubernetes
   ```

3. **Docker Hub'a giriş yapın** (ücretsiz hesap). Sınıf ortamında bu opsiyonel
   değil: Docker Hub anonim imaj çekme işlemlerini *IP adresi başına*
   sınırlıyor; aynı bağlantıyı paylaşan yirmi kişi limiti doldurur ve lab'ın
   ortasında `toomanyrequests` hatası alır.

   ```
   docker login
   ```

4. **Bir Kubernetes kümesi.** En kolayı Docker Desktop içinden
   Kubernetes'i etkinleştirmek (Settings → Kubernetes → Enable).
   Alternatifler: k3d, kind, minikube.

<!-- labs:start -->

## Kubernetes Çekirdeği

Her gün kullandığınız nesneler.

| Lab | Süre | Gerekir |
|---|---|---|
| [Düğümler](labs/nodes/README.md) _(EN)_ | 30 dk | kubernetes |
| [Pod'lar](labs/pods/README.md) _(EN)_ | 45 dk | kubernetes |
| [Service'ler](labs/services/README.md) _(EN)_ | 45 dk | kubernetes |
| [Deployment'lar](labs/deployments/README.md) _(EN)_ | 45 dk | kubernetes |

## Uygulama Modelleme

Yapılandırma, gizli bilgiler ve depolama.

| Lab | Süre | Gerekir |
|---|---|---|
| [ConfigMap'ler](labs/configmaps/README.md) _(EN)_ | 40 dk | kubernetes |
| [Secret'lar](labs/secrets/README.md) _(EN)_ | 40 dk | kubernetes |
| [PersistentVolume'lar](labs/persistentvolumes/README.md) _(EN)_ | 45 dk | kubernetes |
| [Namespace'ler](labs/namespaces/README.md) _(EN)_ | 35 dk | kubernetes |

## İleri Uygulama Modelleme

Erişim denetimi ve Deployment dışındaki iş yükü tipleri.

| Lab | Süre | Gerekir |
|---|---|---|
| [Rol Tabanlı Erişim Denetimi (RBAC)](labs/rbac/README.md) _(EN)_ | 50 dk | kubernetes |
| [DaemonSet'ler](labs/daemonsets/README.md) _(EN)_ | 35 dk | kubernetes |
| [Ingress](labs/ingress/README.md) _(EN)_ | 50 dk | kubernetes |
| [Job ve CronJob'lar](labs/jobs/README.md) _(EN)_ | 40 dk | kubernetes |
| [StatefulSet'ler](labs/statefulsets/README.md) _(EN)_ | 50 dk | kubernetes |

## Kubernetes'i İşletmek

Bir kümeyi gerçek trafiğe hazırlamak.

| Lab | Süre | Gerekir |
|---|---|---|
| [Üretime Hazırlık](labs/productionizing/README.md) _(EN)_ | 50 dk | kubernetes |
| [İzleme](labs/monitoring/README.md) _(EN)_ | 45 dk | kubernetes |
| [Loglama](labs/logging/README.md) _(EN)_ | 45 dk | kubernetes |

## CI/CD

İmaj derlemek ve otomatik dağıtmak.

| Lab | Süre | Gerekir |
|---|---|---|
| [İmaj Optimizasyonu](labs/docker/README.md) _(EN)_ | 40 dk | kubernetes |
| [BuildKit](labs/buildkit/README.md) _(EN)_ | 40 dk | kubernetes |
| [Helm](labs/helm/README.md) _(EN)_ | 50 dk | kubernetes |
| [Rollout'lar](labs/rollouts/README.md) _(EN)_ | 45 dk | kubernetes |
| [Jenkins](labs/jenkins/README.md) _(EN)_ | 55 dk | kubernetes |
| [ArgoCD](labs/argo/README.md) _(EN)_ | 55 dk | kubernetes |

## İleri Kubernetes

Ağ politikası, admission control ve operator'lar.

| Lab | Süre | Gerekir |
|---|---|---|
| [NetworkPolicy](labs/networkpolicy/README.md) _(EN)_ | 45 dk | kubernetes |
| [Admission Control](labs/admission/README.md) _(EN)_ | 55 dk | kubernetes |
| [Operator'lar](labs/operators/README.md) _(EN)_ | 55 dk | kubernetes |

## Üretim Operasyonları

Küme topolojisi, zamanlama ve gerçekten kullandığınız araçlar.

| Lab | Süre | Gerekir |
|---|---|---|
| [Kümeler](labs/clusters/README.md) _(EN)_ | 45 dk | kubernetes |
| [Affinity](labs/affinity/README.md) _(EN)_ | 45 dk | kubernetes |
| [Araçlar](labs/tools/README.md) _(EN)_ | 40 dk | kubernetes |

## Gerçek Kubernetes

Bozuk kümeler ve sebebini nasıl çözersiniz.

| Lab | Süre | Gerekir |
|---|---|---|
| [Sorun Giderme](labs/troubleshooting/README.md) _(EN)_ | 60 dk | kubernetes |
| [Sorun Giderme 2](labs/troubleshooting-2/README.md) _(EN)_ | 60 dk | kubernetes |
| [Sorun Giderme 3](labs/troubleshooting-3/README.md) _(EN)_ | 60 dk | kubernetes |

<!-- labs:end -->

## Bir lab nasıl işler

Her lab aynı kalıbı izler:

- **Kaynaklar** — gerçek dökümantasyona bağlantılar.
- **Anlatım** — adım adım yönlendirme, komutlar kopyala-yapıştır çalışır.
- 📋 **Görevler** — bunları kendiniz çözeceksiniz; cevap hemen altındaki
  `Takıldınız mı?` bloğunda gizli.
- **Lab** — anlatımı olmayan, daha zor bir alıştırma. `hints` yol gösterir,
  `solution` cevabı verir.
- **Temizlik** — mutlaka çalıştırın; yoksa sonraki lab aynı portlar için
  kavga eder.

> Henüz Türkçeye çevrilmemiş lablar listede _(EN)_ işaretiyle görünür ve
> İngilizce sürümüne bağlanır. Çeviri durumu dosya varlığından okunur.

## Durum

Lablar 2021-2025 arası bir kaynaktan modernize edildi. Mekanik düzeltmeler
(compose v2, güncel imaj sürümleri, kırık bağlantılar) uygulandı; lablar
henüz uçtan uca çalıştırılarak doğrulanmadı.

Bozuk bir şey bulursanız issue açın — derste patlayan bir lab, hiç olmayan
lab'dan kötüdür.

## Emeği Geçenler

[Elton Stoneman](https://github.com/sixeyed) tarafından hazırlanan
[courselabs/kubernetes](https://github.com/courselabs/kubernetes) çalışmasından
türetilmiştir; CC BY-SA 4.0 kapsamında kullanılmış ve yeniden dağıtılmıştır.
Ayrıntı: [ATTRIBUTION.md](ATTRIBUTION.md).

Türkçe çeviri, modernizasyon ve lab altyapısı:
[Mehmet Yasin Özkasapoğlu](https://github.com/yasinoz) — `sudo2go`.
