## Entity Framework Kurulumu

Models>Add>New İtem>Bu işlem için Visual Studio IDE ile projeye **ADO.NET Entity Data Model** ve **EF Designer from database** seçeneği ile veritabanı bağlantısı yapılarak işlem yapılacak tabloların eklenmesi yeterli olacaktır. Eğer istediğiniz data gelmez ise **New** **Connection** diyerek **Server** **Name** alanına serverınızın adını yazın önrek ./SQLEXPRES sonrada alt tarafa database. adını yazın 

Not: Kurulum. yaparken. Id kolonunu primery key olarak seçip Identity sini 1 yapmayı unutmayın

### Örnek Database First kullanımı

Öncelikle Controller ımıza Create Controllerini ekleyelim

```csharp
public ActionResult Create(){

return View()
}

[HttpPost]
public ActionResult Create(FormCollection Get){ /* FormCollection yerine Urunler
den parametre alarak da yapabilirsin fakat böyle daha kullanışlı olduğunu düşündüm
buyüzden kullandım*/
 
denemeEntities db = new denemeEntities();
Kullanıcılar k = new Kullanıcılar();
k.KullanıcıName = Get["KullanıcıName"].Trim(); //Trim boşlukları kaldırır
k.KullanıcıDescription = Get["KullanıcıDescription"].Trim(); //Trim boşlukları kaldırır

db.Kullanıcılar.Add(k);
db.SaveChanges();

return RedirectToAction("Index","Home")
}
```

Sonrasında Viewimizi ekliyip Formumuzu ekliyoruz form name lerine ise kolon adlarını ekliyoruz

örnek: KullanıcıName gibi

**Ve evet artık database ile bağlantıyı kurduk ve Create işlemi gerçekleştirebiliyoruz**

### Update işlemi için ise

View için ise  Update sağ tık>Add View>MVC5 View>Template:Edit, Model Class:Veritabanı adı, Data Context: ProductEntities>Add

```csharp
public ActionResult Update(int id){
	var d = db.Categories.Find(id);

	return View(d);
}
[HttpPost]
public ActionResult Update(Categories c){
	Categories cat = db.Categories.Where(x=>x.CategoryId == c.CategoryId)
	.FirstOrDefault(); //Linq sorgusu anlamları bir sonraki katalog da

	cat.CategoryName = c.CategoryName;
	cat.CategoryDescription = c.CategoryDescription;

	db.SaveChanges();s
	return View(d);
}

//Veya daha kolay bir şekilde bu işlmeleri bizim için yapan bir metot ilede yapabiliriz

public ActionResult Update(int id){
	var d = db.Categories.Find(id);

	return View(d);
}
[HttpPost]
public ActionResult Update(Categories c){
	db.Entry(c).State = EntityState.Modeified;//Aslında başta yaptıklarımın
// Aynısını yapıyor
}
```

---

# Linq Sorguları

> OrderBy(x⇒x.Name) //ismi Alfabeye göre sırala
> 

> Where(x⇒x.yas == 20) //Yaş 20 ise
> 

> Max() //maksimum değeri alır
> 

> Min() //Minumum değeri alır
> 

> Sum() //Sayı kumesindeki elemanların toplamını bulan extension methodtur.
> 

## **First**

Collectiondaki ilk elemanı bulan extension methodtur. Eğer collectionda aranan değer bulunamadıysa *InvalidOperationException* fırlatır.

## **FirstOrDefault**

Collectiondaki ilk elemanı bulur, eğer eleman yoksa type’in default value’sini döndürür.

## **Last**

Collectiondaki son elemanı bulan extension methodtur. Eğer collectionda aranan değer bulunamadıysa *InvalidOperationException* fırlatır.

## **LastOrDefault**

Collectiondaki son elemanı bulur, eğer eleman yoksa type’in default value’sini döndürür.

## **Single**

Collectionda yer alan tek bir uniqe elemanı bulan extension methodtur. Eğer collectionda aranan eleman bulunamadıysa veya birden daha fazla sayıda varsa *InvalidOperationException* fırlatır.

## **SingleOrDefault**

Collectionda yer alan tek bir uniqe elemanı bulan extension methodtur. Eğer eleman bulunamdıysa diğer *OrDefault’lar gibi* default valueyi döner. Aranan eleman collectionda yoksa *InvalidOperationException* fırlatır.

---

# Html Helper

> For değerlerini model ile kullanıldığı zaman kullanıyoruz DropDownListFor gibi
> 

> [https://www.muratoner.net/aspnet/aspnet-mvc/aspnet-mvc-html-helper-elemanlari](https://www.muratoner.net/aspnet/aspnet-mvc/aspnet-mvc-html-helper-elemanlari) daha fazlası için burdaki makaleyi okuya bilirsiniz
> 

### Form

```csharp
@using (Html.BeginForm("About","Home",FormMethod.Post)){
	<input type="text" name="isim"/>
	<input type="submit" value="Kaydet"/>
}
//Html versiyonu
<form action="/Home/About" method="Post">
	<input type="text" name="isim"/>
	<input type="submit" value="Kaydet"/>
</form>
```

### CheckBox

```csharp
@Html.CheckBox("hobi1",false) 
@Html.CheckBox("hobi2",false)
@Html.CheckBox("hobi3",false)
//Html versiyonu
<input type="checkbox" name="hobi1" value="hobi1"/>
<input type="checkbox" name="hobi2" value="hobi2"/>
<input type="checkbox" name="hobi3" value="hobi3""/>
```

### DropDownList

```csharp
@Html.DropDownList("Ogrenciler","Lütfen bir öğrenci seç")
//Html versiyonu
<label for="Ogrenciler">Lütfen bir öğrenci seç</label>
<select name="Ogrenciler" id="Ogrenciler">
  <option value="ogrenci1">ogrenci1</option>
  <option value="ogrenci2">ogrenci2</option>
  <option value="ogrenci3">ogrenci3</option>
  <option value="ogrenci4">ogrenci4</option>
</select>
```

### TextArea

```csharp
@Html.TextArea("aciklama","",new {@row = 5})
//Html versiyonu
<label>aciklama:</label>
<textarea name="aciklama" rows="4" cols="50">
Bu uzun bir açıklama metnidir
</textarea>
```

### TextBox

```csharp
@Html.TextBox("isim","",new {maxlength = 5})
//Html versiyonu
<input type="text" name="isim">
```

### Silmek istediğinize emin misiniz ? (Continue)

```csharp
@Html.ActionLink("Delete","Delete",new {id = item.id},new {@class = "btn btn-danger", onclick = "return confirm('Silmek istediğinize emin misiniz')"})
//Güvenlik için çok önemli bir koddur
```

---

# Fotoğraf ekleme (Statik dosya üzerinden)

Bu metot için herhangi bir veri tabanı kolonuna ihtiyacın yok bunun için yapman gekren şu

Öncelikle Create.cshtml de şu inputu girin

```html
<div class="form-group">
        <label class="control-label col-md-2">Product Image</label>
        <div class="col-md-10">
            <input type="file" name="ProductImage" id="ProductImage" class="form-control" />
        </div>
    </div>
```

Bizden ProductImage controller ı arıyor fakat biz sahip değiliz

bunun için öncelikle kodumuzun html helper ındaki şu değişikliği yapmalyıız:

```csharp
@using (Html.BeginForm(null,null,FormMethod.Post,new {enctype="multipart/form-data" }))
```

Ardından ProductController dan Create metoduna bir parametre girmeliyiz

bu parametre şu

```csharp
public ActionResult Create([Bind(Include = "ProductId,ProductName,RefCategoryId,ProductDescription,ProductPrice")] Products products,
            HttpPostedFileBase ProductImage)
//Aslında burda bizim işimize yarıyan yer mor olan
{

if (ModelState.IsValid)
  {
    db.Products.Add(products);
    db.SaveChanges();

    if (ProductImage != null && ProductImage.ContentLength>0)
    {
				// Dosyayı /Image den çek ürün ıd sine göre isimlendir ve .jpg e çevir
        string FilePath = Path.Combine(Server.MapPath("~/Image"), products.ProductId + ".jpg");
        ProductImage.SaveAs(FilePath);//File Path farklı kaydet
    }

    return RedirectToAction("Index");

}
```

### Birden fazla fotoğraf ekeleme (Uploading a File (Or Files) With ASP.NET MVC)

Bu konu için size bir link bırakıcam burdan çok daha detaylı bilgi alabilirsiniz

[http://haacked.com/archive/2010/07/16/uploading-files-with-aspnetmvc.aspx/](http://haacked.com/archive/2010/07/16/uploading-files-with-aspnetmvc.aspx/)

### Ürünü silerken Fotoğrafıda silme

Burda yaptığımız şey şu ürünleri sildiğimiz zaman dosyamızdan fotoğrafı silmeyecek çünkü fotoğraflar çoktan eklenmiş olucak bunu silmek için bu kod çok Önemli

```csharp
public ActionResult DeleteConfirmed(int id)
        {
            Products products = db.Products.Find(id);
            db.Products.Remove(products);
            db.SaveChanges();
            //---------------------------------------
            string FilePath = Path.Combine(Server.MapPath("~/Image"), products.ProductId + ".jpg");
            FileInfo fi = new FileInfo(FilePath);

            if (fi.Exists)
            {
                fi.Delete();
            }
            //Burda yaptıklarımız Ürünü sildiğimizde Fotoğrafıda otomatik olarak silsin 
            //---------------------------------------

            return RedirectToAction("Index");
        }
```

---

# Authentication (Kimlik Doğrulama) ve Login işlemi

Controller ımızı da kuralım LoginController.cs adında 

```csharp
namespace Login.Controllers
{
    [AllowAnonymous] /*Global.asax da tüm sayfaları kilitlemiştik
(bu arada aşağıda bunu nasıl yaptığımızı anlatıyotum) fakat bu sayfamız
anahtar niteliğinde olucak yani login olucaz ve kilitli sayfaları açıcak */
    public class LoginController : Controller
    {
        MySiteEntities db = new MySiteEntities();
        
        public ActionResult Index()
        {
            return View();
        }
        [HttpPost]
        public ActionResult Index(Kullanici k)
        {
            var s = db.Kullanici.FirstOrDefault(x => x.UserName == k.UserName && x.Password == k.Password);
/*Üstte yaptığımız şey User name ler ve şifreler eşit ise diyoruz */
            if (s != null)
            {
/*s eşit değil ise User Name cookie sini ekle ve cookie yi cookie ler silenene kadar kaydet*/
                FormsAuthentication.SetAuthCookie(s.UserName, True); /*Cookie aktif ettik eğer false değerini true olarak girersek cookilerimizi bilgisayar
                da tutucak ve her siteye griiş yapışımızda bizi login edicek (Remember Me) gibi düşünün*/
                return RedirectToAction("Index","Home");
            }
            else
            {
                ViewBag.msg = "invalid Username or Password";
/*username ve password tutmuyorsa isim veya şifre yanlıştır değerini döndür*/
                    return View();
            }
            
        }
        public ActionResult LogOut()
        {
/*LogOut yani cookileri sil burda cookilerimizi sildirtiyoruz*/
            FormsAuthentication.SignOut();
            return RedirectToAction("Index");
        }
    }
}
```

Diyelimki tasarımımızı yaptık şimdi sıra name alanlarını düzenleme

```html
<input type="text" name="UserName" class="form-control" placeholder="User Name">
<input type="password" name="Password" class="form-control" placeholder="Password">
<input name="Remember" type="checkbox">Remember Me
<!--Öncelikle name alanlarımıza Veritabanında girdiğimiiz kolon adlarını girelim -->
```

Tamamdır tasarım alanını bitirdik şimdi sıra Authentication da bunun için öncelikle Web.config gelelim ve aşağıda belirttiğimiz kodları yazalım /Login/Index bizim Login sayfamızın tasarımının olduğu yer

![5](https://user-images.githubusercontent.com/84273839/146655431-3fde3f8f-f53f-462a-871c-1804435f78fc.jpg)


Eğer biz tüm sayfaları kilitlemek istiyorsak ki önerim budur çünkü teker teker her action ımızın üstüne Authentication yazmak bizi uğraştırır oyüzden direk tüm sayfaları kilitleyeceğiz Global.asax a 

geliyoruz ve ilk sıradaki kodu yazıyoruz

![6](https://user-images.githubusercontent.com/84273839/146655440-ff9e935a-5b91-4f42-ba69-5b4cc2b469f2.jpg)


## Rolleme (Yetkiler)

Rolleme Rolleme dediğimiz şey kullanıcı yetkisidir mesela A kullanıcı B kullanıcısının erişebildiği yere erişemez bu tarz kurallar ile doludur öncelikle Web.config imizden ufak bir ayarlama yapalım 

<add name="testRoleProvider" type="Login.Login.AdminRoleProvider"/> burda yazdığımız koddaki name alanı o an oluşturduğumuz bir tanımlama type alanında yaptığımız şey ise Login dosyamızdaki AdminRoleProvider yani rollerin tanımlamasını yaptığımız yerin namespace i

![1](https://user-images.githubusercontent.com/84273839/146655470-c1dace72-e670-4c38-8695-f46f1033eb20.jpg)

Hatta hemen AdminRoleProvider ımızı kuralım projemize gelip yeni bir dosya oluşturalım ve adı Login olsun dosyanın içine AdminRoleProvider adında bir class oluşturalım ve classımıza bir kalıtım verelim

![2](https://user-images.githubusercontent.com/84273839/146655444-49827bd9-8257-4857-b6c6-42b70032402f.jpg)


RoleProvider aslında bir Interface niteliğinde yani içerisinde dolu metotlar taşıyor biz bu metotları AdminRoleProvider üstüne gelerek dosymıza import ediyoruz  GetRolersForUsers alanına geliyoruz

Aslında içerisinde bir hata fırlatıyor bunun nedeni ise dosya hatalı görünmesin diye neyse içerisindeki throw kodunu silip aşağıdaki kodu giriyoruz burda yaptığımız şey database deki username ile metot üzerindeki username eşitse ilk seçeneği seç anlamında bir kod yazdık ve bunu kullanici ya atadık ardından dizin şeklinde kullanici.Role dedik burda dediğimiz Role Database imizdeki Role Artık rollemeyi yaptık sıra hangisinin hangi sayfaya girebileceğine

![3](https://user-images.githubusercontent.com/84273839/146655448-76f00d8b-890f-425a-888c-44083c7e2812.jpg)

Bunun için HomeContorller da istediğimiz sayfaya rol atamasını yapabiliyoruz aynı bu şekilde burda istersen birden çok rolde seçebilirsiniz artık B rolü ile girişimiz yok

![4](https://user-images.githubusercontent.com/84273839/146655461-cc1d1a29-6fe8-40d5-be92-8eb2f151f256.jpg)


---

# Harici Metin Editörü dahil etme

Öncelikle editor olarak bunu kullanıyoruz: [https://ckeditor.com](https://ckeditor.com/)

Peki bunu nasıl kurucam bunun için visual studio>Nuget package management>Browse>Ckeditor indir

Sonrasında Bundle.config dizinimize gelip şu kodu yazın 

```csharp
bundles.Add(new ScriptBundle("~/Sripts/ckeditor").Include(
                "~/Scripts/ckeditor/ckeditor.js"));
```

Ardından html dosyamıza gelip kullanıcağımız dizine şu kodu yapıştırın ve library i dahil edin

```csharp
@section Scripts {
    @Scripts.Render("~/bundles/jqueryval")
    @Scripts.Render("~/Scripts/ckeditor")
}

//Ayriyetten bunu _Layout.cshtml e ekleyin

**@Scripts.Render("~/Scripts/jquery-3.4.1.min.js")
@Scripts.Render("~/ckeditor/ckeditor.js");
@Scripts.Render("~/ckeditor/ckfinder/ckfinder.js");
@Scripts.Render("~/Scripts/jquery.validate.js")**
```

Arından gene html imizden editoru eklemek için tek yapmamız gereken classına ckeditor eklemek olucak

class="ckeditor"

Not: ckeditor ile fotoğraf eklemek istediğimiz kodumuz bunu tehdidt algılayabilir bunu düzeltmek için şu attribute eklememiz gerek

```csharp
[ValidateInput(false)] //Ckeditor de fotoğraf eklemeye izin verdik
```

---
