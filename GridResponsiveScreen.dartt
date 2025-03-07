
import 'package:flutter/material.dart';
import 'package:cached_network_image/cached_network_image.dart';

void main() {
  runApp(MaterialApp(
    debugShowCheckedModeBanner: false,
    home: ProductListPage(),
  ));
}

// Dummy Product Model
class Product {
  final int id;
  final String name;
  final String url1;
  final String price;
  final String mrp;
  final int quantity;

  Product({
    required this.id,
    required this.name,
    required this.url1,
    required this.price,
    required this.mrp,
    required this.quantity,
  });
}

// Dummy Product List
List<Product> sampleProducts = [
  Product(id: 1, name: "iPhone 15 Pro", url1: "https://via.placeholder.com/150", price: "119999", mrp: "129999", quantity: 5),
  Product(id: 2, name: "Samsung S23 Ultra", url1: "https://via.placeholder.com/150", price: "99999", mrp: "109999", quantity: 3),
  Product(id: 3, name: "OnePlus 11 5G", url1: "https://via.placeholder.com/150", price: "56999", mrp: "61999", quantity: 7),
  Product(id: 4, name: "Sony Headphones", url1: "https://via.placeholder.com/150", price: "24999", mrp: "29999", quantity: 10),
  Product(id: 5, name: "MacBook Air M2", url1: "https://via.placeholder.com/150", price: "114999", mrp: "124999", quantity: 2),
  Product(id: 6, name: "Dell XPS 13", url1: "https://via.placeholder.com/150", price: "98999", mrp: "109999", quantity: 4),
];

class ProductListPage extends StatefulWidget {
  @override
  _ProductListPageState createState() => _ProductListPageState();
}

class _ProductListPageState extends State<ProductListPage> {
  final TextEditingController searchController = TextEditingController();
  final Map<int, int> quantities = {};
  final Set<int> addedToCart = {};
  List<Product> displayedProducts = [];

  @override
  void initState() {
    super.initState();
    displayedProducts = sampleProducts;
  }

  void _filterProducts(String query) {
    if (query.isEmpty) {
      setState(() => displayedProducts = sampleProducts);
    } else {
      setState(() {
        displayedProducts = sampleProducts
            .where((product) =>
                product.name.toLowerCase().contains(query.toLowerCase()))
            .toList();
      });
    }
  }

  Widget _buildProductListItem(Product product, double imageSize, double fontSize) {
    bool isInCart = addedToCart.contains(product.id);
    int qty = quantities[product.id] ?? 1;

    double price = double.tryParse(product.price) ?? 0.0;
    double mrp = double.tryParse(product.mrp) ?? 0.0;
    double discountPercentage =
        (mrp > price && mrp > 0) ? ((mrp - price) / mrp) * 100 : 0;

    return Card(
      elevation: 4,
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
      child: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Stack(
              children: [
                ClipRRect(
                  borderRadius: BorderRadius.circular(8),
                  child: CachedNetworkImage(
                    imageUrl: product.url1,
                    height: imageSize,
                    width: imageSize,
                    fit: BoxFit.cover,
                    placeholder: (context, url) => CircularProgressIndicator(),
                    errorWidget: (context, url, error) =>
                        Icon(Icons.error, size: imageSize / 2),
                  ),
                ),
                if (discountPercentage > 0) 
                  Positioned(
                    top: 5,
                    left: 5,
                    child: Container(
                      padding: EdgeInsets.symmetric(horizontal: 6, vertical: 2),
                      decoration: BoxDecoration(
                          color: Colors.red, borderRadius: BorderRadius.circular(10)),
                      child: Text(
                        '${discountPercentage.toStringAsFixed(0)}% OFF',
                        style: TextStyle(color: Colors.white, fontSize: fontSize * 0.8),
                      ),
                    ),
                  ),
              ],
            ),
            SizedBox(height: 8),
            Text(
              product.name,
              textAlign: TextAlign.center,
              maxLines: 2,
              overflow: TextOverflow.ellipsis,
              style: TextStyle(fontSize: fontSize, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 4),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text('₹${price.toStringAsFixed(2)}',
                    style:
                        TextStyle(fontSize: fontSize, fontWeight: FontWeight.bold)),
                SizedBox(width: 6),
                if (mrp > price)
                  Text('₹${mrp.toStringAsFixed(2)}',
                      style: TextStyle(
                          fontSize: fontSize * 0.9,
                          decoration: TextDecoration.lineThrough,
                          color: Colors.grey)),
              ],
            ),
            SizedBox(height: 8),
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                backgroundColor: isInCart ? Colors.green : Colors.amber,
              ),
              onPressed: () {
                setState(() {
                  addedToCart.add(product.id);
                  quantities[product.id] = qty;
                });
              },
              child: Text(isInCart ? "Qty: $qty" : "Add to Cart", style: TextStyle(fontSize: fontSize)),
            ),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    double screenWidth = MediaQuery.of(context).size.width;

    // Determine grid layout based on screen width
    int crossAxisCount;
    double imageSize;
    double fontSize;
    double gridHeight;

    if (screenWidth < 360) {
      // Extra Small (Phones like iPhone SE)
      crossAxisCount = 2;
      imageSize = 80;
      fontSize = 10;
      gridHeight = 200;
    } else if (screenWidth < 600) {
      // Small (Most Phones)
      crossAxisCount = 2;
      imageSize = 100;
      fontSize = 12;
      gridHeight = 220;
    } else if (screenWidth < 900) {
      // Medium (Tablets, Foldable Phones)
      crossAxisCount = 3;
      imageSize = 120;
      fontSize = 14;
      gridHeight = 250;
    } else {
      // Large Screens (Desktops, Large Tablets)
      crossAxisCount = 4;
      imageSize = 150;
      fontSize = 16;
      gridHeight = 280;
    }

    return Scaffold(
      appBar: AppBar(
        title: TextField(
          controller: searchController,
          decoration: InputDecoration(
            hintText: "Search Products...",
            border: InputBorder.none,
          ),
          onChanged: _filterProducts,
        ),
      ),
      body: GridView.builder(
        padding: EdgeInsets.all(8),
        itemCount: displayedProducts.length,
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: crossAxisCount,
          crossAxisSpacing: 8,
          mainAxisSpacing: 8,
          mainAxisExtent: gridHeight, // Adjust height dynamically
        ),
        itemBuilder: (context, index) {
          final product = displayedProducts[index];
          return _buildProductListItem(product, imageSize, fontSize);
        },
      ),
    );
  }
}
