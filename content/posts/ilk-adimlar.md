---

title: "Terraform 101: İlk Adımlar"  
date: 2023-12-31T09:03:20-08:00  
draft: false  
tag: \["terraform"\]

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

Anlatımını devam ettirdiğim tutorial süresince free tier olarak ücretsiz AWS resourcelerinden faydalanacağız. Gidip diğer tierları kodunuza yazarsanız ve herhangi bir maaliyet yansırsa hesabınıza bunun sorumluluğu size aittir. Terraform dökümantasyonunda böyle bir detay düşmeleri benimde aynı uyarıyı yapmama sebep oldu açıkçası.

#### Yapılandırmaları Yapmaya Başlayalım

Terraform'da altyapıyı tanımlamak için kullanılan proje dosyaları Terraform yapılandırması olarak bilinir. İlk yapılandırmanızı tek bir AWS EC2 sanal sunucusunu ayağa kaldırmak için kodlayacağız. Her Terraform yapılandırması kendi proje dizininde olmalıdır. Şimdi terminalinizi açın Windows için PowerShell, Unix based makineleriniz için düz terminal açın.

```plaintext
mkdir terraform-101-ilk-adimlar
```

Bu klasöre gidelim şimdi:

```plaintext
cd terraform-101-ilk-adimlar
```

Altyapımızın temel ayarlamalarını yapacağımız terraform dosyasını oluşturalım. Terraform dosya formatı TF uzantısına sahiptir.

```plaintext
touch main.tf
```

Sizde benim gibi bir Visual Studio Code kullanıcısı iseniz terminale “code .” yazdığınız taktirde proje klasörünü kod editörünüzde kolayca açabilirsiniz. Açılan IDE'de main.tf dosyasını düzenleyeceğiz.

```plaintext
############################################################
# ilk olarak terraforma hangi providerı kullanacağımızı ve #
# versiyon ayarlamalarımızı söylüyoruz, terraform blocku   #
# olarak tanımlanan bu kısımda terraform ile ilgili tüm    #
# ayarlamalarımızı giriyoruz.                              #
############################################################

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

############################################################
# aws'deki hangi regionı kullanacağımızı tanımlıyoruz      #
############################################################

provider "aws" {
  region  = "us-west-2"
}

############################################################
# resource aws'de hangi servisi kullanacağımızı göster     #
# aws_instance yani ilk kısım aws'de ki servisin ismi      #
# app_server olarak belirtilen ise servise verdiğimiz isim #
# instance_type ise sanal makinenin özelliklerini          #
# ami ec2 ami türünü belirtiyor                            #
############################################################

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "İlkAWSInstanceİsmimiz"
  }
}
```

Dosya içeriğine bu kodlarımızı yazdıktan sonrasında proje klasörüne terraformu dahil etmemiz gerekiyor. Node.js vs yazanların bildiği npm init mantığıyla node dosyalarını içeri çekmek gibi düşünebilirsiniz.

```plaintext
terraform init
```

Terraform bu komut sonrasında AWS provider'ıyla ilgili tüm gereklilikleri projenize dahil edecek. Projenizde versiyon kontrol sistemi kullandığınızda nasıl gizli bir **.git** klasörü oluşturuyor ise aynı şekilde gizli bir **.terraform** klasörü oluşturacak.

Bu süreçte iki komuttan bahsetmek istiyorum satır satır çok uzatmadan. Bu iki müthiş komut tüm geliştirme süreçlerinizde kesinlikle baş ucunuzdan ayırmamanız gerekiyor. 

```plaintext
terraform fmt         # yazdığınız terraform kodlarını terraform standartlarına göre düzgün bir formata sokar
```

```plaintext
terraform validate    # yazdığınız terraform kodlarının syntaxa uygun olup olmadığını kontrol eder 
                      # eğer kodunuzda bir problem yok ise aşağıdaki çıktıyı verir
                      # Success! The configuration is valid.
```

Evet yazdığımız kodu formatladık, sonrasında kontrol ettik ve komutumuz hazır olduğu taktirde göndermeye hazırız demektir.

```plaintext
terraform apply      # Kodlarım hazır hadi providerda bunları çalıştıralım demek
```

Bu komut sonrasında AWS ile makinemiz konuşmaya başlayacak demektir.

Kaynak: [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build)