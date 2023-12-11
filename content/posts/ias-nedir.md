---
title: "Infrastructure as Code (IaC) & Terraform Nedir?"
date: 2023-12-11T09:03:20-08:00
draft: false
---

Altyapı Kodlama (IaC) araçları, altyapıyı grafiksel bir kullanıcı arayüzü yerine yapılandırma dosyaları aracılığıyla yönetmenizi sağlar. IaC, kaynak konfigürasyonlarını tanımlayarak altyapıyı güvenli, tutarlı ve tekrarlanabilir bir şekilde oluşturmanıza, değiştirmenize ve yönetmenize olanak tanır; bu konfigürasyonları sürümleyebilir, yeniden kullanabilir ve paylaşabilirsiniz.

Terraform, HashiCorp'un altyapı kodlama aracıdır. İnsan tarafından okunabilir, bildirgesel yapılandırma dosyalarında kaynakları ve altyapıyı tanımlamanıza olanak tanır ve altyapınızın yaşam döngüsünü yönetir. Terraform kullanmanın manuel olarak altyapınızı yönetmeye göre birkaç avantajı vardır:

*   Çoklu cloud altyapısını yönetmenize olanak sağlar.
*   Kolay okunabilir syntax'ı sayesinde hızlı şekilde altyapınızı kodlamanızı sağlar.
*   Terraform **state** özelliği sayesinde her güncellemeyle kaynak değişikliklerini takip etmenize olanak tanır.
*   Mimarinizi (infrastructure) hazırladığınızı kodları git (versiyon kontrol) ortamında tutarak güvene alabilirsiniz. 

Terraform güncelde 1000 üzerinden servis ile API'ler aracılığıyla iletişim kurabilmektedir. En popüler olarak bilinen servisler Amazon Web Services (AWS), Azure, Google Cloud Platform (GCP), Kubernetes, Helm, GitHub, Splunk ve DataDog gibi herkes tarafından bilinen cloud based servislerdir. İhtiyacınız olan servisi belirle ve kodunu yaz ki kurguladığın mimari çok daha verimli olsun.

## **Yayına alma sürecini standart hale getir**

Bir DevOps veya Çözüm Mimarı (Solution Architect) olarak birden fazla sağlayıcı üzerinde çalışma ihtiyacınız doğabilir. Bu gibi durumlarda manuel olarak süreçleri yönetmek zor hale gelecektir. Terraform veya diğer IaC (Infrastructure as Code) araçları bu durumlarda hayatınızı kurtaracak. Bu yazıda Terraform'u anlatma sebebim ise sağladığı 1000 üzerindeki kaynağa erişim sayesinde multi cloud veya multi servis işlemler için çok iyi durumda. Terraform sağlayıcıları, kaynaklar arasındaki bağımlılıkları otomatik olarak hesaplayarak bunları doğru sırayla oluşturup yok eder.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/8830cd3d2e0eeefd02482f58e2afe5e34ac1c2e42b268095.png)

Terraform mimari planlaması standartı şu şekilde:

*   **Scope** - Projeniz için altyapıyı belirleyin.
*   **Author** - Altyapınız için kodunuzu yazın.
*   **Initialize** - Terraformun altyapıyı yönetmek için gerekli eklentileri yükleyin.
*   **Plan** - Terraform'ın yapılandırmanıza uygun olarak yapacağı değişiklikleri önceden görüntüleyin.
*   **Apply** - İşlemleri gönderin, yayınlayın.

## Özet

Infrastructure as Code nediri anlatmaya çalışırken bunu Terraform üzerinden yapmam biraz reklamvari gözükmüş olabilir. Ancak burada size kendi kullandığım araçlar üzerinden anlatım yapacağım. Yani mevcut belirlediğim standartlar üzerinden bildiklerimi veya öğrendiklerimi aşama aşama sizlerle paylaşacağım. Bu öğrenme ve paylaşma sürecinde yanımda olursanız memnun olurum.