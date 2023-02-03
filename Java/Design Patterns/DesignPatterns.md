# Design Patterns

## Creational Design Patterns

Creational patterns deal with creation of objects from classes.

### Builder Pattern

Builder pattern has four components namely Director,Builder, ConcreteBuilder and Product. A main use-case of the builder pattern is when we need to create immutable objects in which case the Builder is defined inside the Product class and the product class has setters only with private access.

<img src="./builder.png">

```java
package com.example.pricipleexample.builder;

public class Product {
    private int id;

    private String name;

    public int getId() {
        return id;
    }

    private void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    private void setName(String name) {
        this.name = name;
    }

    public static ProductBuilder getProductBuilder() {
        return new ProductConcreteBuilder();
    }

    public static interface ProductBuilder {
    
        ProductBuilder withId(int productId);
        ProductBuilder withProductName(String productName);
    
        void build();
    
        Product getProduct();
    }

    public static class ProductConcreteBuilder implements ProductBuilder {
        private String productName;
        private int id;
    
        private Product product;
        @Override
        public ProductBuilder withId(int productId) {
            this.id = productId;
            return null;
        }
    
        @Override
        public ProductBuilder withProductName(String productName) {
            this.productName = productName;
            return this;
        }
    
        @Override
        public void build() {
            this.product = new Product();
            product.setName(this.productName);
            product.setId(this.id);
        }
    
        @Override
        public Product getProduct() {
            return product;
        }
    }
}
```

The various methods in the Builder class return the builder instance itself so that we can use function chaining. The builder interface is optional.

#### Pitfalls

* Function chaining might be hard for begineers
* Partially initialized objects could be created if not used properly.

## Structural Design Patterns

## Behavioural Design Patterns

## Reference
