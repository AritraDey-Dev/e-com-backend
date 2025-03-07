## 💾 Database schemas

#### Admin schema

| **Field** | **Type** | **Required** | **Unique** | **Default** |
| --------- | -------- | ------------ | ---------- | ----------- |
| name      | String   | True         | False      | -           |
| email     | String   | True         | True       | -           |
| password  | String   | True         | False      | -           |
| privilege | String   | False        | False      | low         |

#### Product schema

| **Field**       | **Type**                                                                                      | **Required** | **Unique** | **Default** |
| --------------- | --------------------------------------------------------------------------------------------- | ------------ | ---------- | ----------- |
| name            | String                                                                                        | True         | False      | -           |
| description     | String                                                                                        | True         | False      | -           |
| price           | Number                                                                                        | True         | False      | -           |
| rating          | Number                                                                                        | False        | False      | 0           |
| images          | Object.<{<br> public_id: String,<br> url: String,<br> }>                                      | True         | False      | -           |
| colors          | Array.<{String}>                                                                              | True         | False      | -           |
| sizes           | Array.<{String}>                                                                              | True         | False      | -           |
| company         | String                                                                                        | True         | False      | -           |
| stock           | Number                                                                                        | True         | False      | -           |
| numberOfReviews | Number                                                                                        | False        | False      | 0           |
| reviews         | Object.<{<br> name: String,<br> email: String,<br> rating: Number,<br> comment: String<br> }> | False        | False      | -           |
| shipping        | Boolean                                                                                       | False        | False      | True        |
| featured        | Boolean                                                                                       | False        | False      | False       |
| admin           | Schema.Admin                                                                                  | True         | False      | -           |
| createdAt       | Date                                                                                          | False        | False      | Date.now()  |

#### Order schema

| **Field**     | **Type**                                                                                                                                                       | **Required** | **Unique** | **Default** |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------- | ----------- |
| shippingInfo  | Object.<{<br> address: String,<br> city: String, <br> state: String,<br> country: String,<br> pinCode: Number,<br> phoneNumber: Number<br>}>                   | True         | False      | -           |
| orderItems    | Object.<{<br> name: String,<br> price: String,<br> quantity: Number,<br> image: String,<br> color: String,<br> size: String,<br> product: Schema.Product<br>}> | True         | False      | -           |
| user          | Object.<{<br> name: String,<br> email: String<br>}>                                                                                                            | True         | False      | -           |
| paymentInfo   | Object.<{<br> id: String, <br> status: String<br>}>                                                                                                            | True         | False      | -           |
| paidAt        | Date                                                                                                                                                           | True         | False      | -           |
| itemsPrice    | Number                                                                                                                                                         | True         | False      | 0           |
| shippingPrice | Number                                                                                                                                                         | True         | False      | 0           |
| totalPrice    | Number                                                                                                                                                         | True         | False      | 0           |
| orderStatus   | String                                                                                                                                                         | True         | False      | processing  |
| createdAt     | Date                                                                                                                                                           | False        | False      | Date.now()  |
| deliveredAt   | Date                                                                                                                                                           | False        | False      | -           |

## 🌍 APIs

#### Products

| **Method** | **Route**                  | **Parameters** | **Body**                                                                                                                                                                                                   | **Description**                                                       |
| ---------- | -------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| GET        | /api/products/             | -              | -                                                                                                                                                                                                          | Get list of all available products                                    |
| POST       | /api/products/             | id             | -                                                                                                                                                                                                          | Get details of a single product                                       |
| POST       | /api/admin/product/new     | -              | {<br>name: String,<br>price: Number,<br>description: String,<br>category: String,<br>images: Array,<br>colors: Array,<br>sizes: Array,<br>company: String,<br>shipping: Boolean,<br>featured: Boolean<br>} | Creates a new product                                                 |
| PUT        | /api/admin/product/        | id             | {<br>name: String,<br>price: Number,<br>description: String,<br>category: String,<br>images: Array,<br>colors: Array,<br>sizes: Array,<br>company: String,<br>shipping: Boolean,<br>featured: Boolean<br>} | Update existing products detail                                       |
| DELETE     | /api/admin/product/        | id             | -                                                                                                                                                                                                          | Deletes an existing product                                           |
| GET        | /api/products/reviews/     | id             | -                                                                                                                                                                                                          | Get list of reviews of an existing product.                           |
| POST       | /api/products/reviews/     | -              | {<br>name: String,<br>email: String,<br>rating: Number,<br>comment: String,<br>productId: String,<br>}                                                                                                     | - Creates a product review.<br>- Updates and already existing review. |
| DELETE     | /api/admin/product/review/ | id             | {<br>reviewId: String<br>}                                                                                                                                                                                 | Delete a review for an existing product.                              |

#### Orders

| **Method** | **Route**         | **Parameters** | **Body**                                                                                                                                                                                 | **Description**                |
| ---------- | ----------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| GET        | /api/admin/order/ | -              | -                                                                                                                                                                                        | Get all orders                 |
| POST       | /api/orders/      | -              | {<br>email: String<br>}                                                                                                                                                                  | Get orders of a single user    |
| GET        | /api/orders/      | id             | -                                                                                                                                                                                        | Get a single order             |
| POST       | /api/orders/new/  | -              | {<br>name: String,<br>email: String,<br>shippingInfo: Object,<br>orderItems: Array.,<br>paymentInfo: Object,<br>itemsPrice: Number,<br>shippingPrice: Number,<br>totalPrice: Number<br>} | Creates a new order            |
| PUT        | /api/admin/order/ | id             | {<br>status: String<br>}                                                                                                                                                                 | Update existing order's status |
| DELETE     | /api/admin/order/ | id             | -                                                                                                                                                                                        | Delete an existing order       |

#### Admin

| **Method** | **Route**            | **Parameters** | **Body**                                                                            | **Description**                  |
| ---------- | -------------------- | -------------- | ----------------------------------------------------------------------------------- | -------------------------------- |
| POST       | /api/admin/register/ | -              | {<br>name: String,<br>email: String,<br>password: String,<br>privilege: String<br>} | Creates a new admin user         |
| POST       | /api/admin/login/    | -              | {<br>email: String,<br>password: String<br>}                                        | Login to admin dashboard         |
| GET        | /api/admin/users/    | -              | -                                                                                   | Get list of all admin users      |
| GET        | /api/admin/users/    | id             | -                                                                                   | Get single admin details         |
| PUT        | /api/admin/users/    | id             | {<br>privilege: String<br>}                                                         | Update an admin user's privilege |
| DELETE     | /api/admin/users/    | id             | -                                                                                   | Delete an existing admin user    |

## 🧾 Description

TomperWear is an E-commerce platform for small bussiness owners who want to expand their bussiness by providing an online purchase solution to their customers.

## ✨ Features

#### Client

Users shopping through this platform enjoys following features:

- [x] User authentication using google firebase.
- [x] Secure payments via stripe.
- [x] Users can change their username and display image.
- [x] Users can filter products on various parameters.
- [x] Users can sort products according to price and name.
- [x] Users can add products to cart.
- [x] Users can order products by providing their shipping information and card details for payments.
- [x] Users can view their order's status and their previous orders.
- [x] Users can review the products (or update their previous reviews) and benefit other customers.
- [x] Users can send feedbacks to the owner.
- [x] Responsive for all screen sizes.

#### Admin

The admin panel of TomperWear contains three classes of admins:

**1. Super privileged admin**
Super privileged admins are the topmost in the hierarchy. They have the following permissions:

- [x] Can view, create, update, and delete admin users.
- [x] Can view, create, update, and delete products.
- [x] Can view orders and update their status.
- [x] Can delete orders.

This is privilege is ideal for the owner(s) of the bussiness.

**2. Moderate privileged admin**
Moderate privileged admins comes below Super privileged admins. They have the following permissions:

- [ ] Can view, create, update, and delete admin users.
- [x] Can view, create, update, and delete products.
- [x] Can view orders and update their status.
- [x] Can delete orders.

This is privilege is ideal for the manager(s).

**3. Low privileged admin**
Low privileged admins are the lowermost in the hierarchy and have the least amount of privileges. The have the following permission:

- [ ] Can view, create, update, and delete admin users.
- [ ] Can view, create, update, and delete products.
- [x] Can view orders and update their status.
- [ ] Can delete orders.

This is privilege is ideal for the delivery-agent(s).

**Here is a table dipicting the admin privileges for better clarity**

<table>
<thead>
  <tr>
    <th> </th>
    <th colspan="4">Admins</th>
    <th colspan="4">Product</th>
    <th colspan="3">Orders</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td></td>
    <td><b><u>view</u></b></td>
    <td><b><u>create</u></b></td>
    <td><b><u>update</u></b></td>
    <td><b><u>delete</u></b></td>
    <td><b><u>view</u></b></td>
    <td><b><u>create</u></b></td>
    <td><b><u>update</u></b></td>
    <td><b><u>delete</u></b></td>
    <td><b><u>view</u></b></td>
    <td><b><u>update</u></b></td>
    <td><b><u>delete</u></b></td>
  </tr>
  <tr>
    <td><b>Super</b></td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
  </tr>
  <tr>
    <td><b>Moderate</b></td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
    <td>✔</td>
  </tr>
  <tr>
    <td><b>Low</b></td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>✔</td>
    <td>✔</td>
    <td>-</td>
  </tr>
</tbody>
</table>



## 🛠 Installation and setup

1. Clone the repo to your local machine.
2. Install the required dependency for server using :

   ```javascript
   npm install
   ```

3. Create a .env file inside the root folder and provide the following environment variables:

   ```env
   STRIPE_SECRET_KEY=<stripe_secret_key>
   DB_URI=<mongo_uri>
   JWT_SECRET=<your_jwt_secret>
   JWT_EXPIRE=5d
   COOKIE_EXPIRE=5
   CLOUDINARY_CLOUD_NAME=<cloudinary_cloud_name>
   CLOUDINARY_API_KEY=<cloudinary_api_key>
   CLOUDINARY_API_SECRET=<cloudinary_api_secret>
   ```

4. Start the dev server using :

   ```javascript
   npm start
   ```



