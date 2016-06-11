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
