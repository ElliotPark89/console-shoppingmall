# console-shoppingmall

import 'dart:io';

class Product {
  String name;
  int price;

  Product(this.name, this.price);
}

class ShoppingMall {
  final List<Product> products; // 판매하는 상품 목록
  final List<Map<String, dynamic>> cart = []; // 장바구니
  int totalPrice = 0; // 장바구니 총 가격

  ShoppingMall(this.products);

  void showProducts() {
    print('\n=== 상품 목록 ===');
    for (var product in products) {
      print('${product.name} / ${product.price}원');
    }
  }

  void addToCart() {
    stdout.write('\n장바구니에 담을 상품 이름을 입력하세요: ');
    final productName = stdin.readLineSync();
    stdout.write('담을 상품의 개수를 입력하세요: ');
    final productQuantity = int.tryParse(stdin.readLineSync() ?? '');

    final product = products.firstWhere(
      (p) => p.name == productName,
      orElse: () => Product('', 0),
    );

    if (product.name.isEmpty) {
      print('입력값이 올바르지 않아요!');
      return;
    }
    if (productQuantity == null || productQuantity <= 0) {
      print(productQuantity == null
          ? '입력값이 올바르지 않아요!'
          : '0개보다 많은 개수의 상품만 담을 수 있어요!');
      return;
    }

    cart.add({'product': product, 'quantity': productQuantity});
    totalPrice += product.price * productQuantity;
    print('장바구니에 상품이 담겼어요!');
  }

  void showTotal() {
    if (cart.isEmpty) {
      print('\n장바구니가 비어 있습니다.');
      return;
    }
    print('\n=== 장바구니 ===');
    for (var item in cart) {
      print(
          '${item['product'].name} / ${item['quantity']}개 / ${item['product'].price * item['quantity']}원');
    }
    print('장바구니에 ${totalPrice}원 어치를 담으셨네요!');
  }
}

void main() {
  final shoppingMall = ShoppingMall([
    Product('노트북', 1500000),
    Product('스마트폰', 800000),
    Product('헤드폰', 200000),
    Product('키보드', 100000),
    Product('마우스', 50000),
  ]);

  while (true) {
    print('\n==== 콘솔 쇼핑몰 ====');
    print('1. 판매하는 상품 목록 보기');
    print('2. 장바구니에 상품 담기');
    print('3. 장바구니 보기 및 총 가격 확인');
    print('4. 쇼핑몰 종료');
    stdout.write('메뉴를 선택하세요: ');
    final choice = stdin.readLineSync();

    if (choice == '1') {
      shoppingMall.showProducts();
    } else if (choice == '2') {
      shoppingMall.addToCart();
    } else if (choice == '3') {
      shoppingMall.showTotal();
    } else if (choice == '4') {
      print('\n이용해 주셔서 감사합니다 ~ 안녕히 가세요!');
      break;
    } else {
      print('\n지원하지 않는 기능입니다! 다시 시도해 주세요..');
    }
  }
}
