---
title: "Terraform 101: İlk Adımlar"
date: 2023-12-31T09:03:20-08:00
draft: false
tag: ["terraform"]
---

Bir önceki yazıda Terraform kurulumunu da tamamladık. Artık altyapınızı kodla yönetme işlemine hazırsınız demektir. Biz başlangıç tutoriallarımıza AWS servisiyle devam edeceğiz. İhtiyaç duymayana kadar tüm anlatımlarımız AWS üzerine olacaktır. Ancak yapısal benzerlikler çok fazla, o yüzden providerın hangisi olduğuna çok takılmanıza gerek yok. Var da yok ama olsun. İhtiyaç dahilinde Google Cloud, Azure gibi providerlar için de tutoriallar yapabiliriz. Konuyu çok uzatmadan devam edelim.

### Kodlamaya başlamadan önceki ihtiyaçlar

*   Terraform CLI (ki bunu bir önceki yazıda kurduk)
*   AWS CLI (buradan adım adım öğrenebilirsiniz kurulumu, [tıkla](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html))
*   Son olarak AWS erişimi için Access Key ve Secret Key

Terraform CLI ve AWS CLI kurulumu oldukça basit aşamalar. Bunda herhangi bir problem yaşamayacağınıza eminim. AWS hesabınıza CLI erişimi sağlayabilmek için programatik erişimi sağlayan Access ve Secret keyleri sisteminize girmeniz gerekli.

```plaintext
export AWS_ACCESS_KEY_ID=SENİNACCESSKEYİN
```

```plaintext
export AWS_SECRET_ACCESS_KEY=SENİNSCREETKEYİN
```

Burada girişlerinizi tamamladıktan sonrasında AWS hesabınıza artık erişim sağlayabiliyor oldunuz. Kesinlikle **root** hesabının bilgileriyle yapmanızı tavsiye etmiyorum. AWS üzerinden IAM servisinde ben **terraform** diye bir kullanıcı yaratıp, best practice'e uygun yetkilendirmeler ile erişim sağlama taraftarıyım. Bununla ilgili ileride daha gelişmiş bilgiler verme hedefindeyiz.

Anlatımını devam etttirdiğim tutorial süresince free tier olarak ücretsiz AWS resourcelerinden faydalanacağız. Gidip diğer tierları kodunuza yazarsanız ve herhangi bir maaliyet yansırsa hesabınıza bunun sorumluluğu size aittir. Terraform dökümantasyonunda böyle bir detay düşmeleri benimde aynı uyarıyı yapmama sebep oldu açıkçası.

**Devam edecek…**

Kaynak: [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build)