### **Bug description:**

The Revolut Woocommerce plugin does not fetches the cart when the customer clicks in the PLACE ORDER button, but only fetches it when the checkout fragment is loaded. This conflits with OneClickUpsells, that allows the customer to accept or decline in one click, adding the item to his order, or not.

**Expected behaviour:**
Customer enters checkout with a $29 cart, fills informations, and click PLACE ORDER. A modal shows, and asks customer if he would like to add an $10 upsell to his order. If he refuses, the $29 charge happens, as if there was no upsell. But if customer accepts the upsell, the $10 item is added to the cart, and a $39 charge should happen.

**Current behaviour:**
Declining an upsell doesn't edits the cart, and the $29 charge goes smoothly. Accepting an upsell makes the plugin crash, and customer has to refresh page and enters its details AGAIN.

-----

### **Reproduce the issue:**

Open this URL in your browser and add the product to your cart : [https://weaveweft.how-genius.com/product/cashmere-yarn-wool-tint-2931/](https://weaveweft.how-genius.com/product/cashmere-yarn-wool-tint-2931/)

Once screen greys out, head to [https://weaveweft.how-genius.com/cart/](https://weaveweft.how-genius.com/cart/)

Open a new tab and head to [https://weaveweft.how-genius.com/checkout/](https://weaveweft.how-genius.com/checkout/)

Start to fill out checkout billing details. Use one of the valid [revolut developer test-cards](https://developer.revolut.com/docs/guides/accept-payments/get-started/test-in-the-sandbox-environment/test-cards) to fill out the Revolut card field (4929420573595709 with any date and CVV is considered a valid Visa card)

Let's simulate a One Click Upsell action : In checkout, before clicking the **PLACE ORDER** button, open the browser JavaScript console and execute this JS code to add another Blue Yarn Wool to the cart : `jQuery.post('/?wc-ajax=add_to_cart',{product_id: 1361, variation_id: 1361}, (res) => console.log('added !', {res}));` 

Feel free to refresh the cart tab to check if the Blue Yarn Yool is indeed in the cart, but **do not refresh the checkout page**

Click the **PLACE ORDER** button. 

-----

### **You will obtain the following errors:**

- HTTP error 400 in [https://sandbox-merchant-secure.revolut.com/api/public/orders/<<UUID>>/payments](https://sandbox-merchant-secure.revolut.com/api/public/orders/<<UUID>>/payments)
- A "Something went wrong" error on top of the Woocommerce checkout fragment

-----

### **Why it is a serious issue ?**

- In these economically difficult times, optimising the user experience and customer average order value has become more critical than ever for business to remain profitable and thrive. A website featuring OneClickUpsell often sees his average order value raising by $2 to $5, which often account for a significant part of profits. Sometimes, it is also a feature businesses cannot afford to fade, as they wouldn't be profitable without it. This is especially true for businesses keeping their prices unchanged despite the inflation.

- All other payment gateways plugins, including Stripe and Mollie ones, have no issues managing orders whose cart has been updated while in checkout page. A definitive fix will probably consist in fetching the cart when the **PLACE ORDER** Woocommerce event is triggered (Not an event triggered by the button click, but rather a little later I suppose)

If you need to directly contact me, feel free to raise an issue on this repository on Github, along with an email address if needed.
