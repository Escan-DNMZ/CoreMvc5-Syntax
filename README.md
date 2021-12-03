# AspNetMvc5
# Database First

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

![5](https://user-images.githubusercontent.com/84273839/144657199-4147046e-aa01-4a99-bf68-265716bb17a8.jpg)

Eğer biz tüm sayfaları kilitlemek istiyorsak ki önerim budur çünkü teker teker her action ımızın üstüne Authentication yazmak bizi uğraştırır oyüzden direk tüm sayfaları kilitleyeceğiz Global.asax a geliyoruz ve ilk sıradaki kodu yazıyoruz

![6](https://user-images.githubusercontent.com/84273839/144657068-a74e05d6-d399-4c32-9b21-fffa14e8ac30.jpg)

## Rolleme (Yetkiler)

Rolleme Rolleme dediğimiz şey kullanıcı yetkisidir mesela A kullanıcı B kullanıcısının erişebildiği yere erişemez bu tarz kurallar ile doludur öncelikle Web.config imizden ufak bir ayarlama yapalım 

<add name="testRoleProvider" type="Login.Login.AdminRoleProvider"/> burda yazdığımız koddaki name alanı o an oluşturduğumuz bir tanımlama type alanında yaptığımız şey ise Login dosyamızdaki AdminRoleProvider yani rollerin tanımlamasını yaptığımız yerin namespace i

![Inkedcapture_20211202221049164_LI](https://user-images.githubusercontent.com/84273839/144657300-40bdb5f8-7659-4b87-81d6-b90ed9165f7d.jpg)

Hatta hemen AdminRoleProvider ımızı kuralım projemize gelip yeni bir dosya oluşturalım ve adı Login olsun dosyanın içine AdminRoleProvider adında bir class oluşturalım ve classımıza bir kalıtım verelim

![2](https://user-images.githubusercontent.com/84273839/144656213-34e7727a-d03e-48ac-98da-9db296707aff.jpg)


RoleProvider aslında bir Interface niteliğinde yani içerisinde dolu metotlar taşıyor biz bu metotları AdminRoleProvider üstüne gelerek dosymıza import ediyoruz  GetRolersForUsers alanına geliyoruz

Aslında içerisinde bir hata fırlatıyor bunun nedeni ise dosya hatalı görünmesin diye neyse içerisindeki throw kodunu silip aşağıdaki kodu giriyoruz burda yaptığımız şey database deki username ile metot üzerindeki username eşitse ilk seçeneği seç anlamında bir kod yazdık ve bunu kullanici ya atadık ardından dizin şeklinde kullanici.Role dedik burda dediğimiz Role Database imizdeki Role Artık rollemeyi yaptık sıra hangisinin hangi sayfaya girebileceğine

![3](https://user-images.githubusercontent.com/84273839/144656356-92dfb8e4-f990-438f-aaf1-097ded6ae86d.jpg)

Bunun için HomeContorller da istediğimiz sayfaya rol atamasını yapabiliyoruz aynı bu şekilde burda istersen birden çok rolde seçebilirsiniz artık B rolü ile girişimiz yok
![4](https://user-images.githubusercontent.com/84273839/144656447-64291917-2fbf-403f-8279-4d229322e242.jpg)


