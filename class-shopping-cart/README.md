# โจทย์

จาก Class ShoppingCart นี้จงค้นหาและอธิบาย Bug ที่เกิดขึ้นอย่างละเอียด และให้ทำการแก้ไขโค๊ดในส่วนที่เป็น Bug ภายใน class นี้ให้ทำงานให้ถูกต้องนี้

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
}

interface CartItem extends Product {
  quantity: number;
}

class ShoppingCart {
  private items: CartItem[] = [];
  private taxRate: number = 0.07;

  public addItem(item: Product, quantity: number): void {
    const existingItemIndex = this.items.findIndex((i) => i.id === item.id);
    if (existingItemIndex !== -1) {
      this.items[existingItemIndex].quantity += quantity;
    } else {
      this.items.push({ ...item, quantity });
    }
  }

  public removeItem(itemId: string): void {
    this.items = this.items.filter((item) => item.id !== itemId);
  }

  public calculateSubtotal(): number {
    return this.items.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0
    );
  }

  public calculateTax(subtotal: number): number {
    return subtotal * this.taxRate;
  }

  public calculateTotal(): number {
    const subtotal = this.calculateSubtotal();
    const tax = this.calculateTax(subtotal);

    return Math.floor((subtotal + tax) * 100) / 100;
  }

  public getCartSummary(): string {
    const subtotal = this.calculateSubtotal();
    const tax = this.calculateTax(subtotal);
    const total = this.calculateTotal();

    return `Subtotal: $${subtotal.toFixed(2)}, Tax: $${tax.toFixed(
      2
    )}, Total: $${total.toFixed(2)}`;
  }

  public applyDiscount(discountPercentage: number): void {
    this.items.forEach((item) => {
      item.price *= 1 - discountPercentage / 100;
    });
  }
}

// Usage example
const cart = new ShoppingCart();

cart.addItem({ id: "1", name: "Laptop", price: 999.99, quantity: 1 });
cart.addItem({ id: "2", name: "T-Shirt", price: 19.99, quantity: 2 });

console.log(cart.getCartSummary());

cart.applyDiscount(10); // Apply 10% discount
console.log(cart.getCartSummary());
```
