# The Cashier App
Program sederhana berbasis C#.
## Functionalities
  - user dapat menginput nama item.
  - user dapat memilih 2 tipe, yaitu antara barang dan jasa.
  - user dapat menentukan jumlah barang.
  - user dapat menginput harga barang.
  - program akan men-generate listbox dan total harga ketika button tambahkan di klik.
  - terdapat 4 kolom dalam listbox yaitu item, quantity price dan subtotal.
  - kolom item memuat daftar nama barang atau jasa.
  - kolom quantity memuat daftar jumlah dari tiap barang atau jasa.
  - kolom price memuat harga per-barang/jasa.
  - kolom subtotal memuat total harga dari hasil perkalian antara quantity dan price.
  - label total memuat hasil penjumlahan harga dari subtotal tiap barang dan jasa dalam listbox.

## How Does It works?
Terdapat 3 class yaitu class `Item`, `Calculator`, dan `MainWindow`. Dalam class `Item` berfungsi untuk mendeklarasikan field dan property sehingga source code lebih rapih dan mudah diakses. Hal ini merupakan penerapan dari Single responsibility yaitu tiap Class bertanggung jawab secara mandiri. Pada class `Calculator` terdapat method `addItem` dimana method ini mengakses method `getTotal` dari class `Item`.
```csharp
public void addItem(Item item)
        {
            this.listItem.Add(item);
            this.total += item.getSubTotal();
        }
```
Pada class `MainWindow` terdapat 2 method yaitu `MainWindow` yang merupakan class constructor dan `AddButton_Click`. `MainWindow` berisi instance dari class `Calculator` dan logic yang menghubungkan listBox dengan method `Calculator.getListItem()` sehingga ketika item ditambahkan akan muncul pada listbox.
```csharp
public MainWindow()
        {
            InitializeComponent();
            calculator = new Calculator();
            listBox.ItemsSource = calculator.getListItem();
        }
```
Method `AddButton_Click` berfungsi untuk mengirim value yang diinputkan kedalam variabel kemudian dimasukkan kedalam instance item agar dapat diolah pada method `Calculator.AddItem()`. Kemudian hasil perhitungan dikirim kedalam `TotalLabel.content`.
```csharp
private void AddButton_Click(object sender, RoutedEventArgs e)
        {
            string title = itemNameBox.Text;
            int quantity = int.Parse(quantityBox.Text);
            string type = typeBox.Text;
            double price = double.Parse(priceBox.Text);

            Item item = new Item(new Random().Next(), title, quantity, type, price);
            calculator.addItem(item);
            double total = calculator.getTotal();

            totalLabel.Content = String.Format("Rp. {0}", total);

            listBox.Items.Refresh();
        }
```