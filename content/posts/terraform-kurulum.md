---
title: "Terraform 101: Kurulum"
date: 2023-12-14T09:03:20-08:00
draft: false
tag: ["terraform"]
---

Bir önceki yazımızda [IaC nedir](https://hunk.ar/posts/iac-nedir/), ne değildir, Terraform ne işe yarar gibi kısa bir özet geçtik. 

Yazılarımın asıl kaynağı terraform.io platformu. Oradaki dökümantasyonu Türkçe'ye çevirme ve ihtiyacımız olanları öne çıkarma taraftarıyım. Kafa karıştırma ihtimali olan kısımları dahil etmeyeceğim seriye. En basit düzeyde, ihtiyacımız kadar kullanacağız her aracı. Şahsi mentalitem bu, size dayatma gibi bir planım yok. Anlatacağım tüm yazılardaki araçların, dillerin hepsinin resmi dökümantasyonlarında daha fazla bilgiye erişebilirsiniz. Benim buradaki amacım DevOps Türkçe kaynağını arttırabilmek.

### MacOS Cihazlar İçin

Homebrew, Macbook kullananlar için velinimet. Kullanmıyorsanız hemen kullanmaya başlayın [https://brew.sh/tr/](https://brew.sh/tr/) buradan adım adım ne yapmanız gerektiğiyle ilgili bilgilere ulaşabilirsiniz. 

Başlıyoruz:

Kurulum yaptı iseniz veya daha önce kurulu ise homebrew terminali açıyoruz. İlk olarak, tüm Homebrew paketlerimizin bulunduğu hashicorp/tap repositorysini çağırıyoruz. Bu niye derseniz [https://docs.brew.sh/Taps](https://docs.brew.sh/Taps) adresinden Taps'i de öğrenebilirsiniz.

```plaintext
brew tap hashicorp/tap
```

Şimdi `hashicorp/tap/terraform` paketini yüklüyoruz.

```plaintext
brew install hashicorp/tap/terraform
```

Bu yükleme ile Terraform'un en güncel sürümünü sisteminize çeker ve kurulumunu tamamlar.

```plaintext
brew upgrade hashicorp/tap/terraform
```

Eğer güncelleme geldi ise **upgrade** komutu ile Terraform'un en güncel paketine erişebilirsiniz. Terraform'u güncellerken Homebrew yazılımının da en güncel sürümüne otomatik olarak yükleyecektir öncesinde.

### Windows Cihazlar İçin

Homebrew'ün Windows cihazlar için alternatifi **Choco** kullanacağız burada. [https://docs.chocolatey.org/en-us/choco/setup](https://docs.chocolatey.org/en-us/choco/setup) kaynak olarak sizde burayı kullanabilirsiniz. Sektördeki kişilerin bilgi birikimine güvendiğim için uzun uzadıya anlatmama taraftarıyım her şeyi. Sorunuz olursa iletişime geçebilirsiniz. Belki ilerleyen zamanlarda bu içerikleri videoya da dönüştürebiliriz.

#### cmd.exe terminal ile kurulum (Windows + R diye hatırlıyorum kısa yolu)

```plaintext
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

#### PowerShell terminal ile kurulum

```plaintext
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Eski usül indir yükle taraftarı iseniz [https://community.chocolatey.org/packages/chocolatey](https://community.chocolatey.org/packages/chocolatey) adresinden solda küçücük gri bir Download butonu var. En güncel sürüme oradan erişebilirsiniz. 

Paket yükleyicinizi kurduktan sonra ister cmd.exe ister PowerShelli açıp install komutuyla en güncel sürümü yükleyebilirsiniz.

```plaintext
 choco install terraform 
```

### Linux Kullanıcıları İçin

[https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) adresinden detaylara ulaşabilirsiniz. Linux kullanıcısı olduğunuzdan dolayı bu tarz minimal açıklamalara ihtiyaç duyacağınızı düşünmüyorum, affola.

---

### Kurulum Tamamlandıktan Sonra

Kurulum işlemi tamamlandıktan sonra aşağıdaki kod ile Terraform'un yüklenip yüklenmediğini kontrol edebilirsiniz. Help ile neler yapılabileceğini görebilirsiniz, versiyon ile ise sürüm kontrolü yapabilirsiniz.

```plaintext
terraform -help
```

```plaintext
terraform -version
```