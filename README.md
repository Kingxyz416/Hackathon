
import streamlit as st

# Sample product data with image URLs
products = [
    {"name": "Apple", "price": 10.99, "category": "Fruits", "image_url": "https://subzfresh.com/wp-content/uploads/2022/04/apple_158989157.jpg"},
    {"name": "Banana", "price": 19.99, "category": "Fruits", "image_url": "https://images.everydayhealth.com/images/diet-nutrition/all-about-bananas-nutrition-facts-health-benefits-recipes-and-more-rm-722x406.jpg"},
    {"name": "Cherry", "price": 5.99, "category": "Fruits", "image_url": "https://www.gigadocs.com/blog/wp-content/uploads/2020/02/cherry.jpg"},
    {"name": "Laptop", "price": 999.99, "category": "Electronics", "image_url": "https://m.media-amazon.com/images/W/MEDIAX_792452-T2/images/I/71jG+e7roXL._AC_UF1000,1000_QL80_.jpg"},
]

# Shopping cart to keep track of selected items
shopping_cart = []

# Streamlit app
def main():
    st.set_page_config(page_title="Shop Swiftly", page_icon="🛒")

    st.title("SHOP SWIFTLY")

    # Display product categories
    categories = set(product["category"] for product in products)
    selected_category = st.selectbox("Select Category", ["All"] + list(categories))

    # Display products based on category
    st.header("Products")

    selected_items = []
    for product in products:
        if selected_category == "All" or product["category"] == selected_category:
            col1, col2 = st.columns([1, 3])  # Adjust the column ratios based on your preference
            quantity = col1.number_input(f"Quantity for {product['name']} (${product['price']:.2f} each)", value=1, min_value=0, step=1)
            
            # Display product image
            col2.image(product["image_url"], caption=product["name"], use_column_width=True)

            if quantity > 0:
                selected_items.extend([product.copy() for _ in range(int(quantity))])

    # Add selected items to the cart
    if st.button("Add Selected Items to Cart") and selected_items:
        for item in selected_items:
            add_item_to_cart(item)
            st.success(f"{item['name']} (Price: ${item['price']:.2f}) added to cart!")

    # Display shopping cart
    st.header("Shopping Cart")

    if not shopping_cart:
        st.warning("Your cart is empty.")
    else:
        total_cost = sum(item["price"] * item["quantity"] for item in shopping_cart)
        with st.expander("View Cart Details"):
            st.write("### Cart Items:")
            for item in shopping_cart:
                st.write(f"- {item['name']} (Quantity: {item['quantity']}, Total: ${item['price'] * item['quantity']:.2f})")
            st.write(f"### Total Cost: ${total_cost:.2f}")

def add_item_to_cart(product):
    # Check if the item is already in the cart
    for item in shopping_cart:
        if item["name"] == product["name"]:
            item["quantity"] += 1
            return

    # If not, add the item to the cart with the specified quantity
    shopping_cart.append({"name": product["name"], "price": product["price"], "quantity": 1, "image_url": product["image_url"]})

if __name__ == "__main__":
    main()
