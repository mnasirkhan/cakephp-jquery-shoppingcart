# cakephp-jquery-shoppingcart
Table Of Content

    Preparation
    Database design
    Install Bootstrap
    Edit default layout file
    The end

1. Preparation

    Download Bootstrap framework from this URL. We are using Bootstrap 3.0.2 in this series for the front-end design.
    Extract downloaded zip file and remove original CSS and JavaScript files, we only need to keep their minified version.
    Download CakePHP from this URL. We are using CakePHP 2.4.3 for this tutorial.
    Create a database. We have created a database called "cakephp_shopping_cart". You can name the database based on your preference.
    Setup CakePHP properly, make sure it can connect to database. If you are not familiar with setting up, read this page first. It will go through steps of setting up a basic CakePHP application.
    Download "s4.jpg", "comment.jpg" and "note3.jpg" to CakePHP's directory "app/webroot/img/". These three images are used for the products' photos.

2. Database design

Design a very simple database table "products" as below. It stores products' information. The idea here is to demonstrate how CakePHP Model works with database table.

[cap forum erd]

Import the database table via following codes:
?
1
2
3
4
5
6
7
8
	
CREATE TABLE products
(
id INT(11) NOT NULL AUTO_INCREMENT,
name VARCHAR(250) NOT NULL,
price FLOAT NOT NULL,
image VARCHAR(250) NOT NULL,
PRIMARY KEY (id)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8;

Insert some sample products data as well. As you can see, we are not going to build a backend system to manage products in this series. Some sample products data are needed.
?
1
2
3
	
INSERT INTO `products` (`id`, `name`, `price`, `image`) VALUES (NULL, 'Samsung Galaxy S4', '600', '/img/s4.jpg');
INSERT INTO `products` (`id`, `name`, `price`, `image`) VALUES (NULL, 'Samsung Galaxy Note 3', '500', '/img/note3.jpg');
INSERT INTO `products` (`id`, `name`, `price`, `image`) VALUES (NULL, 'Samsung Comment 3', '400', '/img/comment.jpg');
3. Install Bootstrap

In this step, we will place Bootstrap resources accordingly to CakePHP application.

    Copy "fonts" folder from downloaded Bootstrap files to CakePHP's directory "app/webroot".
    Copy "bootstrap.min.css" file from downloaded Bootstrap files to CakePHP's directory "app/webroot/css".
    Copy "bootstrap.min.js" file from downloaded Bootstrap files to CakePHP's directory "app/webroot/js".

Now the "app/webroot" directory should look similar as below:

[CakePHP shopping cart]

The highlighted parts are what we have added.
4. Edit default layout file

Now if you navigate to the installed CakePHP application, you should view a simple page as below, which indicates CakePHP has been installed properly.

[CakePHP shopping cart webroot]

In this step, we need to edit the "default.ctp" layout file. Specifically we need to load the Bootstrap resources (JavaScript and CSS files). And construct a simple layout for our shopping cart.

After editting, "app/View/Layouts/default.ctp" should look like below:
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
	
<!DOCTYPE html>
<html>
<head>
    <?php echo $this->Html->charset(); ?>
    <title>
        CakePHP Shopping Cart
    </title>
    <?php
        echo $this->Html->meta('icon');
 
        echo $this->Html->css('bootstrap.min.css');
 
        echo $this->fetch('meta');
        echo $this->fetch('css');
        echo $this->fetch('script');
    ?>
    <style>
    body {
      padding-top: 20px;
      padding-bottom: 20px;
    }
     
    .navbar {
      margin-bottom: 20px;
    }
    </style>
 
    <script src="https://code.jquery.com/jquery-1.10.2.min.js"></script>
 
</head>
<body>
 
<div class="container">
     
    <nav class="navbar navbar-default" role="navigation">
      <!-- Brand and toggle get grouped for better mobile display -->
      <div class="navbar-header">
        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="#">CakePHP Shopping Cart</a>
      </div>
     
      <!-- Collect the nav links, forms, and other content for toggling -->
      <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
        <ul class="nav navbar-nav navbar-right">
          <li>
            <?php echo $this->Html->link('<span class="glyphicon glyphicon-shopping-cart"></span> Shopping Cart <span class="badge" id="cart-counter">0</span>',
                                        array('controller'=>'carts','action'=>'view'),array('escape'=>false));?>
          </li>
        </ul>
      </div><!-- /.navbar-collapse -->
    </nav>
     
    <?php echo $this->fetch('content'); ?>
     
</div>
     
    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <?php echo $this->Html->script('bootstrap.min'); ?>
</body>
</html>

You have probably noticed, this is a very simple layout file. It has a top navigation bar with a content area. Pay attention to the right corner shopping cart icon. We have assigned an element ID "cart-counter" to it. We will use this ID to identify and update its value later in the application.

Now the application's default page should look alike below:
[CakePHP shopping cart webroot]
5. The end

In part 2 of this series. We are going to implement the core part of shopping cart. Displaying products from database, adding product to cart as well as updating shopping cart. Stay tuned!

Hopefully this simple tutorial helped you with your development.
If you like our post, please follow us on Twitter and help spread the word. We need your support to continue.
If you have questions or find our mistakes in above tutorial, do leave a comment below to let us know. 


This is the part 2 of 2 in Build a shopping cart with CakePHP and jQuery.

    Build a shopping cart with CakePHP and jQuery (part 1)
    Build a shopping cart with CakePHP and jQuery (part 2)

    In this tutorial, we are going to implement functions for displaying products, adding product to shopping car as well as update shopping cart. At the end of this tutorial, a download link is provided for downloading the full source code.

    Let us get started. 

Download link of entire source code is provided at the end of this tutorial.
Table Of Content

    Display proudcts and view product's detail
    Add product to shopping cart
    Update shopping cart
    Wrap it up
    Download
    The end

1. Display proudcts and view product's detail

In part 1, we have inserted some sample data to "products" table. Now let us build corresponding controller, model and view for "products" table.

Create model file "app/Model/Product.php" with code below:
?
1
2
3
4
5
6
	
<?php
App::uses('AppModel', 'Model');
class Product extends AppModel {
     
}
?>

"Product" class does not do anything specific; it is currently just a placeholder. In case you need to add additional features to this application, you may want to add more functions to this class.

Create model file "app/Controller/ProductsController.php" with code below:
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
	
<?php
App::uses('AppController', 'Controller');
 
class ProductsController extends AppController {
 
    public function index() {
            $this->set('products', $this->Product->find('all'));
    }
             
    public function view($id) {
            if (!$this->Product->exists($id)) {
                    throw new NotFoundException(__('Invalid product'));
            }
                 
            $product = $this->Product->read(null,$id);
            $this->set(compact('product'));
    }  
}
?>

There are two functions in the controller class. Each of them serves a view of thier own.

    public function index(): This functions simply gets all products' data and set it to its corresponding view file.
    public function view(): This function finds a product's detail data and set it to its corresponding view file.

For each controller's action, we need a view file.

Create view file "app/View/Products/index.ctp" with code below, this is the view file for displaying all products.
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
	
<div class="row">
    <?php foreach ($products as $product):?>
    <div class="col-sm-6 col-md-4">
        <div class="">
            <?php echo $this->Html->link($this->Html->image($product['Product']['image']),
                    array('action'=>'view',$product['Product']['id']),
                    array('escape'=>false,'class'=>'thumbnail'));?>
            <div class="caption">
                <h5>
                    <?php echo $product['Product']['name'];?>
                </h5>
                <h5>
                    Price: $
                    <?php echo $product['Product']['price'];?>
                </h5>
            </div>
        </div>
    </div>
    <?php endforeach;?>
</div>

Create view file "app/View/Products/view.ctp" with code below, this is the view file for viewing a product's detail.
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
	
<div class="row">
    <div class="col-lg-12">
        <ol class="breadcrumb">
            <li><?php echo $this->Html->link('Home','/');?>
            </li>
            <li class="active"><?php echo $product['Product']['name'];?>
            </li>
        </ol>
    </div>
</div>
 
<div class="row">
    <div class="col-lg-4 col-md-4">
        <?php echo $this->Html->image($product['Product']['image'],array('class'=>'thumbnail'));?>
    </div>
 
    <div class="col-lg-8 col-md-8">
        <h1>
            <?php echo $product['Product']['name'];?>
        </h1>
        <h2>
            Price: $
            <?php echo $product['Product']['price'];?>
        </h2>
        <p>
            <?php echo $this->Form->create('Cart',array('id'=>'add-form','url'=>array('controller'=>'carts','action'=>'add')));?>
            <?php echo $this->Form->hidden('product_id',array('value'=>$product['Product']['id']))?>
            <?php echo $this->Form->submit('Add to cart',array('class'=>'btn-success btn btn-lg'));?>
            <?php echo $this->Form->end();?>
        </p>
    </div>
</div>

Now we have created the file views we needed for this section. However navigate to index page of the shopping cart. It still gives us the CakePHP's default page. What we want is that, the index page should display all products. Adding a routing rule to the Router can do this.

Open app/Config/routes.php, and add following line to the beginning of the file:
?
1
	
Router::connect('/', array('controller' => 'products', 'action' => 'index'));

This tells the shopping cart to show "products/index" page by default.

Now you should be see following page.

[cap forum erd]

Clicking on the thumbnail image, it should show you below.

[cap forum erd]
2. Add product to shopping cart

You should probably notice that, the "Add to cart" button is not yet working. That is what we are going to fix in this section.

Since we do not have a database table to store shopping cart's items. We are going to use Session. Let us create a powerful "Cart" model, which we will be using to represent the shopping cart. It is a model without database table.
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
	
<?php
App::uses('AppModel', 'Model');
App::uses('CakeSession', 'Model/Datasource');
 
class Cart extends AppModel {
 
    public $useTable = false;
     
    /*
     * add a product to cart
     */
    public function addProduct($productId) {
        $allProducts = $this->read();
        if (null!=$allProducts) {
            if (array_key_exists($productId, $allProducts)) {
                $allProducts[$productId]++;
            } else {
                $allProducts[$productId] = 1;
            }
        } else {
            $allProducts[$productId] = 1;
        }
         
        $this->saveProduct($allProducts);
    }
     
    /*
     * get total count of products
     */
    public function getCount() {
        $allProducts = $this->readProduct();
         
        if (count($allProducts)<1) {
            return 0;
        }
         
        $count = 0;
        foreach ($allProducts as $product) {
            $count=$count+$product;
        }
         
        return $count;
    }
 
    /*
     * save data to session
     */
    public function saveProduct($data) {
        return CakeSession::write('cart',$data);
    }
 
    /*
     * read cart data from session
     */
    public function readProduct() {
        return CakeSession::read('cart');
    }
 
}

Let us divide into this class.

Since this model class is not using any database table, we use "public $useTable = false;" to tell CakePHP not to look for its table.

We are going to store the shopping items into Session using structure as below:
?
1
2
3
4
	
array(
    product_id => number_of_items
    ...
)         

Let us take a look at each function:

    public function addProduct($productId): it takes a $productId and add it to the shopping cart accordingly. It will either increase the item's counter or add a new item to the array.
    public function getCount(): it loop through the array and count number of products in cart.
    public function saveProduct(): a simple wrap function, it stores data to Session using "cart" as the key.
    public function readProduct(): it reads data from Session using "cart" as the key.

Okay, now we have built the "Cart" class. Let us get back to our task of adding product to cart.

Create controller file "app/Controller/CartsController.php" with code below:
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
	
<?php
App::uses('AppController', 'Controller');
 
class CartsController extends AppController {
 
    public $uses = array('Product','Cart');
     
    public function add() {
        $this->autoRender = false;
        if ($this->request->is('post')) {
            $this->Cart->addProduct($this->request->data['Cart']['product_id']);
        }
        echo $this->Cart->getCount();
    }
}

As you can see, public function addProduct() is the action for adding product to cart. This function does not require a view, because we are going to call this functio via ajax. All it does is to return a count of total products in the cart.

We turn off the view file by calling $this->autoRender = false;. And we use "Cart" class functions, which we built previously, to add product and get total products' count.

The last thing we need to do in this step is to add some JavaScript codes to the view.

Open file "app/View/Products/view.ctp", and following piece of JavaScript codes to the end of the file:
?
1
2
3
4
5
6
7
8
9
10
11
	
<script>
$(document).ready(function(){
    $('#add-form').submit(function(e){
        e.preventDefault();
        var tis = $(this);
        $.post(tis.attr('action'),tis.serialize(),function(data){
            $('#cart-counter').text(data);
        });
    });
});
</script>

This piece of JavaScript codes simply calls action "carts/add" and set the counter text correctly.

Now head back to your browser, and clicking "Add to cart" button in product detail page, it should dynamically set the counter on the top right corner correctly.
3. Update shopping cart

The final function we are going to implement is to update shopping cart items. However before that, we need to have a shopping cart page, which lists all current items in the cart.

First let us add a controller's action to CartsController class. Open file "app/Controller/CartsController" and add function below after function "add()":
?
1
2
3
4
5
6
7
8
9
10
11
12
	
public function view() {
    $carts = $this->Cart->readProduct();
    $products = array();
    if (null!=$carts) {
        foreach ($carts as $productId => $count) {
            $product = $this->Product->read(null,$productId);
            $product['Product']['count'] = $count;
            $products[]=$product;
        }
    }
    $this->set(compact('products'));
}

This function loops over items in the shopping cart and find their details.

We need a view file for this action too. Create file "app/Carts/view.ctp" with code below:
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
	
<div class="row">
    <div class="col-lg-12">
        <ol class="breadcrumb">
            <li><?php echo $this->Html->link('Home','/');?>
            </li>
            <li class="active">Cart</li>
        </ol>
    </div>
</div>
 
<?php echo $this->Form->create('Cart',array('url'=>array('action'=>'update')));?>
<div class="row">
    <div class="col-lg-12">
        <table class="table">
            <thead>
                <tr>
                    <th>Product Name</th>
                    <th>Price</th>
                    <th>Quantity</th>
                    <th>Total</th>
                </tr>
            </thead>
            <tbody>
                <?php $total=0;?>
                <?php foreach ($products as $product):?>
                <tr>
                    <td><?php echo $product['Product']['name'];?></td>
                    <td>$<?php echo $product['Product']['price'];?>
                    </td>
                    <td><div class="col-xs-3">
                            <?php echo $this->Form->hidden('product_id.',array('value'=>$product['Product']['id']));?>
                            <?php echo $this->Form->input('count.',array('type'=>'number', 'label'=>false,
                                    'class'=>'form-control input-sm', 'value'=>$product['Product']['count']));?>
                        </div></td>
                    <td>$<?php echo $count*$product['Product']['price']; ?>
                    </td>
                </tr>
                <?php $total = $total + ($count*$product['Product']['price']);?>
                <?php endforeach;?>
 
                <tr class="success">
                    <td colspan=3></td>
                    <td>$<?php echo $total;?>
                    </td>
                </tr>
            </tbody>
        </table>
 
        <p class="text-right">
            <?php echo $this->Form->submit('Update',array('class'=>'btn btn-warning','div'=>false));?>
            <a class="btn btn-success"
                onclick="alert('Implement a payment module for buyer to make a payment.');">CheckOut</a>
        </p>
 
    </div>
</div>
<?php echo $this->Form->end();?>

This file looks big. But its main part is actually just the table. Rest is HTML structure for the design.

Now if you navigate to the shopping cart view page by clicking the cart icon on the top right corner. You should see:

[cap forum erd]

Here comes the last part, open file "app/Controller/CartsController.php" and add following function after function "view()"
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
	
public function update() {
        if ($this->request->is('post')) {
            if (!empty($this->request->data)) {
                $cart = array();
                foreach ($this->request->data['Cart']['count'] as $index=>$count) {
                    if ($count>0) {
                        $productId = $this->request->data['Cart']['product_id'][$index];
                        $cart[$productId] = $count;
                    }
                }
                $this->Cart->saveProduct($cart);
            }
        }
        $this->redirect(array('action'=>'view'));
}

This function is triggered when you click "Update" button on shopping cart page. What it does is that, it loops over product count and add it to the Session. Please note if the product's count is less than 0, it is removed from Session, which means it is removed from shopping cart.
4. Wrap it up

It is almost done. There is only one thing we need to take care of here. If you recall. The top right corner counter is still a static value of 0. We need to modify that piece to reflect the real time items' count.

Open file "app/Controller/AppController.php" and add following function to it:
?
1
2
3
4
5
	
public function beforeFilter() {
    $this->loadModel('Cart');
     
    $this->set('count',$this->Cart->getCount());
}

This function will set the real time items' count to all the view files, which includes layout file. Hence open layout file "app/View/Layouts/default.ctp" and change the static value 0 to $count.
?
1
2
	
<?php echo $this->Html->link('<span class="glyphicon glyphicon-shopping-cart"></span> Shopping Cart <span class="badge" id="cart-counter">'.$count.'</span>',
            array('controller'=>'carts','action'=>'view'),array('escape'=>false));?>
