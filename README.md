# json-server
1, Create .json file with object containing array of objects.
   eg:
      {
        "products": [
          {
            "id": 1,
            "title": "Product 1",
            "category": "electronics",
            "price": 4000,
            "description": "This is description about product 1"
          },
          {
            "id": 2,
            "title": "Product 2",
            "category": "electronics",
            "price": 2000,
            "description": "This is description about product 2"
          },
          {
            "id": 3,
            "title": "Product 3",
            "category": "books",
            "price": 1000,
            "description": "This is description about product 3"
          },
          {
            "id": 4,
            "title": "Product 4",
            "category": "fitness",
            "price": 3000,
            "description": "This is description about product 4"
          },
          {
            "id": 5,
            "title": "Product 5",
            "category": "fitness",
            "price": 4000,
            "description": "This is description about product 5"
          },
          {
            "id": 6,
            "title": "Product 6",
            "category": "gardening",
            "price": 5000,
            "description": "This is description about product 6"
          },
          {
            "id": 7,
            "title": "Product 7",
            "category": "furniture",
            "price": 6000,
            "description": "This is description about product 7"
          },
          {
            "id": 8,
            "title": "Product 8",
            "category": "furniture",
            "price": 7000,
            "description": "This is description about product 8"
          },
          {
            "id": 9,
            "title": "Product 9",
            "category": "accessories",
            "price": 4000,
            "description": "This is description about product 9"
          },
          {
            "id": 10,
            "title": "Product 10",
            "category": "electronics",
            "price": 3000,
            "description": "This is description about product 10",
            "discount": {
              "type": "shipping"
            }
          },
          {
            "id": 11,
            "title": "Product 8",
            "category": "furniture",
            "price": 7000,
            "description": "This is description about product 8"
          },
          {
            "id": 12,
            "title": "Product 9",
            "category": "accessories",
            "price": 4000,
            "description": "This is description about product 9"
          },
          {
            "id": 13,
            "title": "Product 13",
            "category": "accessories",
            "price": 4000,
            "description": "This is description about product 9"
          }
        ],
        "reviews": [
          {
            "id": 1,
            "rating": 3,
            "comment": "Review 1 for product id 1",
            "productId": 1
          },
          {
            "id": 2,
            "rating": 4,
            "comment": "Review 2 for product id 1",
            "productId": 1
          },
          {
            "id": 3,
            "rating": 4,
            "comment": "Review 3 for product id 1",
            "productId": 1
          },
          {
            "id": 4,
            "rating": 5,
            "comment": "Review 1 for product id 2",
            "productId": 2
          },
          {
            "id": 5,
            "rating": 3,
            "comment": "Review 1 for product id 3",
            "productId": 3
          }
        ]
      }

2, Initiate --> npm init -y

3, Install json-server
    * npm i json-server 
    
4, Create script in package.json, to start & monitor json-server 
     "scripts": {
         "test": "echo \"Error: no test specified\" && exit 1",
------>  "serve-json": "json-server --watch db.json"
     },

5, Run the json-server 
    npm run serve-json 
        Home - http://localhost:3000 
        List of Products - http://localhost:3000/products
        List of Reviews - http://localhost:3000/reviews 
        specific Product - http://localhost:3000/products/1
        specific Review - http://localhost:3000/reviews/3

Configuration:
--------------
    1, set Custom Port number:
    --------------------------
        * Open package.json, inside scripts--> serve-json : 'josn-serve --watch db.json --port 4000'

    2, Custom Route
    ----------------
        * create routes.json file 
        * Include the route method that you need in object.
            (note: Custom routes must have unique path to route)
            {
                "api/v1/*":"/$1",
                "/products/:category": "/products?category=:category",
                "/products/p/:price": "/products?price=:price"
            }
        * Include the Custom Route to your scripts in package.json 
            serve-json : 'josn-serve --watch db.json --port 4000 --routes routes.json'

6,Filter method:
----------------
    Access the nested properties of an json.
    

    * http://localhost:3000/products?category=electronics
        This will filter the products with the category of electronics.

    * http://localhost:3000/products?category=electronics&discount.type=shipping
        This will filter the products with the category of electronics,
        that contain discount of type:shipping.

7, Sort Ascending & Descending:
------------------------------- 
    * In this json we sort the objects based on the 'price' key.
        Ascending(default) - http://localhost:3000/products?_sort=price 
        Descending - http://localhost:3000/products?_sort=price&_order=desc 
    
    * second level of sorting based on the second key(category).
        http://localhost:3000/products?_sort=price,category&_order=desc,asc

8, Pagination:
--------------
    * display the limited num.of objects in a page
    * default page limit is 10-objects, it display 1st 10 objects of the array.
            * display 1st 10 objects.
                http://localhost:3000/products?_page=1

            * display next 10 objects.
                http://localhost:3000/products?_page=2
    
    * set limit of objects in page 

        * display the first 3 objects.
            http://localhost:3000/products?_page=1&_limit=3

        * display the next 3 objects.(_page=2)
            http://localhost:3000/products?_page=2&_limit=3

9, Operator:
------------
    >= - _gte (greater-Than-or-EqualTo)
        http://localhost:3000/products?price_gte=2000
    
    <= - _lte (less-then-or-equal-to)
        http://localhost:3000/products?price_lte=6000

    Range()
    -------
        filter within range of items.
            http://localhost:3000/products?price_gte=2000&price_lte=6000

    _ne - notEqual(!=)
        Removes the object form the array.
            http://localhost:3000/products?id_ne=1
        Removes all price = 4000,3000
            http://localhost:3000/products?price_ne=4000&price_ne=3000
    
    _like - used to filter 
        * it filters the category name that starts with 'f'
            http://localhost:3000/products?category_like=^f     

10, full text search
--------------------
    It search the 'text' in value,
    all over the Json and then filter the result;
        http://localhost:3000/products?q=in
    
        * it will filter all the object whose value, that contain a string of 'in'

11, Relationship:
-----------------
    parent & child Relationship.
    
    * _embed
    --------
        * Used to fetch child resources.

        http://localhost:3000/products?_embed=reviews

        * it will attach(include) all reviews for the each products.

        * to return particular product and their all reviews
            http://localhost:3000/products/1?_embed=reviews
            {
                "id": 1,
                "title": "Product 1",
                "category": "electronics",
                "price": 4000,
                "description": "This is description about product 1",
                "reviews": [
                    {
                        "id": 1,
                        "rating": 3,
                        "comment": "Review 1 for product id 1",
                        "productId": 1
                    },
                    {
                        "id": 2,
                        "rating": 4,
                        "comment": "Review 2 for product id 1",
                        "productId": 1
                    },
                    {
                        "id": 3,
                        "rating": 4,
                        "comment": "Review 3 for product id 1",
                        "productId": 1
                    }
                ]
            }, 
    
    _expand
    --------
        * Used to fetch parent resources
            * http://localhost:3000/reviews?_expand=product
                here the _expand = 'product' and not 'products' , because the each child has only one parent.
            
            * it return each reviews(child) with product(parent) attached to it.

12, POST - create 
-----------------
    we can create new product & insert to our db.json

    Using ThunderClient in VsCode.
        * open ThunderClient and create New Request
        * select POST method & give the valid URL(http://localhost:3000/products)
        * select the body & write the object that you want to create 
        * Click the send button to see the response.
            if(success){
                new product is added
            } else {
                error code.
            }
        * open you db.json file to see the new products is inserted.

13, PUT - update 
-----------------
    (note: in PUT method it update the whole object)
    * we can update the product using its 'id'
    * select PUT method & give the valid URL( http://localhost:3000/products/14 )
    * select the body & make the changes to the object you want to update.
    * Click the send button to see the response.
        if(success){
            new product is added
        } else {
            error code.
        }
    * open you db.json file to see the update.

14, PATCH - update specific 'key:value'
--------------------------------------- 
    (note: used to update specific value in object)
    * we can update the product using its 'id'
    * select PATCH method & give the valid URL( http://localhost:3000/products/14 )
    * select the body & make the changes to the object you want to update.
    * Click the send button to see the response.
        if(success){
            new product is added
        } else {
            error code.
        }
    * open you db.json file to see the update.
        eg: update the price value to 9000 for the product of id=14
            {
                "price": "9000"
            }

15, DELETE - delete the product(obj)
    * we can simply delete the product using it's 'id'
    * select DELETE method & give the valid URL( http://localhost:3000/products/14 )
    * Click the send button to see the response.
        if(success){
            new product is added
        } else {
            error code.
        }
    * open you db.json file to see the product is deleted or not.

16, Generate Random Data
------------------------
    * we are going to create Random products(obj) using .js file 
    * create .js file 
        use --> module.exports = () => {}
        * create the const {obj} to store the random Data in a array
            const data = {
                porducts:[],
            }
        * Use for loop to add the data count.
        for (let i=0; i<1000; i++){
            data.products.push(
                {
                    id: i,
                    title: `Product ${i}`
                }
            )
        }
        * finally return the const.
            module.exports = () => {
                const data = {
                    products:[],
                }
                for(let i=0; i<1000; i++){
                    data.products.push(
                        {
                            id: i,
                            title: `Product ${i}`
                        }
                    )
                }
                return data 
            } 
    * now create the run command in package.json 
        (note: pass the .js file)
        "serve-json1": "json-server --watch GenerateRandomData.js --port 5000"
