---
title: "GitOps 101: GitOps Nedir? Minikube Üzerinde ArgoCD Kurulumu"
date: 2024-04-22T09:03:20-08:00
draft: false
tag: ["gitops, argocd"]
---

Bugün GitOps ve ArgoCD hakkında konuşacağız. Konuşmaktan ziyade yaptığım pratikleri sizlere aktarmaya çalışacağım. Yapacağımız işlem ArgoCD kurulumunu Minikube üzerinde çalıştırmak. İlerleyen yazılarda örnek projelerle de ArgoCD üzerinden Kubernetes projelerini yönetme üzerine bir kaç çalışmayı aktarmak istiyorum. Bakalım fırsat olursa onları da deneyeceğiz.

## GitOps nedir?

GitOps, sürekli entegrasyon/sürekli dağıtım (CI/CD) ve sürüm kontrolü gibi DevOps uygulamalarına dayanan bir işletim çerçevesidir. Altyapıyı otomatikleştirir ve yazılım dağıtımını yönetir. Geliştiricilere altyapının istenen durumunu depolama ve bunu kullanarak işletme süreçlerini otomatikleştirme imkanı sağlar. GitOps, geliştirme iş akışının başından itibaren uygulanan bir dizi en iyi uygulamadır ve dağıtıma kadar uzanır.

Okuduğunuz paragraf ChatGPT çevirisidir asıl kaynağı [https://codefresh.io/learn/gitops/](https://codefresh.io/learn/gitops/) ve detayları burada okuyabilirsiniz. Konu hakkında daha fazla bilgiyi alternatif kaynaklardan okuyabilirsiniz. GitOps için en yaygın açık kaynak yazılım olarak ArgoCD kullanılmaktadır. Bizde burada bugün Minikube üzerinde bunun kurulumunu yapacağız.

### ArgoCD nedir? 

Argo CD, Kubernetes için deklaratif, GitOps sürekli dağıtım aracıdır. Uygulama tanımları, yapılandırmaları ve ortamlar deklaratif olmalı ve sürüm kontrolü altında olmalıdır. Uygulama dağıtımı ve yaşam döngüsü yönetimi otomatik, denetlenebilir ve anlaşılması kolay olmalıdır. Özetle ArgoCD'nin amacı Kubernetes projelerindeki süreçleri Git ortamları sayesinde kontrol altında tutmaktadır. Güncelleme, oluşturma gibi süreçleri hem GUI sayesinde rahat takibi hemde müthiş bir senkronizasyon ile verimli bir iş yönetimi haline getiriyor.

### ArgoCD Kurulumu İçin Gereklilikler

Bugün biz lokal ortamlarda çalışacağız, bunun için 3 temel ihtiyacımız var:

1.  Kubectl
2.  Docker
3.  Minikube

Mac cihazlara sahip kişiler homebrew ile bunların hepsinin kurulumunu kolayca yapabilirler.

```plaintext
brew install docker
```

Bu kurulumları tamamladıktan sonra Docker üzerinde bir işlem yapmanız gerekiyor. Docker Desktop > Settings > Kubernetes kısmına geliyoruz. Ve oradan **Enable Kubernetes** seçeneğini seçiyoruz. Tercihinize göre **Show system containers** seçeneğini de seçebilirsiniz. Bunu seçerseniz eğer kubernetes ile birlikte oluşan tüm containerları **Docker Desktop Containers** kısmında listelendiğini göreceksiniz.

```plaintext
brew install kubectl
```

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/978e39ef0d85c9df15b53947b8e61585d73a09ad7ab5dc53.png)

Hemen ardından hızlı şekilde Kubernetes projelerinizi CLI üzerinden yönetmenizi sağlayacağınız **kubectl** aracını ve projeleri dockerda rahatça çaışabileceğiniz **minikube** ortamını kuruyoruz.

```plaintext
brew install minikube
```

#### Ortam Kontrolleri

Bu kurulumları tamamladıktan sonra Kubernetes pratikleri yapabileceğiniz tüm her şeyi sağlamış oldunuz. Şimdi sırayla kontrollerimizi yapalım.

```plaintext
docker --version
minikube version
kubectl version
```

Sırayla kurduğumuz bu araçların sorunsuzca yüklenip yüklenmediğini kontrol ediyoruz.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/e705a924432e279d3696f1aa3c834ae28254343e4e8ac5e8.png)

Hemen ardından minikube ayağa kaldırıyoruz, komutumuz: **minikube start**

Done yazısını gördüğümüzde sorunsuz ayağa kalktığını görüyoruz.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/5580214ea4f1bbdd89c97e3fc37a18d3435fc066993486ed.png)

ArgoCD için Namespace oluşturuyoruz:

```plaintext
kubectl create namespace argocd
```

namespace/argocd created olarak size bir çıktı verecektir. Sonrasında Argo CD'nin en güncel versiyonunu [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/) dökümantasyon adresinden de ulaşabileceğiniz şekilde kurulumunu yapıyoruz.

```plaintext
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Göreceğiniz üzere **kubectl** komutunu kullan -n parametresi ile ismi **argocd** olsun diyoruz ve son olarak **yaml** dosyasının yolunu belirtiyoruz. Bu yaml dosyasını isterseniz local ortamınıza çekip gerekli dizini belirterek kendi bilgisayarınız üzerinden de gösterebilirsiniz. GitOps'un temel taşlarını atıyoruz bu şekilde. Bu komutu çalıştırdığımızda bir yığın dolusu kubernetes gereklilikleri oluşturacak. Kubernetes için kullanılan serviceaccount, configmap, service, deployment gibi bir çok çıktı göreceksiniz. Hızlı bir yanıt alacaksınız zaten. Aşağıdaki komut ile çalışan bir kubernetes node

```plaintext
kubectl get all -n argocd
```

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/3cdcc6c99a41f27371be0dad0038194ed02f73f3f545235d.png)

Bu şekilde **argocd namespace'inin** ayakta olduğunu görebilirsiniz. Artık erişmek için sadece 443 portuna erişim sağlayacağız. Üç farklı yöntem var bunun için isterseniz [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/) dökümantasyon üzerinden kontrol edebilirsiniz. Load Balancer, Ingress ve Port Forwarding, biz lokal ortamda olduğumuz için hızlı şekilde port yönlendirmesiyle ilerleyeceğiz. Ama öncesinde arayüze erişmek için **Argo CD yönetici şifresi** alma işlemini yapalım.

```plaintext
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Unix based cihazlarda çalışmanın avantajları, base64 hazır kurulu olduğu için direkt olarak şifreyi decode edebiliyoruz. Windows cihazlarda sorunla karşılaşabilirsiniz belki, denemediğim için bilmiyorum. Mac'iniz yoksa ve bu işlerle ilgiliyseniz en azından Ubuntu bilgisayar kullanın, Windows kullanmayın.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/91972a77b8b33a0e6eda532a3ad881e313e00e490aef44ec.png)

Görüldüğü üzere ekrana hemen şifreyi bastı, Argo CD'de kullanıcı adı otomatik olarak **admin** olarak gelmekte ve default bir şifre vermekte. Best practice olması açısından şifreyi silip yeniden bir şifre oluşturmalısınız. Bunu belki ek bir yazı olarak hazırlayabilirim. Ancak onun için **argocd** **cli** kurulu olması gerekli bilgisayarınızda. Konuyu uzatmamak için dahil etmeyelim bu sürece.

Evet şifremizi de altık sırada kubectl'in port-forward işlemi kaldı.

```plaintext
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/263baae09fb8e443c8dbcaabec9b7dd6dd4109d66bc75cdb.png)

Sorunsuz şekilde yönlendirme işlemimiz tamamlandı [https://localhost:8080/](https://localhost:8080/) adresine giderek panel arayüzüne erişebilirsiniz. SSL ile ilgili bir problemle karşılaşabilirsiniz ama güven dedikten sonra sayfaya ilerleyin. Bununla ilgili de çözüm var ancak şifre sıfırlama ve SSL problemini ayrı bir yazıda aktaracağım daha fazla uzamasın diye.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/9188f75a58aa5b46fe375fa70c9a8b81cca4c3f239a1a967.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/99d17ebe18b9bd45648c504ba2fed4ddff244eeb37c040bb.png)