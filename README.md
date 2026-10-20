# UTS_PAM_C
Nama  : Ahmad Suhaemi
Nim   : 1207050006
Kelas : Pengembangan Aplikasi Mobile_C





Materi 6: RecyclerView

Pokok Bahasan
Melakukan implementasi RecyclerView pada Android

Tujuan
Di Akhir pertemuan ini, diharapkan mahasiswa dapat memahami dan juga mengimplementasikan penggunaan RecyclerView pada saat membuat aplikasi Android.

Penjelasan Singkat
Pada pertemuan kali ini anda akan memahami dan juga mengimplementasikan cara menggunakan RecyclerView. RecyclerView merupakan komponen yang cukup penting dikarenakan sering digunakan untuk menampilkan list pada sebuah aplikasi. RecyclerView merupakan scrollable container yang dipergunakan untuk menampilkan data dalam jumlah besar. Dengan cara ini salah satunya merupakan cara efisiensi untuk membatasi elemen yang ditampilkan pada View, kemudian ketika ada perubahan data dalam elemen RecyclerView akan dengan mudah dilakukan penambahan dan juga update.

Adapun komponen yang dipergunakan dalam RecyclerView diantaranya sebagai berikut:
RecyclerView merupakan komponen scrolling list untuk setiap item
Layout untuk satu item data yang ditulis dalam XML file
Data
Layout Manager yang dipergunakan untuk menghandle untuk komponen UI dalam satu View (Recyclerview.LayoutManager)
Adapter yang dipergunakan untuk menghandle koneksi data ke RecyclerView (RecyclerView.Adapter)
ViewHolder	yang	menyediakan	informasi	untuk	menampilkan	satu	item (RecyclerView.ViewHolder)

Berikut ini merupakan gambaran dari cara kerja pada RecyclerView yang ditampilkan pada gambar dibawah ini.


Modul 1: Melakukan implementasi RecyclerView pada aplikasi Android sederhana
Pada modul pertama ini anda akan membuat satu buah tampilan yang berisi daftar yang ditampilkan dalam bentuk RecyclerView. Adapun langkah yang perlu dilakukan diantaranya sebagai berikut:
Buat satu buah project baru dengan nama RecyclerViewSample, kemudian pilih Empty Views Activity. Kemudian sesuaikan project tersebut dengan nama package lat.pam.recyclerviewsample. Gunakan bahasa pemrograman Kotlin dengan minimum API 27 (“Oreo”: Android 8.1) seperti dibawah ini.


Setelah project tersebut berhasil dibuat, anda dapat mengganti script xml pada activity_main.xml menjadi seperti dibawah ini.
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">


   <androidx.recyclerview.widget.RecyclerView
       android:id="@+id/recyclerview"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       tools:itemCount="5"
       tools:listitem="@layout/card_view_design" />


</androidx.constraintlayout.widget.ConstraintLayout>



Kemudian tambahkan card_view_design.xml pada file terbaru layout yang diperuntukkan untuk satu item layout yang dibuat. Anda dapat membuatnya dengan menambahkannya seperti pada gambar dibawah ini.

<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="match_parent"
   android:layout_height="wrap_content">


   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:orientation="horizontal"
       android:padding="5dp">


       <TextView
           android:id="@+id/textView"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:layout_marginStart="10dp"
           android:layout_marginLeft="15dp"
           android:text="Item"
           android:textSize="20sp"
           android:textStyle="bold" />


   </LinearLayout>
</androidx.cardview.widget.CardView>

Tambahkan class baru dengan cara seperti berikut.Kemudian tambahkan kode berikut.


ItemsViewModel.kt
package lat.pam.samplerecyclerview
data class ItemsViewModel(val text: String) {}

Tambahkan kode berikut pada Adapter Class, adapter class ini yang berfungsi menampung data yang dikirimkan dari Model yang akan ditampilkan pada Activity. Silahkan tambahkan kode berikut.

CustomAdapter.kt
package lat.pam.recyclerviewsample


import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView
import lat.pam.samplerecyclerview.ItemsViewModel


class CustomAdapter(private val mList:List<ItemsViewModel>): RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
   override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CustomAdapter.ViewHolder {
       // inflates the card_view_design view
       // that is used to hold list item
       val view = LayoutInflater.from(parent.context)
           .inflate(R.layout.card_view_design, parent, false)


       return ViewHolder(view)
   }


   override fun onBindViewHolder(holder: CustomAdapter.ViewHolder, position: Int) {
       val ItemsViewModel = mList[position]


       // sets the image to the imageview from our itemHolder class
//        holder.imageView.setImageResource(ItemsViewModel.image)


       // sets the text to the textview from our itemHolder class
       holder.textView.text = ItemsViewModel.textxxx`
   }


   override fun getItemCount(): Int {
       return mList.size
   }


   class ViewHolder(ItemView: View):RecyclerView.ViewHolder(ItemView) {
       //        val imageView: ImageView = itemView.findViewById(R.id.imageview)
       val textView: TextView = itemView.findViewById(R.id.textView)
   }
}


Tambahkan kode berikut pada MainActivity.kt untuk menampilkannya list data tersebut pada Activity, silahkan pindahkan kode berikut ini.
MainActivity.kt
package lat.pam.recyclerviewsample


import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import lat.pam.samplerecyclerview.ItemsViewModel


class MainActivity : AppCompatActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       setContentView(R.layout.activity_main)


       // getting the recyclerview by its id
       val recyclerview = findViewById<RecyclerView>(R.id.recyclerview)


       // this creates a vertical layout Manager
       recyclerview.layoutManager = LinearLayoutManager(this)


       // ArrayList of class ItemsViewModel
       val data = ArrayList<ItemsViewModel>()


       // This loop will create 20 Views containing
       // the image with the count of view
       for (i in 1..20) {
           data.add(ItemsViewModel("Item " + i))


       }


       // This will pass the ArrayList to our Adapter
       val adapter = CustomAdapter(data)
       Log.d("test string","Jumlah data: "+data.size)


       // Setting the Adapter with the recyclerview
       recyclerview.adapter = adapter
   }
}


Berikut ini merupakan tampilan dari project yang sudah anda coba pada modul ini.



Modul 2: Melakukan modifikasi tampilan halaman Desert menjadi RecyclerView pada aplikasi Android
Pada modul kedua ini anda akan memodifikasi tampilan yang telah anda buat sebelumnya menjadi bentuk RecyclerView. Adapun langkah yang perlu dilakukan diantaranya sebagai berikut:
Buat satu buah project baru dengan nama DessertRecycleView, kemudian pilih Empty Views Activity. Beri nama package: lat.pam.desertrecycleview. Kemudian sesuaikan project tersebut dengan nama package lat.pam.recyclerviewsample. Gunakan bahasa pemrograman Kotlin dengan minimum API 27 (“Oreo”: Android 8.1).

Pada bagian drawable klik kanan, pilih New dan Pilih Drawable Resource File, kemudian berilah nama circle_background.xml

Kemudian tulis kode XML berikut yang berfungsi untuk membuat circle background pada tampilan dari gambar desert.

<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
   android:shape="oval">
   <solid android:color="#C1E8C5" />
</shape>


Setelah itu buatlah tampilan dari layout main activity dengan melakukan modifikasi pada kode XML activity_main.xml. Lalu masukkan kode berikut ini.
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout
   xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   android:id="@+id/main"
   android:layout_width="match_parent"
   android:layout_height="match_parent">


   <!-- Content Area -->
   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical"
       app:layout_behavior="@string/appbar_scrolling_view_behavior">


       <!-- Header Title -->
       <TextView
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:paddingTop="12dp"
           android:paddingBottom="16dp"
           android:text="Droid Desserts"
           android:textAlignment="center"
           android:textColor="#4A454E"
           android:textSize="16sp"
           android:textStyle="bold" />


       <!-- List of Desserts -->
       <androidx.recyclerview.widget.RecyclerView
           android:id="@+id/recyclerView"
           android:layout_width="match_parent"
           android:layout_height="match_parent"
           android:clipToPadding="false"
           android:paddingHorizontal="24dp" />


   </LinearLayout>




</androidx.coordinatorlayout.widget.CoordinatorLayout>


Kemudian pastikan juga tampilan listnya sudah muncul, untuk selanjutnya kita akan membuat tampilan item pada setiap listnya.


Selanjutnya kita akan membuat item pada setiap list tersebut dengan membuat terlebih dahulu layout untuk satu item dengan cara pilih New >> XML >> Layout File. Kemudian beri nama layout tersebut dengan nama itemdesert.xml

Setelah dibuat kemudian masukkan layout berikut ke dalam file itemdesert.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
   xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:gravity="center_vertical"
   android:orientation="horizontal"
   android:paddingVertical="12dp">


   <!-- Circle Avatar with Image -->
   <FrameLayout
       android:id="@+id/imgContainer"
       android:layout_width="120dp"
       android:layout_height="120dp"
       android:background="@drawable/circle_background">


       <ImageView
           android:id="@+id/imgDessert"
           android:layout_width="90dp"
           android:layout_height="90dp"
           android:layout_gravity="center"
           android:contentDescription="Dessert Image"
           android:scaleType="fitCenter" />
   </FrameLayout>


   <!-- Item Description -->
   <TextView
       android:id="@+id/tvDescription"
       android:layout_width="0dp"
       android:layout_height="wrap_content"
       android:layout_marginStart="20dp"
       android:layout_weight="1"
       android:lineSpacingExtra="4dp"
       android:textColor="#4A454E"
       android:textSize="15sp" />


</LinearLayout>

Jangan lupa untuk memasukkan gambar dari desert yang telah diunduh dan diambil pada link berikut: 
https://github.com/google-developer-training/android-fundamentals-apps-v2/tree/master/DroidCafe/app/src/main/res/drawable 

Pindahkan file gambar tersebut kedalam folder atau bagian drawables, seperti pada gambar berikut ini.




Selanjutnya kita akan beralih untuk membuat object data class pada Kotlin dengan membuat kelas baru pada Kotlin dengan cara klik kanan pada nama package, pilih New >> Kotlin Class/ File dan beri nama Dessert. Lalu masukan kode berikut kedalamnya.
package lat.pam.desertrecycleview


data class Dessert(
   val description: String,
   val imageRes: Int,
   val colorHex: String
)

Kemudian untuk menampilkan item data kita membutuhkan adapter, oleh karena itu buat kelas baru kembali dengan cara pilih New >> Kotlin Class/ File dan beri nama DessertAdapter. Kemudian masukan kode berikut.

package lat.pam.desertrecycleview


import android.graphics.Color
import android.graphics.drawable.GradientDrawable
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.FrameLayout
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView


class DessertAdapter(private val dessertList: List<Dessert>) :
   RecyclerView.Adapter<DessertAdapter.DessertViewHolder>() {


   class DessertViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
       val imgContainer: FrameLayout = itemView.findViewById(R.id.imgContainer)
       val imgDessert: ImageView = itemView.findViewById(R.id.imgDessert)
       val tvDescription: TextView = itemView.findViewById(R.id.tvDescription)
   }


   override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): DessertViewHolder {
       val view = LayoutInflater.from(parent.context)
           .inflate(R.layout.itemdesert, parent, false)
       return DessertViewHolder(view)
   }


   override fun onBindViewHolder(holder: DessertViewHolder, position: Int) {
       val dessert = dessertList[position]


       holder.tvDescription.text = dessert.description
       holder.imgDessert.setImageResource(dessert.imageRes)


       // Dynamically change circle background tint color
       val circleBg = GradientDrawable().apply {
           shape = GradientDrawable.OVAL
           setColor(Color.parseColor(dessert.colorHex))
       }
       holder.imgContainer.background = circleBg
   }


   override fun getItemCount(): Int = dessertList.size
}

Langkah terakhir lakukan perubahan pada kelas MainActivity dan masukan kode berikut ini. Pastikan semua kode program yang error/ garis merah sudah diimport pada bagian atas dibawah nama package.
package lat.pam.desertrecycleview


import android.annotation.SuppressLint
import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView


class MainActivity : AppCompatActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       enableEdgeToEdge()
       setContentView(R.layout.activity_main)


       val recyclerView: RecyclerView = findViewById(R.id.recyclerView)


       // Color HEX values matching the screenshot[cite: 1]
       val desserts = listOf(
           Dessert(
               description = "Donuts are glazed and sprinkled with candy.",
               imageRes = R.drawable.donut_circle,
               colorHex = "#C1E8C5"
           ),
           Dessert(
               description = "Ice cream sandwiches have chocolate wafers and vanilla filling.",
               imageRes = R.drawable.icecream_circle,
               colorHex = "#BBE0F8"
           ),
           Dessert(
               description = "Froyo is premium self-serve frozen yogurt.",
               imageRes = R.drawable.froyo_circle,
               colorHex = "#E4BCDF"
           )
       )


       recyclerView.layoutManager = LinearLayoutManager(this)
       recyclerView.adapter = DessertAdapter(desserts)


       ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
           val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
           v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
           insets
       }
   }
}



Jalankan program tersebut dan kemudian lihat apakah hasilnya sudah sesuai seperti yang hendak diinginkan. Berikut ini tampilan dari program/ tampilan sederhana yang sudah dibuat.



Modul 3: Melakukan setting Github pada Android Studio
Berikut ini merupakan cara untuk melakukan setting dan menghubungkan project kita agar repositorynya terhubung ke Android Studio
Menghubungkan Akun GitHub ke Android Studio. Sebelum melakukan push project, pastikan akun GitHub kamu sudah terhubung di Android Studio:
Buka Android Studio.
Masuk ke menu File  Settings (untuk Windows/Linux) atau Android Studio  Settings (untuk macOS).
Di bilah pencarian sebelah kiri, pilih Version Control  GitHub.
lik ikon + (Add account) lalu pilih Log In via GitHub... (atau via Token).
Browser akan terbuka untuk meminta konfirmasi otorisasi. Klik Authorize Github.
Jika berhasil, nama akun GitHub kamu akan muncul di daftar Settings Android Studio. Klik Apply lalu OK.


Membuka Terminal / Fitur Git di Android Studio
Menggunakan Terminal bawaan Android Studio:
Di bagian paling bawah jendela Android Studio, terdapat tab bernama Terminal. Klik tab tersebut untuk langsung mengetikkan perintah git init, git add ., dan perintah git push seperti pada langkah tutorial sebelumnya.

Menggunakan GUI (Tanpa mengetik perintah):
Klik VCS (atau Version Control)  Share Project on GitHub. Android Studio akan membuatkan repositori baru di akun GitHub kamu secara otomatis.

Tugas: 
Setelah project selesai dikerjakan silahkan kumpulkan repository yang sudah dikerjakan pada LMS dengan menuliskan link github yang berisi repository yang sudah dikerjakan



