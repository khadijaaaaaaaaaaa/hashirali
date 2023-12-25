# hashirali
question 1 part b 
#include <iostream>
class Shape {
public:
    virtual ~Shape() {}  
    virtual double calculateArea() const = 0;  
};
class Circle : public Shape {
private:
    double radius;

public:
    Circle(double r) : radius(r) {}

    double calculateArea() const override {
        return 3.14 * radius * radius;
    }
};
class Rectangle : public Shape {
private:
    double length;
    double width;

public:
    Rectangle(double l, double w) : length(l), width(w) {}

    double calculateArea() const override {
        return length * width;
    }
};

int main() {
    Circle circle(5.0);
    Rectangle rectangle(4.0, 6.0);

    Shape* shape1 = &circle;
    Shape* shape2 = &rectangle;

    std::cout << "Area of Circle: " << shape1->calculateArea() << std::endl;
    std::cout << "Area of Rectangle: " << shape2->calculateArea() << std::endl;

    return 0;
}
question 2
#include <iostream>
#include <vector>
#include <string>

class Product {
public:
    int productId;
    std::string productName;
    double price;

    Product(int id, const std::string& name, double p)
        : productId(id), productName(name), price(p) {}

    void displayDetails() const {
        std::cout << "Product ID: " << productId << "\nProduct Name: " << productName << "\nPrice: $" << price << std::endl;
    }
};

class ShoppingCart {
private:
    std::vector<Product*> products;

public:
    ~ShoppingCart() {
        // Cleanup: Release the memory for products
        int i = 0;
        while (i < products.size()) {
            delete products[i];
            i++;
        }
        products.clear();
    }

    void addProduct(Product* product) {
        products.push_back(product);
    }

    void displayAllProducts() const {
        int i = 0;
        while (i < products.size()) {
            products[i]->displayDetails();
            std::cout << "--------------" << std::endl;
            i++;
        }
    }

    double calculateTotalCost() const {
        double total = 0.0;
        int i = 0;
        while (i < products.size()) {
            total += products[i]->price;
            i++;
        }
        return total;
    }
};

class User {
public:
    int userId;
    ShoppingCart* shoppingCart; // Aggregation

    User(int id) : userId(id), shoppingCart(NULL) {}

    ~User() {
        delete shoppingCart;
    }

    void displayDetails() const {
        std::cout << "User ID: " << userId << std::endl;
        if (shoppingCart) {
            std::cout << "Shopping Cart Contents:\n";
            shoppingCart->displayAllProducts();
            std::cout << "Total Cost: $" << shoppingCart->calculateTotalCost() << std::endl;
        } else {
            std::cout << "No shopping cart associated." << std::endl;
        }
    }
};

int main() {
    Product* product1 = new Product(1, "Laptop", 999.99);
    Product* product2 = new Product(2, "Headphones", 49.99);

    User* user1 = new User(101);
    User* user2 = new User(102);

    user1->shoppingCart = new ShoppingCart(); // Aggregation
    user2->shoppingCart = new ShoppingCart(); // Aggregation

    user1->shoppingCart->addProduct(product1);
    user1->shoppingCart->addProduct(product2);
    user2->shoppingCart->addProduct(product1);

    user1->displayDetails();

    std::cout << "----------------\n";

    user2->displayDetails();

    delete product1;
    delete product2;
    delete user1;
    delete user2;

    return 0;
}
